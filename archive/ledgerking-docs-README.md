# Accounting Rules (written specification)

These rules are implemented in the Rust core **and** enforced again by SQLite
triggers, so a bug in one layer cannot silently corrupt the books. Every rule
below has an automated test (see `src-tauri/tests/`).

## 1. Money
- All amounts are **integer paise** (`i64`). Floats are never used.
- One entered amount: max ±9,99,99,99,99,99,999.99 (`MAX_ENTRY_PAISE`), enforced by the app and by DB `CHECK`s.
- All arithmetic is checked; overflow is an error, never a wrap (`overflow-checks = true` in release too).
- Amount input is strict: `1250`, `1250.5`, `1250.50`, `-12.30`. Rejected: `1,250`, `.5`, `12.`, `1.234`, `+5`, `1e3`.
- Rounding only through `domain_money` with an explicit mode. Tax rates are in basis points (18% = 1800).
- Sign convention inside the core: **Debit positive, Credit negative**.

## 2. Company and financial year (FY)
- Books beginning date is fixed after creation (all openings derive from it).
- An FY starts on the 1st of a month and lasts 12 months. The first FY contains the books beginning date.
  Each next FY starts the day after the previous one ends (no gaps, no overlaps).
- FY status: `OPEN` → `CLOSED` (admin). Reopen only by admin, with a reason, and only if the next year is still open.
  Years close strictly oldest-first.
- A closed year blocks: new vouchers, edits, posting, cancellation, opening-balance changes.

## 3. Groups and ledgers
- 28 built-in groups (Tally-style) identified by `system_code`; renaming is allowed, deleting/moving is not.
- A sub-group always has its parent's nature (Asset/Liability/Income/Expense) and gross-profit flag. Nature never changes.
- Only Asset/Liability ledgers may have an opening balance. Income/Expense ledgers start every FY at zero.
- Built-in ledgers: **Cash** and **Profit & Loss A/c**.
- A ledger used in any voucher cannot be deleted (mark it inactive). A ledger with an opening balance cannot be deleted.
- A ledger with entries in a closed year or locked period cannot move to a group of another nature / gross-profit class.
- Unbalanced openings are allowed and reported as **"Difference in opening balances"** in Trial Balance and Balance Sheet.
  It is shown on the **balancing side** (as in Tally): if Dr openings exceed Cr openings by X, the line is X **Cr**.
- Trial Balance totals: the Debit/Credit columns total the **period** movements (always equal, self-checked); the
  Closing column shows the total of Dr closing balances and of Cr closing balances (equal, self-checked).

## 4. Voucher states
```
DRAFT --post--> POSTED --cancel--> CANCELLED  (number kept, no effect)
  |                   \--reverse--> REVERSED  (effect kept, offset by a posted reversal Journal)
  \--delete--> removed (drafts have no number and no effect)
```
- Only drafts can be edited or deleted. Posted/cancelled/reversed vouchers and their lines can **never** be changed or deleted
  (DB triggers reject even raw SQL).
- Balances include `POSTED` + `REVERSED`; exclude `DRAFT` + `CANCELLED`.
- **Cancel**: only if the voucher date is in an open year and unlocked period. Reason required.
- **Reverse**: a new Journal voucher with every Dr/Cr swapped is posted on a reversal date ≥ original date. Only the
  reversal date must be open — this is the controlled way to correct a locked month or closed year. Reason required.
- A reversal voucher can itself never be edited, cancelled or reversed.
- Every change uses an optimistic `version` check (a stale screen cannot overwrite a newer change).

## 5. Posting (double entry)
Posting re-validates everything from the stored draft, inside one transaction:
- ≥ 2 lines, ≤ 1000 lines, every amount > 0, at least one Debit and one Credit, **total Debit = total Credit** (exact paise);
- ledgers exist, belong to the company, are active;
- date inside an open FY, not before books beginning, not in a locked period;
- voucher-type rules: **Contra** only cash/bank ledgers; **Payment** must credit a cash/bank ledger;
  **Receipt** must debit a cash/bank ledger; Journal/Sales/Purchase/Notes (accounting mode) no extra rule.

## 6. Voucher numbering (GST-safe)
- Series per voucher type per FY, restarting at 1 each year. Display = prefix + zero-padded number + suffix (e.g. `PY/0007`).
- The number is assigned **at posting**, in the posting transaction: a failed post or a deleted draft never leaves a gap.
- Cancelled vouchers keep their number and stay visible in the Day Book.
- Numbers follow posting order (a back-dated voucher gets the next number; numbers are never re-sequenced).

