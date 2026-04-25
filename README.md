# optionFlow

Single-file HTML/JS options trading dashboard for The Wheel Strategy and multi-leg trades. Fully client-side — drop `optionflow.html` in any browser and it runs.

## Features

- **Wheel Strategy hub** — CSP / CC suggestions tiered by safety (🟢🟡🔴), filtered by delta (0.15–0.40) and DTE (21–45)
- **Multi-leg builder** — iron condors, spreads, strangles with SVG payoff diagrams, net Δ/θ/vega, PoT, max profit, BPR
- **Trade Journal** — log opens/closes with partial-close support, roll simulator, edit, notes; persists in `localStorage`
- **Analytics tab** — realized & unrealized P&L, win rate, biggest win/loss, breakdown by strategy/ticker/month, cumulative P&L curve
- **🇮🇹 Italian 26% capital-gain tracker** — YTD-cumulative tax math (Jan 1 → Dec 31) with prior-year loss carryforward (4 years), per-month marginal tax view
- **IBKR statement importer** — drop an Activity Statement CSV → parses option trades, matches opens to closes (FIFO), reconstructs gross premium and open dates, deduplicates against existing journal
- **Demo mode** — synthetic Black-Scholes greeks and IV smile so the app is fully usable without a market-data API

## Getting started

```bash
# Open directly:
open optionflow.html

# Or serve locally:
python3 -m http.server 8765
# then visit http://localhost:8765/optionflow.html
```

No build step, no dependencies, no API keys required for demo mode.

## Importing IBKR statements

1. In IBKR Client Portal → Reports → Activity → run an Activity Statement for the period
2. Export as CSV
3. In optionFlow → **📥 Import** tab → drop the CSV
4. Review detected trades, click Import

The parser reads the **Trades** and **Open Positions** sections. Closing rows are matched FIFO against earlier opens in the same CSV to recover the actual open date and gross premium.

## Italian tax logic

Capital gains on options fall under *redditi diversi* — flat 26%, with losses carryforward up to 4 years against gains in the same category.

- Within a calendar year: losses from earlier months automatically offset gains in later months (Jan 1 → Dec 31 cumulative)
- Marginal tax for any given month = `(YTD tax owed at month-end) − (YTD tax owed at start-of-month)`
- Prior-year losses carry forward and are consumed first against the current year's gains before tax is computed

This is shown in:
- The **Trade Journal** monthly summary cards (when you pick a month from the pills row)
- The **Analytics → Monthly Detail** panel
- The **Analytics → Italian Tax History** table (year-by-year)

## Notes

- All trade data persists in `localStorage` only. There is no backend.
- This is a personal tracking tool, not investment advice or a broker.
- The included `.gitignore` excludes any `*ibkr*.csv` or `*statement*.csv` to prevent committing your account data.
