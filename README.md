# Qashio ⇄ QuickBooks Reconciliation Tool

A single web page that reconciles the monthly Qashio corporate card statement
against the QuickBooks ledger — the same checks we used to do by hand,
automated. Nothing leaves your browser: there's no server, no login, and no
data is uploaded anywhere. You can open the file directly on your computer,
or use the hosted link below.

## What it does

You upload two files and it tells you exactly what to do with each
transaction:

1. **Qashio statement export** (`.xlsx`) — the card statement, with columns
   for Date, Voucher, Transaction ID, Type, Merchant, User, Paid, Received,
   Balance.
2. **QuickBooks "Transaction Detail by Account" export** (`.xlsx`) — the
   ledger. The tool automatically finds the Qashio account for you (you can
   change it from a dropdown if it guesses wrong).

Click **Run reconciliation** and it works through the same checks we used to
do manually:

| Step | What it's checking |
|---|---|
| 1. Exact match | Same date, same amount |
| 2. Date-shifted match | Same amount, posted a few days early/late (up to 60 days) |
| 3. Consolidation | Several small card purchases that were posted as one bill in QuickBooks, or vice versa |
| 3b. Zero-net | Bought something, then fully refunded a few days later — nothing needed in QuickBooks at all |
| 4. Net-of-refund | A purchase posted "short" in QuickBooks because a refund was netted against it — flags it if the refund also looks like it was posted separately (a double-count) |

Anything left over after all five checks is sorted into tabs:

- **Post as Expense** — genuinely missing from QuickBooks
- **Post as Refund** — a refund that came back onto the card but was never recorded
- **Correct Existing Entries** — already in QuickBooks but at the wrong amount
- **Remove / Investigate** — in QuickBooks with nothing matching it on the statement at all
- **Already Matched** — for your records, so these don't get re-flagged next month

The balance strip at the top always shows the **Qashio total vs QuickBooks
total and the gap between them** — that's the number that has to tie out to
zero once everything below it is resolved.

### Matching tolerance

Small real-world differences (a rounding fee, a fils of discount) shouldn't
show up as two unexplained transactions. Click **"Matching tolerance
(advanced)"** above the Run button to see and adjust how many AED apart two
amounts can be and still count as the same transaction. Every tolerance
match is still shown to you with the exact difference (e.g. `Δ 0.05 AED`) —
nothing is ever matched silently. The default is deliberately small (0.03–
0.05 AED) so it only catches genuine rounding, not real discrepancies.

### Download report

Once it's run, **Download full report (.xlsx)** gives you the same workbook
structure we've always used — Summary, then a tab for each category above,
plus the raw data from both files — ready to hand to a manager as evidence.

## Using it

**Option A — open the file directly (simplest).** Download
`qashio_reconciler.html` and double-click it. It opens in your browser and
works completely offline once loaded (it only needs internet the first time,
to load two small libraries for fonts and Excel files).

**Option B — use the hosted link.** Once this is on GitHub Pages (see
below), just bookmark the page and use it from there — nothing to download,
and it always has the latest version.

Nothing is posted to QuickBooks automatically. This tool only tells you
*what* to do — you still enter it yourself.

## Deploying to GitHub Pages

You only need to do this once.

1. Go to [github.com](https://github.com) and sign in (or create a free
   account).
2. Click the **+** in the top-right corner → **New repository**.
   - Name it something like `qashio-reconciler`.
   - Set it to **Public** (GitHub Pages needs this on a free account).
   - Click **Create repository**.
3. On the new repository's page, click **Add file → Upload files**.
4. Drag in `qashio_reconciler.html` and `README.md` from this folder, then
   click **Commit changes**.
5. Go to the repository's **Settings** tab → **Pages** (left sidebar).
6. Under **Build and deployment → Source**, choose **Deploy from a branch**.
   Under **Branch**, choose `main` and folder `/ (root)`, then **Save**.
7. Wait about a minute, then refresh the Pages settings page — it will show
   a link like:

   ```
   https://<your-username>.github.io/qashio-reconciler/qashio_reconciler.html
   ```

   That's your permanent link. Bookmark it.

*(Optional: if you'd rather the link not have `/qashio_reconciler.html` on
the end, rename the file to `index.html` before uploading — then the link is
just `https://<your-username>.github.io/qashio-reconciler/`.)*

### Updating it later

Whenever we make changes to the tool, go to the repository, click on
`qashio_reconciler.html`, click the pencil (✎) icon to edit, and either
paste the new version in or use **Upload files** again with the same file
name — GitHub Pages picks up the change automatically within a minute or
two.

## A note on privacy

This tool runs entirely in your browser — your statement and ledger files
are never uploaded to GitHub, Anthropic, or anywhere else. Hosting it on
GitHub Pages only makes the *empty tool* (the web page itself) publicly
reachable by its link; your company's actual financial data only exists on
your screen for as long as the tab is open, and disappears when you close it
or refresh.

## Fuel monthly allocation (ENOC · ADNOC · Emarat)

Fuel is paid on the card all month but booked in QuickBooks as **one allocation per supplier for the whole month** (3 per month: ENOC, ADNOC, Emarat — EPPCO is treated as part of ENOC).
The tool detects fuel merchants, totals each month's fuel (purchases minus refunds) and matches it
against the QuickBooks allocation — per station first, then all stations combined — within the
"Monthly fuel allocation" tolerance (default 1.00 AED). QuickBooks entries dated up to 10 days after
month-end count toward that month. The **Fuel Allocation** tab shows each month as Allocated /
Mismatch / Not allocated, and the **Analytics** tab shows spend by month, merchant, cardholder and
fuel station. Both are included in the downloaded report.

## Favicon

`favicons/` holds the single combined QuickBooks + Qashio icon (SVG, PNG 16/32/48/180/512, ICO).