## 7. Period lock
- Lock any date range (e.g. April 2026). Locks never overlap. Locking fails while drafts exist in the range.
- Inside a locked range: no create, edit, post or cancel. Reports and draft deletion still work.
- Accountant may lock; only Admin may unlock (reason required, audited).
- If a lock covers the books beginning date, opening balances are locked too.

## 8. Year close and carry-forward
- Requirements: year open, previous year closed, no drafts in the year.
- Openings are **derived, never copied**: balance-sheet ledger opening = books-begin opening + all effective entries before the year.
  Profit & Loss A/c additionally carries the net result of all earlier years. Income/expense ledgers restart at zero.
- At close a carry-forward snapshot is stored and immediately verified against the derived next-year openings;
  any mismatch aborts the close. The next FY is created automatically if missing.

## 9. Reports
- Report ranges must lie inside one FY.
- Trial Balance, P&L, Balance Sheet, Ledger report each re-check their own totals (Dr = Cr, assets = liabilities,
  ledger running balance = computed balance). A mismatch returns an **Integrity error with a reference ID** — a wrong
  report is never shown silently.
- P&L: trading part = groups marked "affects gross profit" (Sales, Purchase, Direct Incomes/Expenses).
  Opening/closing stock (FIFO) are part of the trading account.

## 10. Audit, users, errors
- Every master/voucher/lock/year change writes an append-only audit row in the same transaction.
- Roles: Admin (all), Accountant (masters, vouchers, cancel/reverse, lock), Viewer (reports).
- Errors carry a code, a clear reason and a next action; unexpected failures also get a reference ID (`ERR-…`).

## 11. Storage rules
- SQLite with `foreign_keys=ON` (verified), WAL journal, `synchronous=FULL`, `trusted_schema=OFF`, busy timeout.
- Every write = one `BEGIN IMMEDIATE` transaction (all-or-nothing).
- Versioned migrations with checksums; a database from a newer app version is refused; failed migrations roll back fully.
- **The data file must be on a local disk. Keeping it on a network share is not supported.**

## 12. Inventory (owner decisions, 2026-09-30)
- Quantity = integer milli-units (max 3 decimals); each unit fixes its allowed decimals (Nos 0, Kg 3, Mtr 2 ...).
- Quantity on hand and value are **always derived** from stock movements of POSTED/REVERSED vouchers.
- **Costing: FIFO.** Opening stock (books beginning) is the first layer. Order: date, then inward before outward on
  the same date, then entry order. Partly used layers keep `value - consumed`, so value is conserved to the paisa.
- Purchase inward is valued at the **taxable value** (GST input credit is not cost). Sales returns (Credit Note) come
  back at the item's last known cost rate.
- **Negative stock: ALLOWED + warning.** Posting succeeds, but returns a warning for every item that is negative at
  day-end on the voucher date or any later date (back-dated sales included). Stock Summary and Item Register flag
  negative rows (`is_negative`) and count them (`negative_count`) so the UI shows them in **red**.
  A negative position is valued at the last known cost; the next purchase fills it first.
- Trading account: opening stock (value at period start) and closing stock (value at period end). Balance Sheet shows
  closing stock under assets. Trial Balance shows an "Opening Stock" line (value at FY start).
- Opening stock counts in the "difference in opening balances" (enter capital / other openings to match it).
- Opening stock is locked like opening balances (after a year close or a lock over books beginning).
- An item's unit cannot change once it has opening stock or movements; a used item cannot be deleted (mark inactive).

## 13. GST (owner decisions, 2026-09-30)
- Both **tax-exclusive** and **tax-inclusive** prices are supported.
- **Discount first**: Taxable Value = Price - Discount (percent or amount; never more than the line amount).
- **Rounding: 2 decimals, half-up**, per line and per tax component.
- **Intra-state** (company state = place of supply): CGST and SGST, each computed at rate/2, so always **equal halves**.
- **Inter-state**: IGST at the **full rate, never split**.
- Inclusive prices: tax is extracted (`amount x rate / (100% + rate)`, halves for intra-state); taxable = amount - tax,
  so the line total equals the printed price exactly.
- **Invoice round-off to rupee: optional**, half-up, posted separately to the built-in **Round Off** ledger and stored
  on the invoice (`round_off_paise`).
- Place of supply = the party ledger's state unless chosen on the invoice. A taxed invoice without company state or
  place of supply is refused (never guessed).
- Built-in ledgers: Output CGST/SGST/IGST (sales side), Input CGST/SGST/IGST (purchase side), Round Off.
- Entries: Sales Dr Party / Cr Sales, Output GST; Purchase Dr Purchases, Input GST / Cr Party; Credit Note and Debit
  Note are the mirror images. Per-line tax breakup is stored for GST reports; posted invoice data is immutable.
