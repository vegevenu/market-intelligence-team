# Day-trade watchlist and rules

Starts Wed Nov 4, 2026. Intraday only: every day trade is flat by the close.
No overnight holds from day trades. Analysis, not financial advice; no
positions assumed; no orders placed.

## Universe (liquid large-caps only; all must trade >1M shares/day)

Core: the swing universe's most liquid names — MSFT, AAPL, NVDA, AMZN, GOOGL,
META, AVGO, TSM, V, MA, JPM, XOM, LLY, GILD, ABBV, COST, NFLX, MU, AMD, PLTR.
Earnings-day names are excluded from new day entries before their print.

## Daily routine

1. **Pre-market brief (~6:20 AM PT):** overnight gaps >1%, news, earnings
   reporting today (before open / after close), futures, oil, 10-year yield.
   Levels: prior-day high/low, overnight high/low, VWAP reference from prior
   close, opening-range plan. No chasing a gap beyond the planned level.
2. **Mid-morning check (~10:00 AM PT):** opening-range break or rejection,
   price vs VWAP, volume vs 20-day average, macro prints landed (e.g. JOLTS).
3. **Pre-close check (~12:30 PM PT):** what worked, what failed, flat-by-close
   reminder, levels carried into tomorrow's swing screen if relevant.

## Hard rules

- Tight stops: ~0.5–1% from entry, set at order time in the plan; no averaging
  down.
- Half size on any name reporting earnings after the close that day.
- No entry in the first 5 minutes of the regular session.
- Max 3 concurrent day positions. Daily loss limit: stop day trading after
  two stopped-out trades.
- A gap at the open can invalidate a level; a limit without its condition is
  not a valid entry.
- Sentiment danger rule applies (see TEAM.md): severe danger = no day trade.

## Limits (stated plainly)

Price snapshots at set check times (Finnhub/search), not a live tick feed.
No broker connection, no streaming quotes, no automated execution. If a level
moves between checks, the next check restates it; nothing is claimed
intraday that was not observed.
