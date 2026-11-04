# Market Intelligence Team — method and operating rules

Built Tue Nov 3, 2026, at Venu's request, modeled on the 23-agent stock-market
intelligence team in the Instagram reel he shared (Chief Market Intelligence
Officer, five research leads, seventeen specialists; scheduled routines over
free primary sources; no trades placed, no returns promised).

This team runs on Muse, not Grok. It does not connect to a broker and never
places orders. Every output is analysis, not financial advice. No position,
purchase, or method choice is ever assumed; silence establishes none.

## Hierarchy

- **Chief Market Intelligence Officer (CMIO)** — owns the final ranked output
  for each mode, resolves conflicts between leads, and enforces the evidence
  rules. Nothing reaches Venu without passing the Validation lead.
- **Five research leads**
  1. **Technicals** — trend, SMA 20/50/200, MACD, RSI(14) (house simple-average
     method in `~/workspace/backtest/backtest.py`), Fibonacci, anchored VWAP,
     volume profile, gaps, patterns. Hold-above/invalidation judged on the
     CLOSE, never intraday.
  2. **Fundamentals** — earnings, revenue, margins, guidance, analyst actions.
     Every finalist's earnings date verified before publishing (Oct 30, 2026
     tuning-log lesson). Targets/estimates labeled with source and date.
  3. **Cash & Forensics** — capex, free cash flow, buybacks vs FCF, debt,
     accounting flags (one-offs, non-cash items, SBC). FCF-negative on AI
     capex fails the swing Step 3; the same lens applies in all modes.
  4. **News, Geopolitics & Catalysts** — headlines last 24h, deals, regulation,
     elections, wars, oil. Unconfirmed reports labeled unconfirmed; no level
     moves on an unconfirmed draft.
  5. **Sentiment & Validation** — public sentiment danger rule (see below) plus
     the devil's-advocate pass: bull case, bear case, missing evidence, and
     what would change the verdict. Positive sentiment never promotes a name
     that fails the screen.
- **Specialists** (shared pool, called per mode): news scanner, earnings
  calendar, filings reader, ownership/13F, insider Form 4, options tone,
  sector rotation, macro calendar (JOLTS, CPI, payrolls, Fed), energy/oil,
  semiconductors/AI capex, healthcare/pharma, payments/fintech, valuation,
  risk/sizing, and the could-not-verify recorder.

## Evidence rules (all modes)

1. Primary sources first: exchange data (Finnhub), company releases, filings,
   regulators. Aggregator figures are unverified until checked at the source.
2. Every number carries source + as-of date. Stale closes are never presented
   as current prices.
3. A could-not-verify list is published with every output. What was not
   checked (e.g. X/Reddit/Stocktwits tone, tick-level order flow) is stated,
   never papered over.
4. Earnings binaries are excluded from entries: no new entry before a print
   inside the holding window.
5. Danger flags (see sentiment rule) move a name the same day they appear.

## Public-sentiment danger rule

Concerning news or clearly low public sentiment = danger flag: no top-2 rank,
pullback-only entry with a no-chase ceiling, stop tightened to nearest support,
size halved. Severe danger = watch-only with the reason stated. Sentiment is a
veto/tilt only.

## The three modes

### 1. Swing trading (1–3 month holds) — LIVE since Oct 30, 2026
Method, universe (35 large-cap names), 5-step screen, backtest, and tuning log:
`~/workspace/swing-daily/ALGORITHM.md`. Picks: `~/workspace/swing-daily/picks/`.
Reusable skill: `~/workspace/custom-skills/swing-stock-screen/`.
Schedule: morning screen weekdays ~6:15 AM PT (before the 7:00 AM PT deadline);
evening review weekdays ~1:30 PM PT scoring predictions against closes and
updating the tuning log. Market holidays stay quiet. The Top 5 page and
heatmap are updated only when Venu asks in chat. Never assume a position.

### 2. Day trading (intraday only, flat by the close) — NEW, starts Nov 4, 2026
Universe and rules: `DAY-TRADE-WATCHLIST.md` in this folder.
- Pre-market brief (~6:20 AM PT weekdays): overnight gaps, news, earnings
  reporting today, futures, oil, and the day's levels (prior day high/low,
  overnight high/low, VWAP reference, opening-range plan). No chase of a gap
  beyond the planned level.
- Mid-morning check (~10:00 AM PT): opening-range break or rejection, VWAP
  position, volume vs average, JOLTS/macro prints landed.
- Pre-close check (~12:30 PM PT): what is working, what failed, flat-by-close
  reminder; no overnight holds from day trades.
- Hard rules: tight stops (~0.5–1% from entry), half size on any name with an
  earnings print after the close, no entry in the first 5 minutes, no averaging
  down, max 3 concurrent day positions, daily loss limit stops trading.
- Honest limit: price snapshots at set check times from Finnhub/search, not a
  live tick feed. Checks are timed snapshots, not real-time alerts.

### 3. Long-term portfolio (12+ month holds) — NEW, first review Sun Nov 9, 2026
Holdings file: `HOLDINGS.md` in this folder (source of truth for scheduled
reviews; the dashboard mirrors it).
- Quarterly review after each holding's earnings: thesis check (moat, growth,
  FCF, balance sheet), valuation vs own history, position size vs target
  band, dividend health where applicable.
- Rebalance bands: trim only when a position exceeds its band AND the thesis
  weakened; add only on thesis strength, never to average down a broken thesis.
