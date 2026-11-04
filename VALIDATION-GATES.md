# Multi-stage validation gates — before any trade or stock is suggested

Adopted Tue Nov 3, 2026, at Venu's request: build a robust prediction
system with multi-stage validation before suggesting a trade or stock.
It is a validation pipeline, not a crystal ball. It cannot predict a
close. What it does is refuse to publish a setup that has not survived
every check below, and it records which check caught each miss so the
system gets stricter where it has actually been wrong.

Every output is analysis, not financial advice. No position, purchase,
or method choice is ever assumed. Nothing here places trades or
connects to a broker.

## The rule that governs all gates

A name with any HARD gate failed gets **no entry, stop, or target
levels** — watch-only only, with the reason stated. A SOFT fail caps
rank, halves size, or forces pullback-only, as stated per gate. A gate
marked NOT RUN is never treated as a pass. Every picks file carries a
gate table per name: PASS / SOFT-FAIL / FAIL / NOT RUN, with the
evidence for each. The evening review scores the gates themselves:
for every miss, name the gate that caught it, or the gate that
should have caught it and did not — that becomes a tuning correction.

"Prediction" language is banned in outputs. A passing name is a
**validated setup**, never a prediction that the price "will" do
anything. The close is the judge, every day.

## Gate 0 — Data integrity (HARD)

- The regular-session close comes from a primary source (Finnhub bars),
  labelled with source and date. Aggregator figures are unverified until
  checked at the source.
- At least 200 daily bars exist for indicator and simulation work.
  Fewer bars → Gate 4 is NOT RUN and the name cannot be ranked above
  a name with a full history.
- If two vendors disagree by more than ~$0.25 on a decision line
  (entry edge, hold-above, invalidation), the straddling rule applies:
  read it conservatively (invalidated) and state the range. Never pick
  the vendor that keeps a zone alive. (Nov 3, 2026: ABBV $285.88–$286.24
  straddling $286 → invalidated; GILD $167.47–$168.43, all below $169 →
  vendor-robust failure.)

## Gate 1 — Trend (HARD; the 5-step screen's Step 1)

- Price above the 50- and 200-day averages with a rising 50-day;
  RSI(14) and MACD recomputed by the house method
  (`~/workspace/backtest/backtest.py`, simple-average RSI) with the
  latest bar appended. Vendor RSI/MACD figures are never mixed with
  house figures.
- RSI ≥ 70, or distance ~10%+ above the 50-day average, forces
  pullback-only entry, a no-chase ceiling, and half size. A narrowing
  MACD histogram on a name fading from a record high is cited as the
  warning; analyst target raises are never cited as support.
- A hold-above or invalidation condition is judged on the CLOSE only.
  An intraday break and reclaim is not a pass; a fade from a
  pre-market print to a close below the condition is the failure the
  rule exists to catch.

## Gate 2 — Fundamentals and cash (HARD; Steps 2–3)

- Latest quarter verified against consensus (revenue, EPS, guidance
  raised/held/cut); free cash flow positive and covering capex plus
  shareholder returns. FCF-negative on AI capex fails outright.
