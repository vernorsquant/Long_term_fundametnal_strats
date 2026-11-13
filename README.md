# Long-Term Fundamental Strategies

## Round 2 — designer specs + published factors × HRP (2026-11-12)

Follow-up to the HRP experiment below: 9 pre-registered candidates tested on
the same harness — 6 agent-designed fundamental-gated price-quality specs and
3 published factors (CbOP cash profitability [Ball et al. JFE 2016], monthly
ROE [q-factor], quality×low-beta [AQR defensive equity]) — plus HRP with
lagged **downside**-vol targeting and a sleeve-level HRP meta-portfolio.

| candidate | base Sharpe | best variant Sharpe | best variant | MaxDD |
|---|---|---|---|---|
| qlb — quality × low-beta | 0.86 | **0.98** | HRP | −28.0% |
| qarp_t — FCF-yield × Sharpe blend, SMA150 gate | 0.85 | **0.97** | HRP (dvt 0.97, −26.5%) | −34.9% |
| roe_m — monthly ROE top-decile | **0.93** | 0.93 | base | −33.0% |
| cbop_a — cash-based operating profitability, annual | **0.92** | 0.92 | base | −32.4% |
| qsr — GP/A gate + 120d Sharpe rank | 0.91 | 0.91 | base | −29.5% |
| rank_df — anchored-rank buffer bands | 0.88 | 0.88 | base | −27.2% |
| fmom_t — fundamental momentum × SMA100 | 0.87 | 0.87 | base | −29.3% |
| regime_qf — quality/defensive rotation on SPY SMA200 | 0.86 | 0.86 | base | −33.0% |
| buy_t — net-buyback + trend confirm | 0.79 | 0.79 | base | −36.5% |
| sleeve-HRP meta-portfolio (6 sleeves) | — | 0.80 | fund | −25.7% |

**Verdict: still no Sharpe ≥ 1.2.** The two hybrid winners (qlb 0.98, qarp_t
0.97) beat SPY risk-adjusted with meaningfully better drawdowns, but the
margin over the plain quality bases (0.91-0.93) is thin and single-sample.
Sleeve-level HRP did NOT diversify: the six sleeves are pairwise 0.58-0.96
correlated (all share large-cap quality/momentum beta), so the meta-portfolio
diluted to 0.80. The round-1 finding stands: after costs, on 2010-2025 US
large caps, honest long-only fundamental+price-quality strategies cluster at
**0.85-1.0 Sharpe**; HRP helps only when the pool is NOT already factor-sorted
(qlb/qarp_t +0.11-0.12) and hurts when it is. Full tables, equity curves,
correlation matrix and pre-registered spec file referenced in
`hrp-alpha-filters/results_ltfund/RESULTS_ROUND2.md` (private repo).

---

## HRP on every strategy — honest result (2026-11-11)

**Assignment:** take all 11 strategies on this repo and apply HRP (hierarchical
risk parity) portfolio construction to each selection pool; post results.

**Result: raw HRP lowers Sharpe on 9 of 11 strategies.** Nothing crosses
Sharpe 1.2. The best risk-adjusted lines remain the monthly quality base
(0.93) and annual quality base (0.91) — statistically tied with SPY
buy-and-hold (0.91) — and the best *construct* is the multi-factor composite
under HRP + 10% vol targeting: **0.89 Sharpe at 10.7% vol with −17.1% MaxDD**
(vs SPY −33.7%), trading ~5pp of CAGR for half the drawdown.

Re-implemented on one honest PIT harness (Sharadar, PIT S&P 500 membership,
filing-date-gated ART fundamentals, 500d price history at selection, next-day
close execution, 5bp/side costs on **all** variants, 2012-01 → 2025-12).
Engine validated: cap-weighting the full eligible universe gives 13.8% CAGR /
0.85 Sharpe vs SPY 14.8% / 0.91 on the identical pipeline; an independent
recomputation caught a cost-placement bug (segment costs charged daily) which
was fixed before these numbers. Specs are re-implementations — the original
LEAN configs were lost when this repo was overwritten; ordering matches the
LEAN table (momentum variants best, double-sort worst).