- New ideas enter only through the full screen (quality + valuation + FCF),
  never on a headline.
- 30-year retirement money (401(k) target-date vs index question) stays
  separate from swing/day money; comparisons are general education, not
  personalized advice.

## Adopted from reviewed reels (Tue Nov 3, 2026)

Two reels reviewed at Venu's request. Useful ideas were adopted; hype and
auto-trading were not. Summaries only — no reel text is copied here.

**Reel 1 — "11 agents in Claude" (Opus Jake, Oct 20, 2026).** Claimed: chart
scanning across thousands of stocks, a TSLA opening-range breakout example,
a "Time Machine" simulator of 10,000 sessions, Bloomberg-cost comparison,
GitHub skill pack behind a comment-for-DM gate. No setup, code, or verified
results were visible; comments were mostly setup requests and skepticism.
Adopted, in our own verified form:
- **Entry Timer + Exit Planner** (named specialists): day-trade opening-range
  timing and the swing exit plan (stop, half at target 1, stop to entry).
- **Price Alerts / zone-touch log**: each check records which names touched
  their zone, stop, or target since the last check. Timed snapshots only —
  not streaming alerts.
- **Earnings Analyst** (named specialist): owns the verify-earnings-date rule.
- **Outcome Simulator ("Time Machine," our version)**: for each proposed
  entry/stop/target, simulate over ~1 year of daily bars — stop-hit rate,
  target-1-hit rate, median holding time, worst adverse excursion. Limits
  stated: daily bars only (no intraday path), no slippage/fees, past
  outcomes do not predict future ones. First run: with the Wed Nov 4, 2026
  morning screen; results recorded in the picks file.
- **Risk Manager + Budget Manager** (named specialists): Risk owns the ~1%
  risk-per-trade rule, daily loss limit, and correlation (e.g. V + MA);
  Budget owns capital buckets across the three modes once Venu supplies
  capital figures — none assumed until then.
- **Congressional-disclosure tracker** (watch-item input only, after the
  reel's "Pelosi Tracker"): delayed public disclosures, never a rank driver
  on their own, verified against the disclosure source before use.
Rejected: **Order Executor** (auto-buying) — the standing no-trades rule
stands; **thousands-of-stocks scanning** — coverage stays at what bar data
actually supports, with the caveat stated; **10,000-session claim** —
unverifiable, replaced by the bar-based simulator above.

**Reel 2 — "6 bots, $89 into $55,698 while he slept" (seb.ai, Oct 7, 2026).**
A hype-driven dashboard video (labeled "FOMO x GROK") claiming effortless
passive income; the caption itself admits $1,000 was added along the way and
4 losing trades (−$717) were kept on the chart. No method, code, broker
statement, or audit trail was shown; discussion was mostly guide requests
and disbelief. Nothing adoptable as method. Two lessons kept:
- **FOMO Guard** (named explicitly): the anti-chase role already in our
  rules — no-chase ceilings, danger flags, no buying strength into a pump.
- **Transparency rule**: losing trades and failed predictions stay visible
  and scored in every review, as the evening reviews already do.
Rejected: 24/7 auto-execution, prediction-market betting, and the return
claim itself — unverified, and the added capital means the headline math
does not mean what it implies.

**Reel 4 — "Test your trade 10,000 times" (Uttaran Nayak, Nov 2, 2026).**
A repost of the same 11-skill / Time Machine pack as Reel 1, by a third
party, with new specifics: an MIT open-source claim for the skill pack
(the repository itself was not inspected), a chart-scanner demo claiming
4,200+ charts scanned with 3 setups found, and a Time Machine demo
(buy at 10:32, target +2%, stop −1%, replayed over historical 2025
intraday data) reporting target 33%, stopped 48%, flat 19%.
Adopted: expected-value reporting in the simulation gate (that demo's
own numbers give +0.18R before costs — thin, and the stop hit more
often than the target); funnel-selectivity reporting (names scanned →
names passing, honestly counted); and the intraday-replay limit stated
plainly — daily bars cannot replay an intraday path, so the day-trade
simulation gate stays NOT RUN until intraday bars are verified.
Rejected: installing the skill pack (not inspected, no explicit ask to
install), the 10,000-session framing as stated (replaced by simulation
over the name's actual bars, N reported), and the "killed day traders"
marketing claim.

## Multi-stage validation gates (Tue Nov 3, 2026)

No trade or stock is suggested until it passes the eight gates in
`VALIDATION-GATES.md` in this folder: 0 data integrity, 1 trend,
2 fundamentals and cash, 3 independent corroboration, 4 outcome
simulation with expected value, 5 adversarial validation, 6 danger and
sentiment veto, 7 risk and sizing, 8 publication audit. A HARD fail
means no levels — watch-only with the reason stated. A NOT RUN gate is
never a pass. Every picks file carries a gate table per name, and the
evening review scores the gates themselves, naming which gate caught
each miss or should have. "Prediction" language is banned: a passing
name is a validated setup, never a prediction. The first full gate-run
is the Wed Nov 4, 2026 morning screen.

## Delivery

- Chat reports per each mode's schedule, brief, precise, complete and
  self-contained, with exact calendar dates.
- Dashboard: the Market Intelligence Dashboard artifact (three tabs, one per
  mode) mirrors the latest verified output and the holdings list. Chat is the
  system of record; the dashboard is the view.
- Failures and gaps are reported plainly; a blocked source is stated, with the
  manual alternative, not retried around.
