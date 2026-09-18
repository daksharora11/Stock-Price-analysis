# Momentum watchlist

A 13-stock tracker that runs on GitHub Pages. A scheduled GitHub job fetches delayed quotes every 15 minutes during US market hours and republishes the page, so prices, "today" moves, the 52-week range and every buy/wait/stop signal stay current without you touching anything.

**What updates by itself:** prices, prior close, 52-week range, the market strip at the top, and all signals.
**What does not:** news, analyst ratings, market cap, P/E and the "buy when / sell when" notes. Those are from the Sept 18 review and only change when `index.html` is edited.

This is a research aid, not investment advice.

## Setup (about 10 minutes, no coding)

1. **Create the repository.** On github.com click **New repository**. Name it `momentum-watchlist`, choose **Public**, and click **Create repository**. (GitHub Pages is free only for public repositories. Nothing personal is in this project.)

2. **Upload the files.** Unzip the download. In your new repo click **Add file → Upload files**, drag in everything from the unzipped folder (`index.html`, `data`, `scripts`, `setup`, `README.md`, and the `.github` folder), then click **Commit changes**.
   - On a Mac the `.github` folder is hidden in Finder. Press **Cmd + Shift + .** to show it, then drag it in with the rest.
   - If GitHub still doesn't show `.github/workflows/update-prices.yml` after the upload: click **Add file → Create new file**, type `.github/workflows/update-prices.yml` as the file name (typing the slashes creates the folders), paste in the contents of `setup/update-prices.yml`, and commit.

3. **Turn on Pages.** Go to **Settings → Pages**. Under **Build and deployment**, set **Source** to **GitHub Actions**.

4. **Run it once.** Open the **Actions** tab, click **Refresh prices and deploy** in the left list, then **Run workflow → Run workflow**. (If a run from your first upload already failed because Pages wasn't on yet, just run it again.) When the run shows a green check, your site is live at:

   `https://YOUR-GITHUB-USERNAME.github.io/momentum-watchlist/`

After that it refreshes on its own every 15 minutes on weekdays from 13:00 to 21:59 UTC.

## Using the page

- Type a price into any row (or use **Paste prices**) to override the feed for that stock. Your typed price is used until the next feed update replaces it.
- **Set baseline to current prices** re-centres each stock's levels and "since baseline" on today's prices. **Reset my edits** puts the Sept 18 baseline back.
- Your edits and open rows are saved in your own browser only.
- The page re-checks the feed every 5 minutes while it is open, and **Reload prices** does it on demand.

## Changing the stock list

Tickers appear in two places, and both need the same change:
1. `scripts/update_prices.py`: the `TICKERS` list at the top.
2. `index.html`: the `STOCKS` list in the script section (each entry holds the name, snapshot price, news and plan text).

## If something looks wrong

- **The page says "Snapshot" and not "Prices updated":** the workflow hasn't run successfully yet. Check the **Actions** tab for a red X and open the run for details.
- **"Using the Sept 18 snapshot for …" in the header:** the quote source didn't return those tickers on the last run. It usually clears on the next run.
- **Prices look 15+ minutes old:** quotes are delayed about 15 minutes, and GitHub can start scheduled runs several minutes late.
- **It stopped refreshing after a couple of months:** GitHub pauses scheduled workflows in public repos with no activity for 60 days. Open the Actions tab and re-enable it, or click **Run workflow**.
- **The quote source stops working:** the script uses Yahoo Finance's unofficial public endpoint, with Stooq as a per-ticker fallback. Neither is guaranteed. A free API key from a provider such as Finnhub would be a sturdier replacement.
