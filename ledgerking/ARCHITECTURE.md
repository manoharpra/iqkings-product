# How LedgerKing is built and tested

A short overview for business owners, CAs and their IT people: what LedgerKing is made of, where your data lives
and how every release is checked. The source code is private.

## Built on proven technology

| Part | Technology | Why it matters to you |
|---|---|---|
| Accounting core | **Rust** | Fast and memory-safe. Every rule (posting, stock, GST, returns, backups, license) lives here, in one place. |
| Your data | **SQLite**, one file on your PC | Nothing to install or manage, works offline, easy to back up. |
| Screens | **React + TypeScript** | Keyboard-first, themes, zoom, 7 Indian languages with fonts built in, printing. |
| Desktop app | **Tauri** (Windows WebView2) | Small installer, starts fast, updates signed by IQKings. |

```
 Screens  ──►  Accounting core (Rust)  ──►  Your data file (SQLite, on your PC)
              plan and license checks       every change all-or-nothing
```

The screens never write to your data themselves: every action goes through the accounting core, the same code the
automated tests check. When you close the app, it takes an automatic backup first.

## Design choices that keep your books right

- **Exact money**: amounts are kept in whole paise, so totals never drift by a paisa.
- **Openings are calculated, not copied**: last year's closing becomes this year's opening by calculation, and the
  year close checks it before it finishes.
- **Stock value by FIFO**; negative stock blocked or warned, as you choose.
- **GST** with inclusive or exclusive prices, discount before tax, CGST = SGST halves, IGST in full.
- **All-or-nothing saving**: a power cut never leaves half an entry. Upgrades take a verified backup first and roll
  back fully on any problem.
- **Offline license**, signed by IQKings and tied to your computer. When a plan ends, your data stays visible.
- **No payment inside the app**: plans are bought only on [www.iqkings.com](https://www.iqkings.com).

More: [ACCOUNTING.md](ACCOUNTING.md) (all rules) · [SECURITY.md](SECURITY.md) (data safety).

## Files on your computer

```
Documents\LedgerKing\
├── Data\LedgerKing.sqlite     your books (all companies)
├── Backups\                   automatic and manual backups
├── Exports\                   GST JSON / CSV, Excel exports
├── Logs\app.log               problems written here (no accounting data)
└── settings.json              screen and print settings
```

## How every release is tested

Each version must pass the full automated test suite (hundreds of checks) before it is published:

- **Accounting proofs**: random vouchers, cancels and reversals always leave the books balanced and every
  report tied out; a hand-calculated company matches Trial Balance, P&L, Balance Sheet, Day Book and Ledger to the paisa.
- **GST**: hand-calculated tax cases, GSTR-1 / 3B tables cross-checked with the ledgers, exports / SEZ, e-invoice.
- **Stock**: FIFO, negative stock, stock journals, returns, batches, locations, manufacturing.
- **Safety**: damaged backups refused, restore with undo, upgrades with real data, and **the app killed in the middle
  of saving** — the data still opens with balanced books and no missing numbers.
- **Security**: direct tampering with saved vouchers is blocked, password backups refuse any changed byte, only
  IQKings-signed updates are installed, license tampering and a clock turned back are detected.
- **Screens**: the whole keyboard flow in a real browser with the real accounting core; zoom 80–200 % at Windows
  scaling 100 / 125 / 150 % checked for cut-off text; printing checked with real page sizes; every language and
  theme checked for missing text and readable contrast.