- A posted item invoice is corrected by **Cancel** (open period) or a **Credit Note / Debit Note**, not by reversal.

## 14. Stock Journal
- Stock-only voucher (SJ/0001...): no ledger lines; at least one item line. Number, year, lock, audit, cancel as usual.
- Consumption = outward at FIFO cost. Production can be valued: from consumption (the FIFO cost of everything
  consumed in the same voucher, shared by quantity; last line takes the remainder), a fixed value, or last cost.
- Processing order on a day: valued inwards -> issues -> production, so production always sees its consumption.
- Production cost is derived, so a back-dated purchase before the journal updates it automatically.
- Item invoices and stock journals cannot be reversed; use Cancel (open period) or a Credit/Debit Note.

## 15. Quotations and Sales Orders
- No effect on ledgers or stock (so period locks do not apply); date must be in an OPEN financial year.
- Numbers QT/0001, SO/0001 per year, assigned on creation, gap-free; cancelled numbers stay used.
- OPEN documents may be edited (same financial year, audited, version-checked). CONVERTED / CANCELLED are final.
- Quotation -> Sales Order, or Quotation / Sales Order -> Sales Invoice: whole document (partial delivery later).
  An expired quotation cannot be converted until "valid until" is extended.
- GST computed with the same code as invoices.

## 16. GST returns (data + CSV)
- GSTR-1: B2B (incl. SEZ with/without payment and deemed exports, with their invoice type), B2CL (limit is a
  parameter, default Rs 1,00,000 - confirm with CA), B2CS (net of small credit notes; e-commerce operator sales
  tagged with the operator GSTIN), CDNR, CDNUR (incl. export credit notes as EXPWP / EXPWOP), Exports (WPAY / WOPAY
  with port code and shipping bill), e-commerce operator summary, nil / exempt / non-GST (table 8), HSN summary
  (B2B/B2C), documents issued (incl. cancelled numbers).
- GSTR-3B: 3.1(a), 3.1(b) zero-rated (exports + SEZ), 3.1(c), 3.1(d) inward reverse charge, 3.1(e), 3.2, 4 ITC
  (purchases net of purchase returns, reverse-charge ITC separately), 5, and output minus ITC per head before
  set-off (set-off itself is left to the CA).
- Exports / SEZ: always IGST (inter-state), place of supply not stored for exports. Under LUT/bond the line keeps the
  item rate with zero tax. An export buyer must have no GSTIN; SEZ and deemed exports need one. A credit note
  against an invoice takes the invoice's supply type. Export details are frozen once the invoice is posted.
- Invoice lines store HSN, unit and tax as on the invoice date, so later master edits never change a return.
- Every GSTR-1 build re-checks that all sections and the HSN summary add up to the invoice totals.
- GSTR-3B warns when GST ledgers contain manual journal entries not covered by invoices.
- CSV: offline-tool column layout, dd-mm-yyyy dates, UTF-8 BOM, formula-injection guard, atomic file writes.
- Portal JSON (GSTR-1 / GSTR-3B) for upload through the GST offline tool; check the first upload in the offline tool.
- Not supported yet: compensation cess, tax on advances received, amendments of earlier returns.

## 17. Backup and restore
- Backup = SQLite online backup API (never a raw file copy) + manifest (SHA-256, size, versions, companies).
- A backup is successful only after full verification: checksum, size, integrity_check, foreign_key_check,
  structure version known to this app. Failed backups leave no files.
- Restore: verify -> verified pre-restore safety backup -> atomic restore -> structure upgrade if older -> health
  check -> automatic undo from the safety backup on any failure. Admin only; audited.
- Opening an existing data file that needs a structure upgrade first takes a verified pre-migration backup.
- Retention deletes only the oldest AUTO backups; manual / pre-restore / pre-migration backups are never auto-deleted.
- Keep backups on another disk / pen drive / cloud folder too: a backup on the same disk does not survive a disk failure.

## 18. Done after V1 / still later
Done: bank reconciliation, TDS / TCS (incl. quarterly worksheet), e-invoice / e-way bill JSON with IRN / EWB number
recorded, foreign-currency notes with forex gain / loss on settlement, cost centres, budgets.

Still later: compensation cess, tax on advances, return amendments, direct IRN / e-way bill upload through a GSP
(needs a GSP account), TDS FVU file (made with the RPU), GSTR-2B reconciliation, payroll, multi-PC / network use.
