# How LedgerKing is built

A short technical overview for accountants' IT people, CAs and curious users. The source code is private.

## Building blocks

| Part | Technology | Job |
|---|---|---|
| Accounting core | **Rust** | All rules: posting, numbering, stock, GST, returns, backups, license, plans. |
| Data | **SQLite** (one file) | All companies in one local file; rules enforced again by triggers and `CHECK`s. |
| Screens | **React + TypeScript** | Keyboard-first UI, themes, zoom, English / हिंदी, printing. |
| Desktop shell | **Tauri** (WebView2) | Windows window, installer (NSIS / MSI), signed updates. |

```
 Screens (React + TypeScript)
        │  one command channel ("api")
        ▼
 Command dispatcher (Rust) ── plan check (Free / Pro) ── license (Ed25519, offline)
        │
        ▼
 Engines: posting · vouchers · stock (FIFO) · GST · returns · e-invoice · TDS · bank · backup · import
        │  one transaction per change (all-or-nothing)
        ▼
 SQLite data file  (Documents\LedgerKing\Data\LedgerKing.sqlite)
```

The screens never write to the database themselves: every action is one command to the Rust core, the same code
the automated tests run. When the window closes, the core takes the automatic backup before the app exits.

## Key decisions

- **Money is whole paise** (integers), never floating point; every sum is overflow-checked.
- **Dates** are ISO text; one SQLite file holds all companies (`company_id` on every row).
- **Opening balances are derived** from entries (no carry-forward vouchers) and verified at year close.
- **Stock**: FIFO costing from stock movements; negative stock blocked or warned by setting.
- **GST**: exclusive and inclusive prices, discount before tax, 2-decimal half-up, CGST = SGST halves, IGST full.
- **Storage**: foreign keys on, WAL journal, full sync, one `BEGIN IMMEDIATE` transaction per change.
- **Migrations**: numbered, checksum-verified, applied in order; a data file from a newer version is refused; a
  failed upgrade rolls back fully, and a verified backup is taken before any upgrade.
- **Licensing**: Ed25519-signed offline license, device bound; Free plan after expiry, data never locked.
- **Payments**: no payment gateway in the app. Plans are bought on www.iqkings.com (UPI / bank transfer).

## Files on your computer

```
Documents\LedgerKing\
├── Data\LedgerKing.sqlite     your books (all companies)
├── Backups\                   automatic and manual backups
├── Exports\                   GST JSON / CSV, Excel exports
├── Logs\app.log               problems written here (no accounting data)
└── settings.json              screen and print settings
```

## Testing

Every release passes the full test suite:

- **Accounting proofs**: random vouchers, cancels and reversals always leave the books balanced and all reports
  tied out (property tests); a hand-calculated company matches TB, P&L, BS, Day Book and Ledger to the paisa.
- **GST**: hand-calculated tax cases, GSTR-1 / 3B tables cross-checked with the ledgers, exports / SEZ, e-invoice.
- **Stock**: FIFO, negative stock, stock journals, returns, batches, locations.
- **Safety**: backups refused when damaged or truncated, restore with undo, upgrades with data, **the app killed
  in the middle of a write three times** and the data still opens with balanced books and gap-free numbers.
- **Security**: raw database tampering with posted vouchers is blocked, password backups refuse any changed byte,
  only owner-signed updates are accepted, license tampering and clock rollback are detected.
- **Screens**: keyboard-only accounting flow in a real browser with the real Rust core; zoom 80–200 % × Windows
  scaling 100 / 125 / 150 % checked for overlap and clipping; printing checked with real PDF page sizes; every
  theme's text contrast and English / Hindi text parity tested.
