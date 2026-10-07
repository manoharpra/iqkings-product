# GST in LedgerKing

LedgerKing calculates GST on every bill, keeps the tax details of each line, and makes the files you upload on
the government portals. This page explains what it does and how. The complete rules are in
[ACCOUNTING.md](ACCOUNTING.md) (sections 13 and 16).

> Always check the first return of a new business with your CA. GST rules and portal formats change; LedgerKing
> updates (included in Update & Support) follow those changes.

## Types of business

| Registration | What LedgerKing does |
|---|---|
| **Regular** | Full GST on sales and purchases, CGST + SGST or IGST, ITC, GSTR-1 and GSTR-3B. |
| **Composition** | Bills without GST charged, composition rules applied. |
| **Unregistered** | No GST on bills; purchases kept at full cost. |

Change the registration in **Company Details**. The GSTIN and state of the company are required for taxed bills.

## How tax is calculated on a bill

- Prices can be **without tax (exclusive)** or **with tax (inclusive)**.
- **Discount first**: taxable value = price − discount.
- **Same state** (company state = place of supply): **CGST and SGST**, each at half the rate, always equal.
- **Other state**: **IGST** at the full rate.
- Rounding: 2 decimals, half-up, per line and per tax. Rounding the bill to the rupee is optional and posted to a
  separate Round Off ledger.
- Place of supply is the party's state unless you choose another on the bill. A taxed bill without company state or
  place of supply is refused, never guessed.
- **Exempt, Nil-rated and Non-GST** items are kept apart (GSTR-1 table 8).
- **RCM** (reverse charge) purchases: tax payable and ITC are recorded separately.
- **Dated rate slabs**: an item keeps its rate history, so a bill always uses the rate valid on its date. Official
  rate changes arrive as owner-signed rate updates.
- Each posted bill stores the HSN, unit and tax of each line as on the bill date, so later changes to the item never
  change a filed return.
- **Exports and SEZ**: with or without IGST (LUT / bond), port code and shipping bill; SEZ always IGST; deemed exports;
  e-commerce operator sales tagged with the operator GSTIN. **Pro**

## GST returns (Alt+G)

| Return | On screen | Files |
|---|---|---|
| **GSTR-1** | B2B, B2CL, B2CS, credit / debit notes (CDNR, CDNUR), exports, SEZ, e-commerce, nil / exempt / non-GST, HSN summary, documents issued (including cancelled numbers) | **Portal JSON** and CSV **Pro** |
| **GSTR-3B** | 3.1(a)–(e), 3.2, 4 (ITC, reverse-charge ITC separately), 5, tax payable per head before set-off | **Portal JSON** and CSV **Pro** |

- GSTR-1 and GSTR-3B are shown on screen on every plan; the files need Pro.
- Every GSTR-1 build checks that all tables and the HSN summary add up to the bill totals.
- GSTR-3B warns if GST ledgers have manual journal entries that no bill explains.
- B2C large limit: ₹1,00,000 by default (a setting; confirm with your CA).
- CSV files follow the offline-tool column layout (dd-mm-yyyy dates, UTF-8).
- How to upload the JSON: [iqkings.com/guides/how-to-file-gstr-1-json](https://www.iqkings.com/guides/how-to-file-gstr-1-json).

## GSTR-2B match **Pro**

Download GSTR-2B as JSON from the GST portal (Returns → GSTR-2B → Download) and pick the file in LedgerKing. It is
matched with your purchase bills by supplier GSTIN and supplier invoice number. You see the tax as per GSTR-2B, the
tax on your bills, the **credit at risk** (bills your suppliers have not reported) and a "Needs attention" list.
Nothing in your books is changed. ITC needs the supplier's GSTIN on the purchase bill.

## ITC register **Pro**

Input tax credit per bill and per month, with reverse-charge ITC kept separately.

## E-invoice **Pro**

1. Open the saved invoice (view / print screen) → **E-invoice / E-way bill** → **E-invoice JSON**.
2. Upload the JSON on the e-invoice portal.
3. Record the IRN (or paste the portal reply). The IRN and signed QR are printed on the invoice.

An invoice with an active IRN cannot be cancelled in LedgerKing (cancel the IRN first); the IRN never changes.
Export, SEZ and deemed-export invoice types are supported. Direct upload through a GSP is on the
[roadmap](ROADMAP.md).

## E-way bill **Pro**

1. Open the saved invoice → **E-way bill JSON**.
2. Upload it on the e-way bill portal (e-Waybill → Generate Bulk).
3. **Record e-way bill no.**: the number and date are printed on the invoice.

Exports: bill-to is the foreign buyer, ship-to is the port / ICD / airport in India. Guide:
[iqkings.com/guides/e-way-bill-json-guide](https://www.iqkings.com/guides/e-way-bill-json-guide).

## TDS / TCS **Pro**

- Sections with dated rates (kept up to date by IQKings), rate by PAN status, nearest-rupee rounding.
- The TDS / TCS journal is posted for you; challans and month-wise due amounts are tracked.
- Quarterly worksheet for **26Q / 27EQ** (deductor TAN, challan mapping, reason codes, late-payment interest) and a
  deduction statement for each party. The FVU file and Form 16A / 27D come from the government RPU / TRACES.

## Not supported yet

Tax on advances received, amendments of earlier returns, direct IRN / e-way bill upload through a GSP. See
[ROADMAP.md](ROADMAP.md).

**Compensation cess is not needed:** GST compensation cess ended on 01-02-2026: tobacco / pan masala now pay 40% GST, and the new excise duty and Health Security cess are paid by the manufacturer, not on GST bills. LedgerKing therefore needs no cess calculation; GST files send cess as 0.
