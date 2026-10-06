# LedgerKing (by IQKings) — Offline Accounting Software

Windows-first offline accounting app: **React + TypeScript + Tauri + Rust + SQLite**.
Plan: *prove the accounting core first, then build UI and advanced features around it.*

## Status

| Build step (from the file-structure plan) | Status |
|---|---|
| 1. Domain types + written accounting rules | ✅ done |
| 2. SQLite connection, migrations m001–m006 + m009, migration runner | ✅ done |
| 3. Posting, double-entry check, period lock, year close + tests | ✅ done |
| 4. Voucher lifecycle, numbering, cancel/reverse + tests | ✅ done |
| 5. Stock (m004, FIFO, negative-stock warnings) + GST (m005, invoices) | ✅ done |
| 6. Reports: Trial Balance, P&L, Balance Sheet, Day Book, Ledger, Stock Summary, Item Register | ✅ done |
| V1-C: Stock Journal, Quotation, Sales Order (m009), GSTR-1 / GSTR-3B data + CSV export | ✅ done |
| 7. Backup / verify / restore / retention / pre-migration backup | ✅ done |
| 7b. Excel/CSV import (Alt+X: ledgers + opening balances, stock items; preview, all-or-nothing) | ✅ done |
| 7c. Outstanding report (Alt+W): receivable / payable, FIFO ageing 0–30 / 31–60 / 61–90 / 90+ | ✅ done |
| 7d. App lock + users / roles (Alt+N): Argon2id passwords, recovery key, auto-lock, Admin / Accountant / Viewer (m010) | ✅ done |
| 7e. Password-protected backups (Argon2id + XChaCha20-Poly1305, one `.apenc` file, tamper-evident) | ✅ done |
| S1. Dynamic GST: dated rate slabs, item rate history, Exempt / Nil / Non-GST, owner-signed rate updates (Alt+Z) | ✅ done |
| S2. GST registration (Regular / Composition / Unregistered), RCM, ITC register, GSTR-1 table 8, company edit | ✅ done |
| S3. Stock: negative-stock block/warn, returns against invoice, low-stock, locations + transfer, weighted average, batches | ✅ done |
| S4. Invoicing: delivery challan → invoice, proforma print, credit days + due dates, services, WhatsApp / e-mail share | ✅ done |
| S5. Reports: Cash Flow, Sales / Purchase Register, Excel export on every printable report | ✅ done |
| S6. Quick create in vouchers (Alt+C), license move to another PC, signed auto-update (APU1), password backups | ✅ done |
| S7. Import old vouchers (CSV / Excel, column matching), Tally XML (masters + day book) | ✅ done |
| T1. GSTR-1 / GSTR-3B JSON for the GST portal (offline tool upload) | ✅ done |
| T2. E-invoice (NIC schema JSON, IRN + signed QR recorded and printed, IRN lock on cancel) and e-way bill JSON / number | ✅ done |
| T3. TDS / TCS: dated sections (admin), PAN rates, rupee rounding, journal posted, challans, month-wise due dates | ✅ done |
| T4. Bank reconciliation: bank dates, BRS, statement file auto-match | ✅ done |
| T5. Tally item vouchers imported as item invoices (only when the total matches Tally) | ✅ done |
| T6. Invoice print settings: logo, Classic / Compact / Modern layouts, bank details, terms, signatory | ✅ done |
| T7. Cheque printing (layout per bank) and interest on overdue bills | ✅ done |
| T8. Cost centres, foreign-currency notes, budgets vs actual | ✅ done |
| T9. Barcodes (Code 128 labels) and POS counter billing | ✅ done |
| T10. Busy / Marg export column names recognised in imports | ✅ done |
| U1. TDS quarterly worksheet (26Q / 27EQ: deductor TAN, challan mapping, reason codes, late-payment interest) + party TDS certificate print | ✅ done — the FVU file and Form 16A come from the RPU / TRACES |
| U2. GSTR-1 / 3B: exports (WPAY / WOPAY, port + shipping bill), SEZ, deemed exports, e-commerce operator sales; e-invoice EXPWP/EXPWOP/SEZ/DEXP | ✅ done (m017) |
| U3. Item selling price + MRP (fills invoice / POS rates) | ✅ done (m017) |
| U4. Forex gain / loss when a foreign-currency bill is settled | ✅ done (m017) |
| U5. Tally stock journal / manufacturing journal / delivery note import | ✅ done |
| U6. Compensation cess (now only tobacco / pan masala) | ⏳ not yet — item master warns for those HSN codes; needs a cess column in the posted-invoice tables |
| U9. Free / Pro plans: 30-day Pro trial, then Free forever (1 company, 30 sales a month, basic accounting); license = Pro; lifetime Update & Support (`updates_until`); buying only on www.iqkings.com | ✅ done (`eng_plan.rs`) |
| U8. E-way bill for exports: bill-to = foreign buyer (state 99), ship-to = port / ICD / airport (PIN + state), sub-type Export | ✅ done |
| U7. Direct e-invoice / e-way bill upload through a GSP | ⏳ needs a GSP account and API keys |
| 8. Command API (`dispatch`), Tauri desktop shell (`main.rs`, auto-backup on close) | ✅ done (Windows build type-checked; installer build to be run on Windows) |
| 9. React UI phase 1: company, ledgers, F4–F9 vouchers, Day Book, Ledger, TB, P&L, BS, backup, settings | ✅ done |
| 10. Printing: A4 + 80 mm thermal for vouchers and reports, PDF via the Windows print dialog | ✅ done |
| 9a. Voucher view: Cancel / Reverse; Years & Period Locks screen (Alt+Y) | ✅ done |
| 9c. Data safety: live data never kept in OneDrive/Dropbox; verified "Move to this PC" | ✅ done |
| 9b. Item invoices with live GST (F8/F9, Ctrl+F8/F9), stock items, Stock Journal, quotations / sales orders, stock summary, GST returns + CSV | ✅ done |
| 11. License client: Ed25519 offline license, device binding, 30-day Pro trial then Free plan, buy on www.iqkings.com, update check (owner panel = iqkings-admin) | ✅ app side done — backend per `docs/doc_license_api_contract.md` |
| 12. Crash-recovery test (process killed mid-write), release script (`scripts/release_windows.ps1`), version script | ✅ done |
| 12b. Build + sign the installer on Windows (code-signing certificate), publish on api.iqkings.com | ⏳ owner, on Windows |

