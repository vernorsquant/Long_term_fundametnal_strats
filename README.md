# Long-Term Fundamental Strategies

Research + backtests for simple, economically-motivated, long-term stock-selection
strategies — the portfolio-selection component of a larger system (regime detection
+ risk management live upstream). Compiled 2026-11-07.

## Contents

| Path | What it is |
|---|---|
| [`literature/REPORT.md`](./literature/REPORT.md) | Main literature report — ~55 fetch-verified sources across 9 rabbit holes (low-vol anomaly, weighting schemes, momentum/trend, insider/smart-money, value/quality composites, factor combination, seasonality/issuance anomalies, replication methodology, regime dependence) |
| [`literature/round1_findings.md`](./literature/round1_findings.md) | Raw round-1 research notes (5 facets) |
| [`literature/session_log.md`](./literature/session_log.md) | Research session timestamps |
| [`backtests/README.md`](./backtests/README.md) | LEAN backtest results, interpretation, methodology caveats |
| [`backtests/TOP3_STRATEGIES.md`](./backtests/TOP3_STRATEGIES.md) | The three chosen strategies + selection rationale |
| [`backtests/metrics.csv`](./backtests/metrics.csv) | Combined metrics table |
| `backtests/s1_composite.md` … `s3_gate_test.md` | Per-strategy reports + pre-registered gate test |

## The three strategies (LEAN, 2010-01 → 2026-08, $100k, 5bp/side)

| Strategy | Rule | CAGR | Sharpe | MaxDD |
|---|---|---|---|---|
| S1 Multi-Factor Composite | equal ranks: B/M + profitability + low-vol + 12-2 momentum; top decile, EW, 3% cap, monthly | 10.2% | 0.74 | −30.8% |
| S2 Net Payout Yield | (dividends + net buybacks)/mcap top decile, EW, annual | 12.0% | 0.68 | −37.9% |
| S3 SMA-Gated Momentum | 12-2 momentum, gated by index 200-day SMA | 8.9% | 0.55 | −36.0% |
| *SPY buy&hold (reference)* | — | 14.3% | 0.87 | −33.7% |

## Honest headline

SPY beat all three over this window — expected, since 2010→2026 is a growth-led
bull, the documented failure regime for value/low-vol defensive tilts. The claim
these sleeves test is full-cycle risk-adjusted robustness (S1: SPY-like
participation at 14.6% vol vs 17.1%), not bull-market relative return. The S3
SMA gate **failed** its pre-registered test (ΔSharpe −0.02 vs required +0.15) and
is reported as a negative result. Universe is a static 817-name snapshot with
survivorship bias — treat absolute numbers as upper bounds; relative ordering is
the usable signal. Details and caveats: [`backtests/README.md`](./backtests/README.md).