- SOFT-FAIL: one required item could not be verified (for example
  MA's Q3 free cash flow, unverified on Nov 3, 2026). A soft-failed
  name cannot rank above any name with a clean Gate 2, stays half
  size, and the unverified item is carried as could-not-verify until
  retrieved — it is never quietly dropped.

## Gate 3 — Independent corroboration (HARD, for decision lines)

- The close and every figure a level depends on are checked against a
  second independent source. Agreement within tolerance → PASS.
  Disagreement on a decision line → Gate 0's straddling rule decides,
  conservatively, with the range stated.
- Earnings dates are verified before publishing, from the earnings
  calendar or the company's own release. A vendor date that conflicts
  with a reported quarter (for example a listing showing a new date
  after the company already reported) is left unverified and re-checked,
  never assumed either way.

## Gate 4 — Outcome simulation (HARD for new zones; the reel's "Time Machine," honestly built)

- For each proposed entry zone, stop, and target 1, mechanically apply
  the levels to ~1 year (~250) of that name's own daily bars: every
  bar where the low first enters the zone is a simulated fill; follow
  the trade until the stop, target 1, or the end of the window.
- Report, with the sample size beside every rate: fills (N), stop-hit
  %, target-1-hit %, flat/unresolved % (still open at the window end
  — reported, never dropped), median holding time, and worst adverse
  excursion. Require N ≥ 5; fewer fills → "insufficient history,"
  NOT RUN, and no zone.
- Report **expected value per trade**: EV = (target-hit % × reward
  multiple) − (stop-hit % × 1) − 0.1R cost haircut. Require EV > 0.
  Hit rate alone never passes a setup.
- Worked example from the reel that prompted this gate (its own demo
  numbers, BUY at 10:32, target +2%, stop −1%): target 33%, stopped
  48%, flat 19%. EV = 0.33 × 2 − 0.48 × 1 = **+0.18R before costs** —
  positive but thin, and the stop hit more often than the target.
  That is the lesson: a 1:2 reward-to-risk setup can still be a
  marginal trade, and any simulation that reports only the win rate
  is hiding the trade.
- Limits, stated on every simulation: daily bars only — the intraday
  path inside a day is invisible, so gap risk through a stop is
  understated; no slippage, fees, or borrow costs beyond the haircut;
  past outcomes do not predict future ones; one year of bars is one
  market regime, not all of them.
- **Intraday version: NOT RUN until intraday bar data is verified.**
  The reel's demo replayed the rest of a trading day from historical
  intraday data. Daily bars cannot do that. For day-trade mode this
  gate stays NOT RUN and is never faked with daily bars; the
  opening-range plan stands on its own rules instead.

## Gate 5 — Adversarial validation (HARD; the Validation lead)

- Every suggestion carries a bull case, a bear case, the missing
  evidence, and the named falsifier: what single fact would change
  the verdict. If no bear case or no falsifier can be stated, the
  name does not publish.
- The thesis-breaking headline is named before entry (for example a
  de-escalation headline for a war-driven energy thesis) and treated
  as an exit even if targets are unmet.
- Sector read-through is checked: a print from a peer (for example
  AMD's print bearing on an MSFT re-screen) is judged explicitly,
  never assumed to help or hurt.

## Gate 6 — Danger and sentiment veto (HARD)

- The public-sentiment danger rule applies at this gate: concerning
  news or clearly low sentiment = no top-2 rank, pullback-only, stop
  tightened to nearest support, size halved. Severe danger (two
  independent concerning items, or a sentiment collapse) = watch-only
  the same day, with the reason stated.
- A danger flag on a name carried from a prior day states explicitly
  whether to exit early at the next open or hold to the stop — never
  silently hold. Positive sentiment never promotes a name that fails
  Gates 1–4.
- Social tone on X/Reddit/Stocktwits is stated as unmeasured when it
  is unmeasured (social search here covers Instagram, Facebook, and
  Threads only). Never paper over that gap.

## Gate 7 — Risk and sizing (HARD)

- Reward-to-risk at least 1:1.5, preferring 1:2, measured from the
  zone top to target 1 against zone bottom to stop. Below 1:1.5 → no
  zone, however strong the trend.
- The stop sits BELOW the entry zone, never at its bottom edge. Risk
  ~1% of account per trade, with the arithmetic shown. Correlation
  check: two names in the same sector/theme (for example V and MA,
  both payment networks) are sized as one exposure, not two.
- Every zone carries a no-chase ceiling, close-judged conditions, and
  a gap-risk note: an opening gap can blow through entry conditions,
  so a limit order without its hold-above condition attached is not a
  valid entry. Earnings binaries inside the holding window exclude
  entry before the print.

## Gate 8 — Publication audit (HARD)

- Every number carries source + as-of date. Stale closes are never
  presented as current prices. A could-not-verify list is published
  with every output, including what was not checked.
- No level moves on an unconfirmed report, draft, or headline. No
  recycled prior-day numbers when live quotes or news cannot be
  verified. On a market holiday the run stays quiet with a one-line
  note.

## Per-mode application

- **Swing (1–3 month holds):** all eight gates, every morning, on the
  full 35-name universe funnel. First full gate-run: the Wed Nov 4,
  2026 morning screen (Gate 4's first run on fresh bars; the
  dashboard's Outcome Simulator already computes the same quantities
  from stored bars and is the display surface once gate results exist).
- **Day trading (intraday, flat by close):** Gates 0, 1 (intraday
  levels + prior-day range), 3, 5, 6, 7, 8. Gate 2 is the earnings
  calendar check (no entry into a print). Gate 4 is NOT RUN until
  intraday bars are verified — stated, never faked.
- **Long-term (12+ month holds):** Gates 0, 2, 3, 5, 6, 8, plus the
  thesis/valuation review in `HOLDINGS.md`. Gate 4 uses multi-year
  monthly bars and drawdown, not 1-year daily simulation; Gate 1
  trend conditions do not apply to a thesis hold the way they do to
  a swing entry.

## Audit loop (how the gates get stricter)

The evening review scores every gate, not just every pick. For each
miss it records: which gate caught it, or which gate should have
caught it. Three consecutive misses caught by the same gate → that
gate's threshold tightens in the tuning log. A miss no gate caught →
a new gate or a widened gate is proposed in the tuning log before the
next morning screen. Gate changes are recorded newest-first in
`~/workspace/swing-daily/ALGORITHM.md`, and the live state stays
there; this file is the definition, not the state.

## What this system deliberately does not do

- No auto-execution, no broker connection, no order placement. The
  reel's Order Executor role is rejected, as before.
- No scanning thousands of names: coverage stays at what bar data
  actually supports, with the funnel counts reported honestly
  (names screened → names passing each gate).
- No 10,000-session claim: simulations run over the name's actual
  historical bars, N reported, unresolved trades reported. A Monte
  Carlo resample is not used unless its method is written down first
  and its assumptions stated — it is not used today.
