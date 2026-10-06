# Security and data safety

Your accounts are your business. LedgerKing is built so that your data stays on your computer, stays correct, and
can always be recovered.

## Your data stays with you

- LedgerKing works **offline**. Vouchers, bills, parties and reports are kept in one data file on your PC
  (`Documents\LedgerKing\Data`). They are **not** sent to IQKings or anyone else.
- Internet is used only to: activate or renew the license, check for updates, and send a problem report (only if
  you allow it: automatic reports can be switched off; a manual report is sent only when you press **Send report**).
- The data file must be on a **local disk**. LedgerKing warns if it is inside a live OneDrive / Dropbox folder and
  offers **Move to this PC**, which copies, verifies and only then switches (it never overwrites).

## Books that cannot be quietly changed

- A posted, cancelled or reversed voucher can never be edited or deleted, not even by changing the database file
  directly: database triggers refuse it.
- Corrections are visible: **Cancel** (keeps the number, removes the effect) or **Reverse** (a new journal with
  every side swapped), each with a reason.
- Every change to masters, vouchers, locks and years writes an **audit row** in the same step.
- Reports check their own totals (Dr = Cr, assets = liabilities). A mismatch shows an integrity error with a
  reference number instead of a wrong report.
- Period locks and closed years protect filed months.

## Backups

- **Verified backups**: a backup counts only after its checksum, size and database checks pass. A failed backup
  leaves no half file.
- **Automatic backup when the app closes**, plus a **second copy on a pen drive or another drive**. LedgerKing warns
  when the last backup is old.
- **Restore** first makes a verified safety copy of the current data, then restores; on any problem it goes back
  automatically.
- **Password-protected backups** (**Pro**): one encrypted file (Argon2id key + XChaCha20-Poly1305). A wrong password
  or any changed byte is refused; no unprotected copy is left behind. Keep the password safe: IQKings cannot
  recover it.
- Keep a copy away from the computer too: a backup on the same disk does not survive a disk failure.

## App lock, users and roles **Pro**

- Password to open the app; passwords are stored only as Argon2id hashes.
- Users with roles: **Admin** (everything), **Accountant** (masters, vouchers, cancel / reverse, locks),
  **Viewer** (reports only).
- **Recovery key** for a forgotten admin password; repeated wrong passwords slow down further tries.
- **Auto-lock** after idle time and **Ctrl+Shift+L** to lock at once. The app always starts locked.

## License and updates

- The license is a signed file (Ed25519) bound to your computer. It is checked offline; a changed license or a
  clock turned back is detected.
- **Data is never locked**: when a plan ends, the Free plan keeps everything visible, printable and exportable.
- Changing computers? **Move license to another computer** (License screen, Alt+U) frees it online so you can
  activate it on the new PC.
- **Updates are signed by IQKings.** The app installs an update only if the signature, the https address, the file
  name, the size and the SHA-256 of the installer all match. Any changed file is deleted, not run.
- Payments are made only on [www.iqkings.com](https://www.iqkings.com) (UPI or bank transfer), never inside the app.
  IQKings will never ask for your password, OTP or license file by phone or chat.

## Reporting a security problem

If you find a security problem in LedgerKing or iqkings.com, **do not open a public issue**. Write to IQKings
support from your account on [www.iqkings.com](https://www.iqkings.com) (Customer Support) with the subject
"Security" and include:

- what you found and where (app version, screen or web address),
- the steps to see it,
- what someone could do with it.

We reply as soon as we can, keep you informed, and fix confirmed problems in the next update. Please give us
reasonable time to fix a problem before telling others about it.

| Version | Security fixes |
|---|---|
| 1.0.x (latest) | ✓ |
| Earlier test builds | ✗ (please update) |