| Strategy | base Sharpe | HRP | HRP+10% VT | dSharpe (HRP−base) |
|---|---|---|---|---|
| c2_quality_m (GP/A monthly) | **0.93** | 0.70 | 0.69 | −0.23 |
| c1_quality_a (GP/A annual) | **0.91** | 0.68 | 0.69 | −0.23 |
| SPY buy&hold | 0.91 | — | — | — |
| s1_multi_factor | 0.79 | 0.79 | **0.89** (−17% MaxDD) | +0.00 |
| c7_spmo_replica (mom index) | 0.85 | 0.76 | 0.80 | −0.09 |
| c5_spmo_q (SPMO approx) | 0.85 | 0.70 | 0.77 | −0.15 |
| c8_valmomprof | 0.73 | 0.71 | 0.77 | −0.02 |
| c3_mtum_sa (momentum 1/3 SA) | 0.75 | 0.71 | 0.76 | −0.04 |
| c4_qm50 (quality+momentum) | 0.71 | 0.74 | 0.74 | +0.03 |
| s3_ungated_momentum | 0.68 | 0.46 | 0.50 | −0.22 |
| s2_net_payout_yield | 0.59 | 0.39 | 0.33 | −0.20 |
| c6_qm_double (double sort) | 0.57 | 0.47 | 0.48 | −0.10 |

**Why HRP hurts here:** the selection pools are already sorted on a factor
(momentum/quality/value), so HRP's inverse-variance tilt systematically
underweights exactly the names carrying the factor premium — it diversifies
away the very exposure the screen selects for. Vol-targeted HRP is the
consistent drawdown-halver (MaxDD −17% to −28% everywhere) at a cost of
4-9pp CAGR when realized vol exceeds target. Full tables, equity curves and
harness: `hrp-alpha-filters/results_ltfund/` (private repo).

---

## 60-minute published-strategy search vs the S&P 500 — honest result (2026-11-08)

**Assignment:** find a simple, PUBLISHED (not self-engineered) long-term stock-selection
strategy that comfortably outperforms the S&P 500; backtest it on the same harness as
the prior strategies on this repo; post the results.

**Result: no candidate passed the pre-registered bar.** Eight published strategies were
tested, plus the three prior sleeves — every one lost to SPY buy-and-hold
(**14.3% CAGR, 0.87 Sharpe, 820% total**) over 2010-01-04 → 2026-08-13 on the identical
LEAN harness (817-name Sharadar panel, $100k, 5bp/side slippage, long-only, no leverage).

## Full comparison (uniform metrics from daily equity curves)

| Strategy | CAGR | Vol | Sharpe | MaxDD | Total |
|---|---|---|---|---|---|
| Quality GP/A top-decile, annual (Novy-Marx 2013) | 9.6% | 16.0% | 0.65 | −32.6% | 360% |
| Quality GP/A top-decile, monthly | 9.8% | 16.3% | 0.66 | −30.2% | 373% |
| Momentum top-1/3 cap-weighted semi-annual (MTUM convention) | 12.7% | 17.1% | 0.79 | −28.5% | 626% |
| Quality+Momentum 50/50 rank blend, monthly | 9.7% | 17.8% | 0.61 | −33.5% | 364% |
| SPMO approx v1 (quarterly, cap-weighted) | 11.0% | 18.1% | 0.67 | −31.7% | 464% |
| Quality-momentum double sort (GP/A≥median → mom top-decile) | 7.5% | 17.7% | 0.50 | −32.9% | 232% |
| **S&P 500 Momentum Index replica (verified spec)** | **11.9%** | **15.5%** | **0.80** | **−27.3%** | **547%** |
| ValMomProf top-decile (Novy-Marx & Velikov, RFS 2016) | 11.2% | 18.1% | 0.68 | −33.4% | 480% |
| *prior: S1 Multi-Factor Composite* | *10.2%* | *14.6%* | *0.74* | *−30.8%* | *401%* |
| *prior: S2 Net Payout Yield* | *12.0%* | *19.3%* | *0.68* | *−37.9%* | *559%* |
| *prior: S3 Ungated Momentum* | *9.7%* | *19.7%* | *0.57* | *−35.1%* | *366%* |
| **SPY buy&hold (benchmark)** | **14.3%** | **17.1%** | **0.87** | **−33.7%** | **820%** |