Accounting rules: [`docs/doc_accounting_rules.md`](docs/doc_accounting_rules.md). License server contract:
[`docs/doc_license_api_contract.md`](docs/doc_license_api_contract.md). Windows setup and updates:
[`docs/doc_local_update.md`](docs/doc_local_update.md).

## Build and test (Rust core)

```bash
cd src-tauri
cargo test                      # all unit + integration + property tests
cargo clippy --all-targets      # must be warning-free
cargo fmt --check
```

The core crate builds without Tauri/WebView, so all accounting tests run on any OS.
The desktop window is behind the `desktop` feature (`src-tauri/src/main.rs`, `tauri.conf.json`).

## UI (React + TypeScript) and desktop app

```bash
npm install
npm run typecheck && npm test          # TypeScript + unit tests (vitest)
npm run e2e                            # real browser + real Rust core (Playwright)
npm run dev                            # browser development: starts Vite AND the Rust engine (data in .dev-data/)
npm run desktop:dev                    # Windows: desktop window (Tauri)
npm run desktop:build                  # Windows: NSIS + MSI installers (needs Visual Studio Build Tools)
```

- **Windows setup and updating from a zip**: [`docs/doc_local_update.md`](docs/doc_local_update.md)
  (`.\scripts\update_from_zip.ps1`). E2E on Windows: run `npx playwright install chromium` once.
