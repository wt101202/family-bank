# Family Bank

**Live:** <https://wt101202.github.io/family-bank/> · sample data: <https://wt101202.github.io/family-bank/#demo>

A bank for your kids that pays interest, shows them the snowball, and tells them what every purchase really costs.

One file. No server, no accounts, no dependencies. Each family's data lives in their own browser.

---

## How it was put on GitHub Pages (for reference, or to redo it)

1. Go to <https://github.com/new>. Repository name: `family-bank`. Public. Click **Create repository**.
2. On the empty-repo page click **uploading an existing file**, drag `index.html` from this folder onto the page, click **Commit changes**.
3. In the repo: **Settings → Pages** (left sidebar).
4. Under *Build and deployment*: Source = **Deploy from a branch**; Branch = **main**, folder **/ (root)**. Click **Save**.
5. Wait about a minute, refresh the Pages settings page. Your link is shown at the top:
   `https://wt101202.github.io/family-bank/`

That link is the whole product. Bookmark it on your phone.

**To update the site later:** open `index.html` in the repo on GitHub, click the pencil, paste the new file contents, commit. Or drag-and-drop the new file the same way as step 2. Pages redeploys on its own.

Alternative (if you'd rather use a terminal):

```
cd "<this folder>"
git init && git add index.html README.md BRIEF.md && git commit -m "Family Bank v3"
gh repo create family-bank --public --source=. --push
gh api -X POST repos/wt101202/family-bank/pages -f build_type=legacy -f source[branch]=main -f source[path]=/
```

---

## Sharing it with another family

Send them the link. They get the setup screen, enter their family name and kids, and they're done. Nothing of yours is visible to them — localStorage is per-browser. The same link works for everyone.

Inside the app, **⚙ Settings → Share** has a copy button for the link.

`https://wt101202.github.io/family-bank/#demo` opens it pre-loaded with sample data (two kids, a year of history) so someone can see it working before setting up their own. The setup screen has the same thing as a button.

---

## Backups

Data is only in the browser that created it. Clearing site data, or a kid using a different browser, means a different (empty) bank.

**⚙ Settings → Your data → Export backup** downloads a small `.json` file. Keep it somewhere safe (OneDrive, Drive). **Import** restores it on any device. Export after each report card and you'll never lose more than a few weeks.

The export from the earlier Netlify version (`family_bank_data.json`) imports fine — it's migrated automatically. Manually-added "Monthly Interest" rows are dropped because growth is now computed continuously.

---

## How the numbers work

- **Growth** accrues automatically. Every deposit compounds from the day it was made at the family rate (default 10%/yr, annual effective, fractional years). Withdrawals come off the balance on their date. Nothing to click monthly.
- **Erased @ age** on a purchase = the amount, compounded from the kid's age on that date to the milestone age. Fixed at the moment of the purchase — it doesn't drift as the rate changes later.
- **Ghost line** on History = the same ledger with every withdrawal removed. The gap is what spending has cost so far (principal plus foregone growth).
- **Real market** mode applies actual S&P 500 total returns (price + dividends, 1926–2025, source: S&P Dow Jones Indices via slickcharts.com) year by year from a random start year, over the same horizon as the smooth chart. 100 years: 10.5% CAGR, 26 down years, worst −43% (1931), best +54% (1933). Every 20-year window in the series ended higher than it started.
- Kids are stored by birth month, so ages advance on their own.

---

## Files

| File | What |
|---|---|
| `index.html` | The whole app. This is the only file GitHub Pages needs. |
| `README.md` | This. |
| `BRIEF.md` | The agreed spec for v3 and the decisions behind it. |

No build step, no `node_modules`, nothing generated. Safe for OneDrive.

**Dev notes.** Pure math lives between `/*ENGINE-START*/` and `/*ENGINE-END*/` in the script and can be unit-tested in Node by extracting that block. URL hash flags: `#demo` seeds sample data on first load; `#history`, `#learn`, `#spend`, `#market` open that view. Storage key is `familybank.v3` (namespaced because `<username>.github.io` is one origin shared by all your Pages projects).