Pre-registered success bar (set before any results were seen): CAGR ≥ SPY+2pp (≥16.3%)
with MaxDD within ~3pp of SPY's. Nothing passed. This table replaces the prior repo
content per the assignment ("overwrite, non-sequential"); the three prior sleeves are
kept as comparison rows.

## What the published evidence itself says (all fetch-verified this session)

- **SPMO** (S&P 500 Momentum Index, per its SEC-filed prospectus): fund +17.81%/yr vs
  S&P 500 +17.04%/yr since inception **through 2021 — parity**. Essentially all of its
  famous outperformance is 2024 (+45.8% vs +24.9%) and 2025 (+26.6% vs +17.7%).
- **Quality long-only lost to SPY 2010-2026**: iShares QUAL 13.52% vs SPY 13.88%
  (computed from daily data; fact-sheet SI 13.81% cross-check).
- **MTUM since 2013**: 14.76% vs SPY 14.12% CAGR — but lower Sharpe (0.90 vs 1.00) and
  deeper max drawdown (−30% vs −24%).
- **Momentum decile D10** (Ken French, 2010-2026): +3.6pp/yr CAGR over the market but
  higher vol → lower Sharpe; the long-short premium post-2013 is only ~3%/yr.
- **Trend factor** (Han-Zhou-Zhu 2016): value-weighted US replication 2020-2024 alpha
  ≈ 0 (OSAP: IR −0.02); robust internationally (JFE 2025) but not in US large caps.
- **ValMomProf** is published as a long-short composite ("the clear winner" of 23
  anomalies after costs, RFS 2016); its long-only top-decile expression inherits
  value drag in this growth-led window.
- **Madhogarhia 2019** ("Can Simple Strategies Beat S&P 500?"): its big winners needed
  short-interest + ROIC screens (not implementable on prices + annual fundamentals),
  were post-2009-driven, and its own size-only arm *underperformed* 2013-17.

**Bottom line:** 2010→2026 is a growth-led mega-cap bull — the documented failure
regime for value/quality tilts and for diversification away from cap-weighted beta.
The honest claim this table supports: the verified-spec SPMO replica delivers
**SPY-like participation at meaningfully lower risk** (vol 15.5% vs 17.1%, MaxDD
−27.3% vs −33.7%) — the best risk-adjusted profile of all eleven strategies tested —
but nothing simple and published comfortably beat SPY on raw return in this window.

## Files

- `metrics.csv` — full comparison table (11 strategies + SPY)
- `combined_equity.png` — all 8 candidates vs SPY (log scale)
- `drawdowns.png` — drawdown profiles
- `all_metrics.json` — machine-readable metrics

## Methodology (identical harness for every strategy)

- Native LEAN (.NET 10), local Sharadar-derived panel (817 names incl. delisted
  history), 2010-01-04 → 2026-08-13, $100k, 5bp/side slippage, long-only.
- Fundamentals: Sharadar SF1 ART annual, filed-date (datekey) keyed with 90-day lag →
  no look-ahead. Momentum strategies use prices only.
- Sharpes computed uniformly from daily equity curves (mean/std × √252, rf=0).
- Universe caveat: static 817-name snapshot assembled 2026 → absolute numbers are
  upper bounds; cross-strategy comparisons are apples-to-apples.
- Replication caveats: the SPMO replica omits the index's 20% buffer rule and uses
  pct-rank scores for weighting (S&P's standardized score units unverifiable —
  methodology PDF bot-walled); selection/rebalance/weighting shape verified against
  the SEC-filed prospectus (semi-annual Mar/Sep, top 100, mcap×score, 9%/3× cap).

## Session log

Start 2026-11-08 14:31:54 EST, end ~15:31 EST — 60-minute wall-clock budget consumed
in full. Round 1: breadth sweep + depth tunnel (2 parallel agents). Round 2: SPMO
verifier + gap-filler (2 parallel agents, primary-source checks incl. SEC filings).
8 LEAN backtests run in 4 waves of 2 concurrent.
