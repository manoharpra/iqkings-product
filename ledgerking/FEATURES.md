# LedgerKing features

Everything below is in **LedgerKing 1.1.0**; items marked 🆕 are new in 1.1.0. Every feature is in the trial and
in every plan (see [Plans](#plans)).

## Accounting

- Companies with their own financial years (any start month), books beginning date, GSTIN and state.
- 28 built-in groups (Tally style), your own groups and ledgers; the group is suggested from the ledger name.
- Vouchers: **Contra (F4), Payment (F5), Receipt (F6), Journal (F7), Sales (F8), Purchase (F9), Credit Note
  (Ctrl+F8), Debit Note (Ctrl+F9)**.
- Draft → post → cancel or reverse; gap-free numbers per type and year (e.g. `PY/0007`), cancelled numbers kept.
- **Recycle Bin**: bring back a voucher deleted in the last 30 days.
- Create a missing ledger or item from inside a voucher (**Alt+C**); edit the ledger or item you are looking at
  (**Alt+E**).
- Opening balances, period locks (e.g. lock April after filing), year close with verified carry-forward
  (**Alt+Y**).
- Party credit days, due dates and **credit limit** with a warning on the sales bill.
- **Recurring vouchers and bills** (rent, salary, EMI, AMC).
- Cost centres, budgets vs actual, foreign-currency bills with forex gain / loss on settlement.
- Interest on overdue bills.

## Bills, orders and stock

- GST sales and purchase bills with live tax, discount, round-off, services and goods.
- Bills remember the rate this party was last billed for each item; item selling price and MRP.
- Gross, discount and estimated profit shown on the bill.
- Stock items with units (Nos, Kg, Mtr …), HSN / SAC, GST rate history, opening stock and rate.
- **FIFO** stock value; negative stock blocked or warned (your choice); low-stock list.
- Stock Journal (manufacture, adjustment) (**Alt+J**), item register, stock summary (**Alt+V**).
- Sales returns and purchase returns against the original bill.
- Batches, multiple locations (godowns) with transfers, weighted average.
- Quotations (**Alt+Q**), sales orders (**Alt+O**) with part delivery, **purchase orders** that become purchase
  bills.
- Delivery challans that become invoices; proforma print.
- **POS counter billing** and Code 128 barcode labels.
- Sales / purchase list with tabs, delete or open any voucher from the list.
- **Price lists**: rate levels such as Wholesale, Retail or Dealer; give a party its list and its sales bills
  use those rates; fill a list from the selling price with a percentage.

## Bill scan 🆕

- **Scan supplier bill (PDF / QR)** on the Purchase bill (F9), or drop the file on the form. Read on your PC,
  offline; nothing is uploaded.
- From the **e-invoice QR** (page 1 or 2, or a picture of the QR) and the bill text: supplier GSTIN (check digit
  verified), bill no., date, taxable value, CGST / SGST / IGST, total; the supplier ledger is found by GSTIN.
- Warns when this supplier's bill no. is already entered; checks taxable + GST = total.
- **Item lines**: quantity, rate, amount, discount %, HSN, unit and GST % per row, matched with your stock items;
  the supplier's wording is remembered for next time. Missing items or suppliers can be created from the bill.
- Green = read, amber = check it, red = not on the bill. You check and save; nothing is posted by itself.
- Photo of a printed bill (text from the picture): planned, see [ROADMAP.md](ROADMAP.md).

## Fixed assets and manufacturing

- **Fixed asset register**: cost, date put to use, Income-tax WDV or straight-line rate, salvage value, half rate
  for assets used less than 180 days in the first year, earlier depreciation for old assets.
- Depreciation for the year posted as one journal; year-wise plan; sale or scrap posts the profit or loss.
- **Manufacturing**: bill of material for a batch, production with a stock check, actual quantity used
  (wastage), other costs (labour, power) added to the product's stock value; cancel from the voucher list.

## GST

Details in [GST.md](GST.md).

- Regular, Composition and Unregistered businesses; RCM; exempt / nil / non-GST items; dated GST rate slabs.
- **GSTR-1 and GSTR-3B**: on screen for every plan; **portal JSON** and CSV files.
- **GSTR-2B match**: load the portal JSON and match it with your purchase bills.
- **ITC register**.
- **E-invoice JSON** (NIC schema), IRN and signed QR recorded and printed.
- **E-way bill JSON**, e-way bill number recorded and printed; exports with port / ICD / airport.
- Exports (with / without IGST), SEZ, deemed exports, e-commerce operator sales.
- 🆕 **Debit note to a customer** (Debit Note (Sales)): in GSTR-1 and as an e-invoice debit note.
- 🆕 Purchase IGST or CGST + SGST from the supplier's state; tax rounded once per GST rate per bill; GSTR-1
  "Check with books".
- **TDS / TCS**: sections, PAN rates, challans, month-wise dues, quarterly 26Q / 27EQ worksheet, party
  statement.

## Reports

- Day Book (**Alt+D**), Ledger (**Alt+R**), Trial Balance (**Alt+T**), Profit & Loss (**Alt+P**), Balance Sheet
  (**Alt+B**).
- **Outstanding** receivable / payable with ageing 0–30 / 31–60 / 61–90 / 90+ (**Alt+W**).
- Cash Flow, Sales / Purchase register, item register.
- **Month-end party statement**: last month in one click, share on WhatsApp or e-mail.
- Excel export of every printable report.
- Home screen **"Today's work"**: money to collect and pay, low stock, coming GST dates.

## Money in and out

- **Payment reminders** on WhatsApp / e-mail, also on a schedule (the list is ready on the chosen day).
- **UPI "Scan to pay" QR** with the bill amount printed on sales bills (UPI ID set in invoice print settings).
- **Bank reconciliation**: bank dates, BRS, auto-match from the bank statement file (CSV / Excel).
- **Cheque printing** with a layout per bank.

## Printing

- Every voucher and report: **A4, 80 mm and 58 mm thermal** paper; PDF through "Microsoft Print to PDF".
- **Page setup**: orientation, paper size, margins, fit to width; saved per computer.
- Invoice print settings: your **logo**, Classic / Compact / Modern layouts, bank details, terms, signatory.
- Amount in words, signatures, CANCELLED stamp, supplier bill no. / buyer's order no. on the invoice.

## Moving in (imports)

- **Tally**: masters and Day Book XML (including item vouchers, stock journals, manufacturing journals, delivery
  notes). A summary per kind, a problem list (CSV) and "import the good entries".
- 🆕 Tally units and missing base units, invoices keep their Tally numbers, credit / debit notes linked to their
  invoice, cancelled numbers kept; company state / GSTIN / books-beginning checks; after the import the counts,
  values, closing balances and stock are compared with the Tally file; problems grouped by reason with a fix.
- **Excel / CSV**: ledgers with opening balances, stock items, old vouchers with column matching; up to
  **1,00,000 rows per file**.
- **Busy / Marg** export column names are recognised.
- Every import shows a preview first and is all-or-nothing.

## Safety and users

Details in [SECURITY.md](SECURITY.md).

- Verified backups, automatic backup when the app closes, a second copy on a pen drive / other drive, restore with
  an automatic safety copy.
- **Password-protected backup** files.
- **App lock with users and roles** (Admin / Accountant / Viewer), recovery key, auto-lock (**Alt+N**,
  Ctrl+Shift+L to lock now).
- Data file never kept in a live OneDrive / Dropbox folder; "Move to this PC" moves it safely.

## Ease of use

- **Keyboard first**: every screen has a shortcut, Enter goes to the next field, Esc goes back, **F1** shows help
  for the screen with search.
- **Ctrl+K**: search everything from one box (party, item, bill no., reference, amount or a screen).
- **English, हिंदी, मराठी, ગુજરાતી, বাংলা, தமிழ், తెలుగు**, with fonts built in for every script.
- **Simple Mode**: one switch in the header shows only the everyday screens.
- Themes **Neon**, Light, Dark, High-contrast; zoom 80–200 % (Ctrl + / Ctrl − / Ctrl 0).
- Getting-started card for a new company; focus mode; icon rail menu.
- Signed automatic updates; problem reports to IQKings support (automatic or "Send a problem report" with a
  screenshot).

## Keyboard shortcuts

| Key | Opens | Key | Opens |
|---|---|---|---|
| F4 | Contra | Alt+I | Stock items |
| F5 | Payment | Alt+J | Stock Journal |
| F6 | Receipt | Alt+Q | Quotations |
| F7 | Journal | Alt+O | Sales orders |
| F8 | Sales invoice | Alt+V | Stock summary |
| F9 | Purchase invoice | Alt+G | GST returns |
| Ctrl+F8 | Credit note | Alt+W | Outstanding |
| Ctrl+F9 | Debit note | Alt+X | Import (Excel / Tally) |
| Alt+L | Ledgers | Alt+Y | Years & locks |
| Alt+D | Day Book | Alt+N | App lock & users |
| Alt+R | Ledger report | Alt+U | License |
| Alt+T | Trial Balance | Alt+C | Create a missing ledger / item in a voucher |
| Alt+P | Profit & Loss | Alt+E | Edit the ledger / item on screen |
| Alt+B | Balance Sheet | Ctrl+K | Search everything |
| Alt+H | Home | Ctrl+P | Print |
| F1 | Help and all shortcuts | Esc | Back |

## Plans

**LedgerKing 1.1.0**: a **7-day trial** with every feature, then a paid plan (Monthly, Yearly or Lifetime)
with every feature. Without an active plan (trial over, Monthly / Yearly ended, or a version newer than a Lifetime
license's Update & Support) LedgerKing is **read-only**: everything you entered stays visible, printable,
exportable (Excel, GST CSV / JSON) and can be backed up and restored; new entries need a plan. A license may
limit the number of companies.

**Earlier, in LedgerKing 1.0.0**: a 30-day trial with every feature, then a Free plan for small shops
(1.1.0 replaced it with read-only):

- **Free (1.0.0 only)**: one company, up to **20 sales bills a month**, offline, all accounting vouchers, GST
  bills, Day Book, Ledger, Trial Balance, P&L, Balance Sheet, GSTR-1 and GSTR-3B on screen, printing on all paper
  sizes, backups and restore. It does not add: credit / debit notes, batches, more than one location, challans,
  export / SEZ / e-commerce invoices, password backups, GST portal JSON and CSV, ITC register, GSTR-2B match,
  payment reminders, recurring vouchers, Excel export, e-invoice, e-way bill, imports, TDS / TCS, bank
  reconciliation, cheque printing, cost centres, budgets, foreign currency, barcodes / POS, quotations and
  orders, logo and invoice print settings, users and roles, outstanding and ageing, interest, Cash Flow,
  registers, price lists, the fixed asset register and manufacturing.

**Nothing is ever locked away**, on any version: what you entered stays visible, printable and can be backed up.
Buying or renewing a plan opens new entries again at once.
