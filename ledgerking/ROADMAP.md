# LedgerKing roadmap

What is planned after 1.0.0. Dates are not promised: GST law changes and customer requests decide the order.
Tell us what you need: open a **Feature request** issue in this repository or write to support from your account
on [www.iqkings.com](https://www.iqkings.com).

## Next

| Item | Why |
|---|---|
| **Tax on advances received** | Advance receipts for goods / services before the invoice. |
| **Amendments of earlier returns** | GSTR-1 amendment tables for corrections of earlier months. |
| **Bill from a photo** | Scan a supplier's bill (photo or PDF): the e-invoice QR and the printed text fill the purchase bill for you to check. Works offline. |
| **Direct e-invoice / e-way bill upload through a GSP** | One click instead of JSON upload; needs a GSP account and API keys. |

## Later

| Item | Notes |
|---|---|
| Payroll | Salary structure, attendance, payslips. |
| Multi-PC / network use | Several computers on one set of books. Today the data file must be on a local disk. |
| TDS FVU file inside the app | Today the quarterly worksheet is ready and the FVU file is made with the government RPU. |

## Not needed

- **Compensation cess:** GST compensation cess ended on 01-02-2026: tobacco / pan masala now pay 40% GST, and the new excise duty and Health Security cess are paid by the manufacturer, not on GST bills. LedgerKing therefore needs no cess calculation; GST files send cess as 0.

## Done (moved from this list)

- GSTR-2B reconciliation with purchase bills — **1.0.0**
- Purchase orders, part delivery of sales orders — **1.0.0**
- Recurring vouchers, payment reminders on a schedule — **1.0.0**
- Bank reconciliation, TDS / TCS, e-invoice / e-way bill JSON, cost centres, budgets, foreign currency — test builds

See [CHANGELOG.md](CHANGELOG.md) for everything that is released.