- **Data folder**: `Documents\LedgerKing\` (`Data\LedgerKing.sqlite`, `Backups\`, `Exports\`, `Logs\`,
  `settings.json`). `ACCOUNTING_PRO_HOME` overrides it (tests use a temp folder).
- **Desktop shell**: one IPC command `api` → `commands::dispatch` (the same code the tests run). Closing the window
  takes the automatic backup in Rust before it closes; a failure is written to `Logs\app.log`. If the data file
  cannot be opened, the window still opens and every command returns that error.
- **Keyboard first**: F4 Contra, F5 Payment, F6 Receipt, F7 Journal, F8 Sales invoice, F9 Purchase invoice, Ctrl+F8 Credit
  Note, Ctrl+F9 Debit Note, Alt+I Stock Items, Alt+J Stock Journal, Alt+Q Quotations, Alt+O Sales Orders, Alt+V Stock
  Summary, Alt+G GST Returns, Alt+W Outstanding, Alt+X Import (Excel / Tally), Alt+C create a missing ledger / item inside a voucher, Alt+N App Lock & Users, Ctrl+Shift+L lock now,
  Alt+Y Years & Locks, Alt+U License, Enter = next field,
  Esc = back, Ctrl+P print, F1 lists every shortcut. Browser keys (F5 reload, F7 caret browsing, Ctrl+R) are blocked.
- **Zoom** 80–200 % (Ctrl + / Ctrl − / Ctrl 0) re-lays out the screen (rem-based, wide / medium / narrow layouts);
  text size is separate. Themes: **Neon** (default: deep navy; headings cyan, labels violet, amounts gold, Dr/Cr
  sky blue, pink focus ring), Light (soft teal), Dark, High-contrast — every text role contrast unit-tested.
  Printing is always dark ink on white, whatever the theme. English + हिंदी.
- **Printing**: every report and voucher has *Print (Ctrl+P)* with A4 or 80 mm thermal paper (default saved in
  Settings). Prints use physical font sizes (screen zoom never changes paper output), a company letterhead, period,
  printed-on time; vouchers add amount in words, signatures and a CANCELLED stamp for cancelled vouchers.
  PDF = "Microsoft Print to PDF" in the print window. Day Book: Enter on a voucher opens it for printing.
- Screenshots: [`docs/screenshots/`](docs/screenshots/).

## Test suite

| File | What it proves |
|---|---|
| `test_money_rounding.rs` | paise parsing/formatting, rounding modes, overflow never wraps (property tests) |
| `test_double_entry_rules.rs` | random vouchers/cancels/reversals/openings → books always balance, all reports tie out, year close carries forward exactly (property tests) |
| `test_voucher_state_flow.rs` | draft→post→cancel/reverse, gap-free numbering, permissions, **raw-SQL tampering blocked by DB triggers** |
| `test_period_and_year_lock.rs` | period lock, reversal into open period, year close/reopen, closed-year protection |
| `test_golden_company.rs` | hand-calculated company: TB, P&L, BS, Day Book, Ledger match to the paisa |
| `test_gst_tax_cases.rs` | hand-calculated GST cases (exclusive/inclusive, intra/inter, discount, half-up, round-off) + property test (CGST = SGST, totals exact) |
| `test_stock_valuation_cases.rs` | FIFO, negative stock warnings/flags, GST invoices end-to-end, credit note, cancel, stock in P&L/BS/TB, year close, Stock Journal (manufacture/adjustment); random stock property test |
| `test_orders_quotations.rs` | quotation → order → invoice, expiry, cancel, gap-free numbers, immutability |
| `test_gst_returns_export.rs` | hand-calculated GSTR-1 (B2B/B2CL/B2CS/CDNR/HSN/docs) and GSTR-3B, ledger cross-check, CSV files |
| `test_backup_restore_verify.rs` | verified backups, damaged/truncated/foreign files refused, real restore + safety copy, older-backup upgrade, pre-migration backup, retention |
| `test_migration_upgrade.rs` | fresh install, upgrade with data, newer-DB refusal, checksum tamper, rollback, WAL file |
| `test_inventory_commands.rs` | item master, invoice preview = posted invoice, purchase → sales → credit note, Stock Journal, item register, quotation → order → invoice, GSTR-1/3B + CSV through the command API |
| `test_license.rs` | license signature / tampering / product / device / expiry, 30-day Pro trial, clock rollback, Free plan after the trial (data never locked), lifetime Update & Support (versions released until `updates_until`) |
| `test_data_location.rs` | OneDrive/Dropbox detection, safe data folder choice, verified move that never overwrites |
| `test_crash_recovery.rs` | app killed mid-write 3× → data opens, integrity OK, gap-free numbers, books balance |
| `test_import.rs` | strict amounts (commas, decimals, Dr/Cr), CSV + real .xlsx, every row problem listed, nothing imported unless all rows are correct |
| `test_outstanding.rs` | receivable / payable with FIFO ageing, advances, cancelled vouchers ignored, agrees with the ledger |
| `test_app_lock.rs` | lock gate, Argon2id hashes only, wrong-password throttle, restart starts locked, idle auto-lock, recovery key, Accountant / Viewer limits, deactivated user logged out |
| `test_tax_rules.rs`, `test_gst_registration.rs` | dated GST slabs and item rates, signed rate updates (owner only), Exempt / Nil / Non-GST, composition / unregistered, RCM, ITC |
| `test_stock_extras.rs`, `test_invoicing_extras.rs` | negative-stock policy, returns, locations / transfers, weighted average, batches; challans, due dates, services |
| `test_reports_extra.rs` | Cash Flow ties to cash/bank movement, registers, Excel export |
| `test_backup_encrypt.rs` | password backup round trip, wrong password, no plain copy left, every flipped byte refused |
| `test_app_update.rs` | only owner-signed update manifests, https + safe file name, size + SHA-256 of the installer, changed file deleted |
| `test_import_vouchers_tally.rs` | voucher import with column mapping, preview changes nothing, all-or-nothing, no double import; Tally XML (UTF-16, signs, groups, items, cancelled / stock vouchers) |
| `test_einvoice.rs` | e-invoice / e-way bill JSON only with complete details, IRN only for the matching invoice, active IRN blocks cancel, IRN never changed |
| `test_tds.rs` | TDS / TCS rates by PAN, nearest-rupee rounding, exact journal, challans and month-wise dues, dated admin sections, quarterly statement (challan mapping, reason codes, interest) |
| `test_bank_reco.rs` | BRS arithmetic, bank date rules, statement matching (amount, side, date window, used once) |
| `test_office_tools.rs` | logo checked as a real image, cheque data / layout, interest, cost centres, FC note = voucher total, budget vs actual, barcodes, cash counter sale, selling price / MRP, forex gain and loss on settlement |
| `test_plan.rs` | Free plan after the trial: one company (others view-only), 30 sales a month (cancelled ones free a place), no returns / batches / other locations / export invoices / password backup / Pro tools, data made with Pro stays visible; a license lifts every limit; license company limit |
| `test_gst_export_sez.rs` | exports with / without IGST, SEZ (always IGST), deemed export, e-commerce sales: posting rules, GSTR-1 tables + cross-check, 3.1(b), portal JSON, e-invoice export buyer |
| `test_review_fixes.rs` | stock journal cost conserved (yield / loss), mixed-unit cost sharing refused, sales/purchase only to trading ledgers, party never a tax / built-in ledger, openings locked after entries (admin restatement with reason, audited), no report ending before the books begin, DB triggers (SGST, cross-company updates), Day Book stock-only vouchers |

UI tests: `src/tests/*.test.ts` (money, dates, zoom steps, shortcuts, i18n parity, ledger search ranking, palette
contrast, amount in words, paper sizes) and `e2e/*.spec.ts` (keyboard-only accounting flow; zoom 80–200 % × Windows
scaling 100/125/150 % with overlap/clipping checks; printing incl. real PDF page sizes).

## Decisions

- **Money** = integer paise; **dates** = ISO text; **one SQLite file** holds all companies (`company_id` everywhere).
- **Openings are derived** from entries (no carry-forward vouchers), verified at year close by a snapshot.
- **Inventory**: FIFO costing; negative stock allowed with red warnings and report flags.
- **GST**: exclusive + inclusive, discount before tax, 2-decimal half-up, optional separate round-off,
  CGST/SGST equal halves intra-state, IGST full rate inter-state.
- **Migrations**: the runner applies any known, not-yet-applied migration in ascending order. After the first public
  release, new migrations must use a number above every released one, and released migration files are frozen
  (checksum-verified). Until then migration files may still change (no customer databases exist yet).
- **Payments — no payment gateway.** Customers pay by **UPI QR code or bank transfer** to the owner. The in-app
  license screen and the owner panel show the owner's QR code image and bank details (account name, number, IFSC,
  bank, UPI ID) from configuration — never hard-coded. After receiving payment the owner verifies it manually and
  issues the signed offline license (owner-panel Phase 1: `tool_license_keygen.ts`). The Razorpay webhook in the
  original file plan (`owner_api_payments.ts`) is replaced by a manual "record payment → issue license" flow.
- **Licensing**: Ed25519-signed offline license; data view/export always allowed even after expiry.

## Repository note
This project is independent and is being moved to its own repository by the owner (git is managed by the owner).
`.github/workflows` CI will be added once it lives in its own repository.
