# LedgerKing changelog

Newest first. Download the latest version from [iqkings.com/ledgerking](https://www.iqkings.com/ledgerking);
installed copies update themselves (signed updates).

## Next version — in testing

**Bill scan: a purchase bill from the supplier's PDF** (Purchase, F9)
- **Scan supplier bill (PDF / QR)**, or drop the file on the Purchase form. The file is read on your PC, offline;
  nothing is uploaded.
- Reads the **e-invoice QR** (page 1 or 2, or a picture of it) and the bill text: supplier GSTIN (check digit
  verified), bill no., date, taxable value, CGST / SGST / IGST and total. Finds the supplier ledger by GSTIN and
  warns if this supplier's bill no. is already entered.
- **Item lines**: quantity, rate, amount, HSN, unit and GST % from each row; each row is matched with your stock
  item, and LedgerKing **remembers the supplier's wording** for next time. "Fill items into the bill" puts them on
  the form; a missing item or supplier can be created from the bill.
- Colours show what to check: green = read, amber = check it, red = not on the bill. Nothing is ever posted
  without you.

**Moving from Tally, more complete**
- Units from Tally (and missing base units), invoices keep their Tally numbers, credit / debit notes linked to
  their invoice (Agst Ref), cancelled bill numbers kept.
- Company state, GSTIN and books-beginning date checked; opening balances, closing balances and stock compared
  with the Tally file after the import.
- Problems grouped by reason with a suggested fix (also in the CSV), and an import log.

**GST and reports**
- Purchase bills: IGST or CGST + SGST decided by the supplier's state.
- **Debit note to a customer** (Debit Note (Sales)), also in GSTR-1 and as an e-invoice debit note.
- Tax rounded once per GST rate per bill, as the portal does; GSTR-1 **"Check with books"**; document series by
  number prefix; credit notes on B2C large bills reported correctly.
- Balance Sheet: a party with a balance on the "wrong" side is shown as an advance.

**Plans (new rules)**
- **7-day trial** with every feature (existing users count from their first start).
- After the trial, or when a Monthly / Yearly plan ends, LedgerKing becomes **read-only**: everything you entered
  stays visible, printable, exportable and can be backed up; new entries need a plan. The Free plan is replaced
  by the trial.
- Lifetime: a newer version than your Update & Support covers also opens read-only; renew updates to use it.
- Name on the window, start screen and invoices: **LedgerKing by IQKings**. Header shows **👑 Pro**.
- A short review card after 10 days of use and a monthly feedback card (you can skip both).

## 1.0.0 — 7 October 2026

First public release under the name **LedgerKing** (earlier test builds were called "Accounting Pro").
Download, pay by UPI and get the product key by e-mail at [iqkings.com/ledgerking](https://www.iqkings.com/ledgerking).

### Added just before release

**Speak your customer's language**
- Five more languages: **मराठी, ગુજરાતી, বাংলা, தமிழ், తెలుగు** (with English and हिंदी, that makes seven).
  Fonts for every script are built in, so matras and conjuncts show correctly, even without internet.
- **Simple Mode**: one switch shows only the everyday screens for first-time users.

**Grow your business** (Pro)
- **Price lists**: Wholesale / Retail / Dealer rates; a party's sales bills use its list automatically.
- **Fixed asset register** with depreciation (Income-tax WDV or straight line), yearly posting, sale or scrap.
- **Manufacturing**: bill of material, production with shortage check, wastage and true production cost.

**Big books, still fast**
- **Excel / CSV import up to 1,00,000 rows per file** (was 5,000): a whole year of vouchers in one file. The
  preview shows the first 1,000 rows and every row with a problem; counts always cover the whole file.
- **Bills stay fast as the books grow**: the stock check on a bill now reads only the items on that bill. Tested
  with a big company (big imports, thousands of GST bills, backup and restore) before release.
- Long jobs (a big import or backup) run in the background, so Windows never shows "Not responding".

**Plans**
- 30-day trial with every feature, then the Free plan for small shops: 1 company and **20 sales bills a month**
  (changes with the next version, see above).
- A paid license shows **👑 LedgerKing Pro** in the header; "Upgrade" only during the trial and on the Free plan.
- Lifetime licenses: official GST rate updates come with an active Update & Support; a small note on GST pages
  says when it has ended.

### New since the test builds

**Work faster**
- **Ctrl+K**: search everything (party, item, bill no., reference, amount, screen) from one box.
- Home **"Today's work"** card: money to collect and pay, low stock, coming GST dates.
- Getting-started card for a new company; **F1** help for each screen, with search.
- Bills remember the rate this party was last billed for each item.
- Ledger form suggests the group from the name.
- Edit the ledger or item you are looking at (✎ / **Alt+E**).
- New app layout: title-bar header, icon rail menu, focus mode, section colours.
- One "Sales / Purchase List" with tabs; delete or open any voucher from the list.

**Money in and out**
- **UPI "Scan to pay" QR** with the bill amount on sales bills.
- **Month-end party statement**: last month in one click, share on WhatsApp / e-mail.
- **Payment reminders** by WhatsApp / e-mail, also on a schedule.
- Party **credit limit** with a warning on sales bills.
- **Recurring vouchers and bills** (rent, salary, EMI, AMC).

**Orders and stock**
- **Purchase orders** that become purchase bills.
- Sales orders with **part delivery** (bill only some of an order).
- Clear units, item opening rate; gross, discount and estimated profit on bills.

**GST**
- **GSTR-2B match**: the portal JSON reconciled with your purchase bills.
- ITC needs the supplier's GSTIN (as the law requires).

**Printing**
- **Page setup**: orientation, paper size, margins, fit to width, and a new Print Settings page.
- **58 mm** thermal paper (with 80 mm and A4); thermal invoice layout; every print uses the chosen paper.
- Invoice shows the supplier bill no. / buyer's order no.

**Safety**
- **Recycle Bin**: bring back a voucher deleted in the last 30 days.
- Backup: a second copy on a pen drive / other drive; stronger warning when the last backup is old.
- Tally import: summary per kind, a problem list (CSV) and "import the good entries".

**License and plans**
- "Upgrade" button in the header and a Plans screen; the License screen shows the plan clearly.
- License renewal after expiry, reminders for Monthly plans, banner when a plan or Update & Support ends soon.
- Problem reports to IQKings support (automatic, or "Send a problem report" with a screenshot).
- Signed installer named `LedgerKing-Setup-<version>`; data folder `Documents\LedgerKing` (an older
  `AccountingPro` folder is taken over automatically).

### Included from the test builds

- Complete double-entry accounting: F4–F9 vouchers, credit / debit notes, cancel / reverse, gap-free numbering,
  period locks, year close.
- Reports: Day Book, Ledger, Trial Balance, P&L, Balance Sheet, Cash Flow, Sales / Purchase registers, Outstanding
  with ageing, stock summary, item register; Excel export.
- Inventory: FIFO, negative-stock policy, returns, batches, locations and transfers, weighted average, stock journal,
  quotations, sales orders, delivery challans, POS and barcodes.
- GST: dated rate slabs, Regular / Composition / Unregistered, RCM, ITC register, GSTR-1 / GSTR-3B JSON and CSV,
  exports / SEZ / deemed exports / e-commerce, e-invoice and e-way bill JSON with IRN / EWB number.
- TDS / TCS with quarterly 26Q / 27EQ worksheet; bank reconciliation with statement matching; cheque printing;
  interest; cost centres; budgets; foreign currency with forex gain / loss.
- Imports: Excel / CSV, old vouchers with column matching, Tally XML (masters, Day Book, item vouchers, stock
  journals, delivery notes), Busy / Marg column names.
- Safety: verified backups and restore, password-protected backups, app lock with users and roles, crash-safe
  writes, data kept off live cloud folders.
- Printing on A4 and 80 mm thermal; logo, Classic / Compact / Modern invoice layouts.
- Free / Pro plans with a 30-day Pro trial; offline license; signed automatic updates.
- English and हिंदी; Neon, Light, Dark, High-contrast themes; zoom 80–200 %.
