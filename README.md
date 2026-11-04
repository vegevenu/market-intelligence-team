# Market Intelligence Team

A stock-market research system built on Muse: one team definition, three
modes (swing trading, day trading, long-term portfolio), and an
eight-gate validation pipeline that every suggestion must pass before
it is published. It does not place trades, connect to a broker, or
promise returns. Every output is analysis, not financial advice.

## Files

- `TEAM.md` — the team definition: Chief Market Intelligence Officer,
  five research leads, shared specialists, evidence rules, the
  public-sentiment danger rule, and the three modes with their
  schedules. Includes the roles adopted from reviewed social-media
  posts about AI trading teams, and what was rejected from them.
- `VALIDATION-GATES.md` — the eight validation gates (data integrity,
  trend, fundamentals and cash, independent corroboration, outcome
  simulation with expected value, adversarial validation, danger and
  sentiment veto, risk and sizing, publication audit). A name with a
  hard gate failed gets no entry, stop, or target levels.
- `DAY-TRADE-WATCHLIST.md` — the day-trading universe and hard rules:
  timed snapshots (not a live tick feed), tight stops, no averaging
  down, flat by the close.

## What is intentionally not in this repo

- Holdings and positions. The holdings file is the source of truth for
  scheduled long-term reviews and lives locally only; it is never
  pushed here.
- Live state. The swing method's tuning log, daily picks, backtest
  outputs, and review scorecards change every trading day and live
  locally only.
- The reusable swing-screen method. The screen skill lives in its own
  repository: https://github.com/vegevenu/swing-stock-screen

## How it runs

Scheduled runs (morning screen, evening review, day-trade checks,
weekly long-term review) work through the team roles in order and
deliver one self-contained report per run, each with a
could-not-verify list. A chat report is the system of record; the
dashboard is the view.
