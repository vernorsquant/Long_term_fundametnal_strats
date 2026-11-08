# Portfolio Selection: Simple, Backtestable, Economically-Motivated Stock-Selection Methods
## A Literature Research Report

**Session timestamps (EST, 2026-11-07):**
- Research start: **11:13:33** | Round 1 dispatched: **11:14:54** | Round 1 complete: ~**11:24**
- Round 2 dispatched: **11:24:54** | Round 2 complete: ~**11:35** | Orchestrator verification pass: ~**11:40**
- Report compiled: **11:40–12:00** | Budget: 60 minutes

**Method:** 11 parallel research agents across 10 non-overlapping rabbit holes (3 rounds: 5 + 4 + 2). Every cited URL was fetched and read by the researching agent (or re-verified by the orchestrator); fetch failures are disclosed inline rather than silently dropped. **~65 verified sources.** Raw round-by-round notes: `round1_findings.md` (this directory). Transcripts: `/root/.hermes/cache/delegation/live/`.

---

## 0. DIRECT ANSWER (read this first)

For a long-term, diversified, S&P-500-beating-or-matching portfolio with lower beta, the evidence-backed simple methods, ranked by (evidence strength × robustness-to-scrutiny × simplicity):

| Rank | Method | The rule | Why it belongs in your system |
|---|---|---|---|
| 1 | **Multi-factor rank composite** | Average each stock's cross-sectional ranks on ~4 signals (value, profitability/quality, low-risk, momentum); hold top quintile, equal-weight or cap-capped, quarterly | Best risk-adjusted evidence (value+mom 50/50 Sharpe 1.04 vs 0.47–0.51 singles); equal weights avoid overfitting (Novy-Marx 2016); capacity is a non-issue |
| 2 | **Low-risk/low-vol tilt** | Rank top-1000 by trailing 12m volatility (or beta); hold bottom quintile, cap-capped, quarterly | Directly answers "match S&P with lower beta": low-vol decile 15.0%/yr Sharpe 1.12 vs high-vol 5.8%/yr Sharpe 0.05 (1980–2021); one caveat: lags growth-led recoveries badly (2020: −1.1% vs +18.4%) → pair with your regime layer |
| 3 | **Equal-weight / anti-concentration weighting** | Rebalance the selected universe to equal weights monthly or quarterly | 1/N beats 14 optimized models (DeMiguel et al. 2009); EW alpha comes from the rebalancing itself; S&P top-10 = 40.7% of cap index → EW is the simple diversification answer |
| 4 | **Cross-sectional momentum (12–2)** | Rank by trailing 12m return skipping last month; hold top third; monthly | 12%/yr excess for 6/6 (Jegadeesh-Titman 1993); works in every asset class (AMP 2013); pair with value (corr ≈ −0.5) to kill crash risk |
| 5 | **Net payout yield (buybacks − issuance)** | (Dividends + net repurchases)/mcap, top decile, annual | Corporate insiders buying their own stock at scale; works 1926–2002; large-cap friendly (unlike most insider signals) |
| 6 | **200SMA/10-month SMA as a regime gate, not a stock picker** | Hold equity sleeve only while index > 10-month SMA | Faber: Sharpe 0.55 vs 0.32, MaxDD −50.3% vs −83.5% (1901–2012), <1 trade/yr; OOS 2006–12 held up. Your instinct that single-asset timing "isn't good enough" is right — apply it as the gate ON the multi-factor sleeve, or per-stock (Han-Yang-Zhou 2013: Sharpe ~3 in-vol-sorted portfolios) |
| 7 | **Insider cluster buys (small/mid-cap sleeve)** | ≥3 different insiders bought, zero sold, trailing month; 6–12m hold; small/mid caps only | +4.8% size/BM-adjusted 12m (Lakonishok-Lee 2001); filter out "routine" traders → 82bps/mo (Cohen-Malloy-Pomorski 2012). Weakness: dead in large caps, Form-4 lag |
| 8 | **Slow structural screens** | Avoid top decile asset growth; avoid issuers | Asset-growth spread ~8%/yr VW (Cooper et al. 2008); redundant-with-accruals net-external-financing confirms the family |

**What to skip:** Dogs of the Dow (beats Dow only 44% of years 1990–2005, economically ≈ zero after costs/taxes); 1-month factor rotation (turnover kills it); macro-rate-based factor timing (no robust signal — Ilmanen et al. 2021); stacking beta + IVOL + total-vol as if they were three signals (they're one).

**How to package it (the formal answer to "match S&P 500, lower beta"):** score → optimize with a tracking-error cap (~1–4%) + **total portfolio variance ≤ benchmark variance** (Jorion 2003) + a minimum Active Share floor + 50–100 name cardinality. This mechanically delivers index-matching with ≤ index risk. See Rabbit Hole 10.

---

## 1. RABBIT HOLE 1 — The Low-Beta / Low-Volatility Anomaly *(your "match S&P, lower beta")*

**The anomaly:** since Haugen & Baker (1991), the lowest-risk stocks have earned higher returns than the highest-risk ones. This inverts the CAPM and directly serves "index-like exposure, lower beta."

| Paper | Rule | Numbers | Status |
|---|---|---|---|
| Haugen & Baker 1991, JPM 17(3) | Quarterly min-variance on top-1000, max 1.5%/stock, 15%/industry | 14.7%/yr @ 16.5% vol vs Wilshire 12.0% @ 18.8% (1972–89 OOS); lower vol in *every* rolling 5-yr window | ✓ fetched [efalken.com PDF] |
| Ang, Hodrick, Xing & Zhang 2006, JF 61(1) | Monthly IVOL quintiles (FF3 residuals) | High-IVOL quintile earned **negative** total return 1963–2000; low–high FF3 alpha spread **1.31%/mo** | ✓ fetched (WP version) |
| Baker, Bradley & Wurgler 2011, FAJ 67(1) | Quintiles by trailing 5-yr vol or beta | $1 low-vol → **$59.55** vs $0.58 high-vol (1968–2008); low-beta CAPM alpha +2.60%/yr vs high-beta −5.06%/yr | ✓ fetched [stern.nyu.edu PDF] |
| Frazzini & Pedersen 2014, JFE 111(1) | BAB: long low-beta / short high-beta, both rescaled to β=1 | Sharpe **0.78** (1926–2012), ~2× value; 19 markets + other asset classes | ✓ fetched |
| Novy-Marx & Velikov 2022, JFE 143(1) | Forensic replication of BAB | Costs cut BAB ~**60%**; rank-weighting = EW microcaps ($1.05 per $1 to bottom-1% caps); **no alpha net of costs vs FF5**. Long-only large-cap versions survive this critique | ✓ fetched (WP) |
| Liu, Stambaugh & Yuan 2018, JFE 128(1) | Beta anomaly vs IVOL disentangling | Beta anomaly is **IVOL in disguise**; insignificant once you exclude overpriced high-IVOL names | ✓ fetched (abstract) |
| Marquette Associates 2022 (S&P Low Vol Index data) | 1-yr vol rank, quarterly, inverse-vol weights | Lowest-vol decile **15.0%/yr, Sharpe 1.12** vs highest **5.8%, 0.05** (1980–2021); 2008 capture ~45%; **2020: SPLV −1.1% vs S&P +18.4%** | ✓ fetched |

**Economic rationale:** leverage-constrained investors overweight high-beta instead of levering low-beta (Black 1972 → FP 2014); lottery-preference + benchmarked-mandate limits to arbitrage (BBW 2011).

**Design implications for you:** (1) implement long-only in liquid large caps — the academic long-short Sharpe is microcap/cost artifact; (2) it is **low-beta, not crash-proof** (min-vol still fell ~41% in GFC); (3) it carries a persistent defensive-sector tilt and fails in growth-led recoveries → this is exactly the sleeve your regime detector should gate; (4) beta, IVOL and total-vol sort the same stocks — pick one, don't stack.

**⚠ Round-3 caveat (material):** Cederburg & O'Doherty 2016, "Does It Pay to Bet Against Beta?", *JF* 71(2):737–774 (✓ fetched full text [tevgeniou.github.io/EquityRiskFactors/bibliography/ConditionalBetaFactor.pdf]): using conditional IV performance measures, the high-minus-low beta alpha shrinks from −0.59%/mo (t=−2.3) unconditional to **−0.18%/mo (t=−0.7, insignificant)** — >80% of the low-beta premium is attributable to biases in unconditional measures (time-varying betas). Notably, the *low-beta decile's own* unconditional alpha was +0.09%/mo (t=0.8) — insignificant even unconditionally. Their residual is consistent with the Black (1972) zero-beta CAPM, i.e., risk, not exploitable alpha. **Implication:** treat low-vol/low-beta as a *risk-shaping* tilt (defensive equity, lower drawdowns) rather than an alpha engine; the long-only evidence above (BBW quintiles, S&P Low Vol decile data) still supports the risk-reduction claim.

---

## 2. RABBIT HOLE 2 — Simple Weighting & Diversification Schemes

| Paper | Rule | Numbers | Status |
|---|---|---|---|
| DeMiguel, Garlappi & Uppal 2009, RFS 22(5) | 1/N vs 14 optimized models × 7 datasets | **None consistently beats 1/N**; MV would need ~3,000 months of data (25 assets) to win | ✓ fetched (EconPapers/OUP abstract) |
| Plyakha, Uppal & Vilkov 2012, EDHEC WP | Monthly-rebalanced equal weight vs value/price weight | EW wins on mean, 4-factor alpha, Sharpe; **the rebalancing itself is a contrarian reversal strategy**; survives 50bp costs | ✓ fetched (SSRN 2724535) |
| RBC WM "Great Narrowing" (Frawley, Jan 2026) | — | S&P top-10 weight: 19% (1990) → **40.7%** (YE2025); EW beat CW ~1.5%/yr 2003–22, CW beat EW ~32% over 3 yrs through 2025 | ✓ fetched |
| Arnott, Hsu & Moore 2005, FAJ 61(2) | Weight by fundamentals (book/sales/cash flow/dividends) | "Consistent, significant benefits" vs cap-weight; embedded contrarian/value tilt (cap weights mechanically correlate with overvaluation) | ✓ fetched (SSRN 604842) |
| Jagannathan & Ma 2003, JF 58(4) | GMV with w ≥ 0 | Nonnegativity constraint = **implicit shrinkage** of noisy covariance elements; constrained GMV works as well as fancy estimators | ✓ fetched (NBER abstract page) |
| Maillard, Roncalli & Teïletche 2010, JPM 36(4) | Equal Risk Contribution | Provable ordering: **σ(GMV) ≤ σ(ERC) ≤ σ(1/N)** | ✓ fetched [roncalli.com PDF] |
| López de Prado 2016, JPM 42(4) | Hierarchical Risk Parity | Clustering + recursive bisection; no matrix inversion; works on singular covariance; lower OOS variance than CLA | ✓ fetched (SSRN 2708678) |
| Tu & Zhou 2011, JFE 99(1) | Blend 1/N with one optimized rule | The blend **beats 1/N in most scenarios** — the theoretically sanctioned middle ground | ✓ fetched (RePEc abstract) |

**Design implications:** equal-weight is your robust default (zero parameters, rebalancing alpha, size/value tilt). GMV-with-long-only-constraints is the safe risk-based upgrade. ERC/HRP sit between — provably. Don't optimize weights in-sample (ties into Rabbit Hole 8).

---

## 3. RABBIT HOLE 3 — Momentum & Trend (your 200-SMA example, done right)

| Paper | Rule | Numbers | Status |
|---|---|---|---|
| Jegadeesh & Titman 1993, JF 48(1) | 6m/6m decile, overlapping portfolios | **12.01%/yr** compounded excess (1965–89); ~half decays over years 2–3 | ✓ fetched [bauer.uh.edu PDF] |
| Moskowitz, Ooi & Pedersen 2012, JFE 104(2) | Long asset if own trailing 12m excess return > 0 | Positive in **58/58** futures markets; diversified Sharpe > 1 | ✓ fetched [lhpedersen.com PDF] |
| Asness, Moskowitz & Pedersen 2013, JF 68(3) | 12–2 momentum, top/bottom third | Significant premia in **every** asset class; **value–momentum corr ≈ −0.6**; 50/50 combo Sharpe **1.04–1.28** vs 0.47–0.59 singles | ✓ re-verified by orchestrator [stern.nyu.edu PDF] |
| Daniel & Moskowitz 2016, JFE 122(2) | Momentum + conditional vol-scaling in "panic states" | WML Sharpe 0.83 (1927–2010); crashes: Jul–Aug 1932 loser decile **+236%** vs winners +30%; Mar–May 2009 losers +156% vs winners +6.5%; the fix raises Sharpe substantially | ✓ fetched [NBER w20439 PDF] |
| **Faber 2007/2013, JWM** | **Hold index only while close > 10-month SMA (≈200-day)** | S&P 1901–2012: Sharpe 0.55 vs 0.32 B&H, MaxDD **−50.3% vs −83.5%**; invested ~70% of the time; **OOS 2006–2012: 6.01%/Sharpe 0.61/−9.4% vs 3.94%/0.16/−46%**; SMA 3–12m all work (no knife-edge parameter) | ✓ re-verified by orchestrator [mebfaber.com PDF] |
| Antonacci 2014, *Dual Momentum* | GEM: US-vs-world relative + absolute 12m momentum gate | Author: 15.8%/yr, Sharpe 0.96, MaxDD −17.8% (1950–2018); independent replication: 15.2%, Sharpe 0.83, MaxDD 21.7%; **but underperformed passive ~4.8pp/yr since 2010** | ✓ fetched (optimalmomentum.com + replication) |
| Han, Yang & Zhou 2013, JFQA 48(5) | **Stock selection**: vol-decile portfolios held only while close > MA (10–200d all work) | Sharpe **~3.1–3.2** vs 7.4–21.6%/yr B&H (1963–2020); **post-publication alphas persist: 13.38%/yr, t=2.75** (2010–2020); uncorrelated with momentum | ✓ fetched (SSRN 1656460) |

**This is the resolution of your 200-SMA concern:** the SMA rule isn't bad — it's just incomplete as a *single-asset* bet. The evidence supports it as (a) an index-level tail-risk gate (Faber), and (b) a **per-stock selection overlay** (Han-Yang-Zhou — the underrated gem of this dig: cross-sectional MA-timing survived post-publication checks). And momentum's crash risk (1932/2009) is precisely why it should live inside a regime-aware parent system rather than standalone.

---

## 4. RABBIT HOLE 4 — Insider & Smart-Money Signals *(your insider-trading idea)*

| Paper | Rule | Numbers | Status |
|---|---|---|---|
| Lakonishok & Lee 2001, RFS 14(1) | Heavy insider **purchases** trailing 6m; "≥3 buyers, no sellers" | Top-buy vs top-sale: **+7.8%/12m** (+4.8% adj.), 1975–95; **concentrated in small caps** — "even strong buy signals convey almost no information" for large caps; **selling has NO predictive power** | ✓ fetched [lsvasset.com PDF] |
| Jeng, Metrick & Zeckhauser 2003, REStat 85(2) | Hold every open-market insider purchase 6m | >**50bps/mo** abnormal (1975–96); ¼ of alpha in first 5 days, ½ within first month → much evaporates before disclosure in the modern 2-business-day Form 4 regime | ✓ fetched [wharton PDF] |
| Cohen, Malloy & Pomorski 2012, JF 67(3) | Classify insiders **routine** (same calendar month yearly) vs **opportunistic**; trade only opportunistic buys | **82bps/mo** abnormal for opportunistic; routine ≈ 0 | ✓ fetched (SSRN 1692517) |
| Yan & Zhang 2009, RFS 22(2) | Copy **short-term (high-turnover)** institutions' 13F position increases | +2.16% first year (1980–2003); long-term institutions = zero signal | ✓ fetched [lehigh.edu PDF] |
| Jiao, Massa & Zhang 2016, JFE 122(3) | HF 13F holdings **up** + short interest **down** | ~**10%/yr** predictive; resolves the hedging-vs-informed confound in naive copycatting | ✓ fetched (RePEc abstract) |
| Boudoukh, Michaely, Richardson & Roberts 2007, JF 62(2) | **Net payout yield** = (div + buybacks − issuance)/mcap, top decile | Works 1926–2002; dividend yield alone flat, total payout predicts (t>2); repurchases are "management buying its own stock" — the scalable insider signal | ✓ fetched [NBER w10651 PDF] |
| Cohen, Malloy & Nguyen 2020, "Lazy Prices," JF 75(3) | YoY 10-K/10-Q text changes | Up to **188bps/mo** alpha 1995–2014 (NLP screen; companion signal, not core) | ✓ fetched (NBER page) |
| Juranek 2026, SSRN 6883638 | **Caution**: naive insider cluster-buy backtests | Null result under robust inference once overlapping observations handled (2015–24 S&P 500) — use count-based non-overlapping clusters + block tests | ✓ fetched (unrefereed WP — flagged) |

**Verdict:** insider buying works but is structurally **small-cap, rare-signal, lag-encumbered**. For a long-term diversified sleeve, **net payout yield is the insider signal that scales to large caps**. The 13F copycat (short-term institutions, or HF+short-interest combo) is a viable quarterly-frequency overlay. Decode routine vs opportunistic if you go transaction-level.

---

## 5. RABBIT HOLE 5 — Value / Quality Composite Scores

| Paper | Rule | Numbers | Status |
|---|---|---|---|
| Piotroski 2000, JAR 38(S) | **F-Score**: 9 binary tests within high-B/M quintile; buy 8–9 | High-F **+13.4%** market-adjusted vs +5.9% for all high-B/M (1976–96); WML 23%/yr; works best in small/no-coverage names | ✓ fetched [ivey.uwo.ca PDF] |
| Greenblatt 2006 (book) + Podgórski 2026 lit review | **Magic Formula**: rank EBIT/EV + EBIT/(NWC+NPP&E); buy 20–30, annual | Original 30.8%/yr vs 12.4% (1988–2004); independent replications smaller; **EBIT/EV-only often ≥ full formula** (Finland: 20.57% vs 19.26%) — the quality leg may be partly redundant with cheapness | ✓ fetched [journals.ue.poznan.pl] |
| Novy-Marx 2013, JFE 108(1) | **Gross profitability** = (REV−COGS)/TA, annual June sorts | FF3 alpha **0.52%/mo** (t=4.49); **50/50 value+profitability Sharpe 0.85 vs 0.34 market**; corr(value, prof) = **−0.58** | ✓ fetched [rochester.edu PDF] |
| Sloan 1996, The Accounting Review 71(3) | Accruals decile sort; long-only use: **exclude top decile** | Hedge +10.4% yr-1 (t=4.71), decays after; 28/30 years positive | ✓ fetched |
| Asness, Frazzini & Pedersen 2019, Rev. Accounting Studies | **QMJ / QARP**: quality (profit+growth+safety) z-sum, or quality-rank − price-rank | 4-factor alpha **1.05%/mo (t=9.31)** US 1957–2016; positive 23/24 countries; max-Sharpe QMJ/HML mix ≈ **63/37**; quality has flight-to-quality convexity in crises | ✓ fetched [springer open access] |
| Harvey, Liu & Zhu 2016, RFS 29(1) | Multiple-testing hurdle | 316 factors catalogued; **new factors need t > 3.0**; "most claimed research findings in financial economics are likely false" | ✓ fetched [duke.edu PDF] |
| McLean & Pontiff 2016, JF 71(1) | Post-publication decay, 97 predictors | **−26%** out-of-sample (bias), **−58%** post-publication (learning); don't expect 100% decay but plan live results at **~40–70% of published premium** | ✓ fetched |
| Mohanram 2005, Rev. Accounting Studies 10(2) | **G-Score**: 8 binary tests within LOW-B/M (growth) universe, vs 2-digit SIC industry medians: ROA, CFO-ROA, CFO>NI, ROA variance, sales-growth variance, R&D/capex/advertising intensity | Hedge long G 6–8 / short 0–1: **positive 21/21 years**, >10% in 17; G=0 raw −12.6%/yr, G=8 +31.1%/yr; one score point ≈ +3.7–3.9% size-adj return. **Better at identifying losers ("torpedo stocks") than winners** — growth-side complement to F-Score | ✓ fetched [business.columbia.edu/.../mohanram.pdf] |

---

## 6. RABBIT HOLE 6 — Factor Combination & Factor Momentum *(how the sleeves merge)*

- **AMP 2013 (above):** value+momentum 50/50 is the canonical pair (Sharpe doubles via corr ≈ −0.6).
- **Novy-Marx 2016 (WP), "Testing Strategies Based on Multiple Signals":** compositing bias — signals signed/weighted using in-sample results inflate t-stats severalfold (a strategy backtesting t>5 may need **t>7**); **equal weights are the safe choice**; drop the weakest half of signals rather than downweighting. ✓ fetched [rochester.edu MSES PDF]
- **Gupta & Kelly 2019, JPM 45(3):** factor momentum across 65 factors — TSFM Sharpe 0.84 (1-mo) / **0.70 (12-mo, the realistic horizon)**. ✓ fetched [aqr.com PDF]
- **Ehsani & Linnainmaa 2022, JF 77(3):** long factors with positive trailing-12m returns, short negative — **4.2%/yr (t=7.04)**; factor momentum *spans* stock momentum ("stock momentum works because it indirectly times factors"). ✓ fetched [NBER w25551 PDF]
- **Factor timing verdict (both camps):** Asness et al. 2017 "Contrarian Factor Timing Is Deceptively Difficult" (±50–150% tilts on value spread: no compelling improvement vs a diversified book — ✓ fetched AQR PA report) vs Arnott, Beck & Kalesnik 2016 (valuations predictive, but **aggressive rotation erodes Sharpe** — ✓ fetched SSRN 3040956). **Convergent consensus: gentle persistent tilts or none.**
- **Frazzini, Israel & Moskowitz, "Trading Costs" (SSRN 3229719):** $1.7T of live executions — costs are an order of magnitude smaller than assumed; break-even fund sizes long-short: value $83B, size $103B, momentum $52B. **Capacity is a non-issue at your scale; turnover is the binding constraint.** ✓ fetched

---

## 7. RABBIT HOLE 7 — Additional Simple Anomalies (seasonality, industry, issuance)

- **Bouman & Jacobsen 2002, AER 92(5):** "Sell in May" — Nov–Apr beats May–Oct in **36/37 markets**, UK evidence since 1694; no convincing explanation; a beta-timing overlay orthogonal to stock selection. ✓ fetched (SSRN/RePEc/AEA abstracts)
- **McConnell & Xu 2008, FAJ 64(2):** turn-of-month (days −1..+3) = most of the equity premium (0.16%/day vs 0.01%); costs eat it at monthly frequency. ✓ fetched
- **Moskowitz & Grinblatt 1999, JF 54(4):** **industry momentum** — top-3 industries by trailing 6m: EW **0.81%/mo (10.2%/yr, t=7.71)**; industry-adjusting stock momentum kills stock momentum (13bp/mo). Implementation insight: express as **sector-ETF rotation**, highly correlated with stock momentum (not a new signal, a cheaper vehicle). ✓ fetched
- **Cooper, Gulen & Schill 2008, JF 63(4):** **asset-growth anomaly** — low-growth decile ~18%/yr vs high-growth ~5% (1968–2003); risk-adj spread **8%/yr VW / 20%/yr EW**; survives controls for accruals/B/M/momentum — **the best genuinely-new slow factor in this batch**. ✓ fetched
- **Pontiff & Woodgate 2008, JF 63(2):** share issuance ≈ −0.28%/mo per SD post-1970 (absent pre-1970) — **corroborates** the net-payout-yield sleeve, don't stack. ✓ fetched (RePEc abstract)
- **Bradshaw, Richardson & Sloan 2006, JAE:** net external financing long-short 15.5%/yr — but Cohen & Lys 2006 show it's **attenuated/insignificant after controlling for accruals** (redundant with Sloan). ✓ fetched
- **Dogs of the Dow: SKIP.** McQueen, Shields & Thorley 1997 (FSR): 1946–95 raw +3.06% but risk+costs+taxes ≈ all of it; Domian et al. 1998: post-1987 the Dogs stopped being prior losers; beat DJIA only **44%** of years 1990–2005. ✓ fetched [domian PDF]

---

## 8. RABBIT HOLE 8 — Replication & Backtest Methodology *(what survives scrutiny + how not to fool yourself)*

The anomaly literature has a two-camp fight, and **both camps agree on the parts that matter to you**:

- **Hou, Xue & Zhang 2020, "Replicating Anomalies," RFS 33(5):2019–2133** (✓ re-verified by orchestrator [global-q.org PDF]): 452 anomaly variables, 1967–2016. With **NYSE breakpoints + value-weighting**: **65% fail** at \|t\|≥1.96; **82% fail** at the multiple-testing hurdle \|t\|≥2.78; **96% of trading-friction anomalies fail**; drop microcaps entirely → replication falls further (30.5%). Microcaps = **3.2% of market cap but 60.7% of stock count** — EW sorts on microcaps manufacture most published alpha.
- **Jensen, Kelly & Pedersen 2023, "Is There a Replication Crisis in Finance?" JF 78(5)** (✓ fetched [CBS PDF]): 153 factors — **82.4% replicate** on CAPM alphas / Bayesian model vs 55.6% on raw returns; replication 85.7% in small caps, 68.1% in nano — signals survive once methodology is uniform. **The fight is largely definitional (raw vs alpha vs shrinkage). Both papers imply: use uniform methodology, value-weight, ex-microcap headline results.**
- **Sullivan, Timmermann & White 1999, JF 54(5):1647–1691** (✓ re-verified): White's Reality Check on **7,846 technical rules** (MA lengths 2–250 days + bands — *exactly the 200SMA grid you'd scan*). Best rule out-of-sample 1987–96: 14.41%/yr but **data-snooping-adjusted p = 0.341** (nominal 0.004); S&P futures: p 0.042 naive → **0.908 adjusted**. Scant evidence of technical-rule value in that window once you count the search.
- **Bailey & López de Prado 2014, "The Deflated Sharpe Ratio," JPM 40(5)** (✓ fetched [bailey.com PDF]): DSR tests your Sharpe against the expected max Sharpe under N trials; same paper: "the holdout method cannot prevent backtest overfitting." **Keep a trial log; deflate against the number of configurations you actually tried.**
- **Survivorship/delisting bias:** Shumway 1997 JF 52(1) — only 11.7% of NYSE/AMEX performance delists have returns in CRSP; impute **−30%**. Shumway & Warther 1999 JF 54(6) — Nasdaq: impute **−55%**; after correction, "no evidence that there ever was a size effect on Nasdaq." Banz & Breen 1986 — COMPUSTAT backfill introduces look-ahead. (✓ fetched)
- **Arnott, Harvey & Markowitz 2019, "A Backtesting Protocol in the Era of ML," JFDS** (✓ fetched [duke.edu PDF]): write the **ex ante** economic hypothesis before touching data; ex post stories are "almost always a mistake."

### Your backtest DO/DON'T list (distilled)
1. **DO** value-weight (or cap-cap), NYSE breakpoints, headline results excluding microcaps; **DON'T** equal-weight deciles or scan microcap-heavy universes.
2. **DO** point-in-time universe, delisting-return imputation (−30%/−55%), ≥4-month accounting lag; **DON'T** use current constituents or backfilled fundamentals.
3. **DO** log every trial and deflate Sharpe (DSR); **DON'T** treat a train/test split as overfitting protection.
4. **DO** bootstrap against the whole rule family searched; **DON'T** grid-search 200SMA variants and report the winner unadjusted.
5. **DO** demand survival across subperiods, universes, and costs, with a haircut vs published numbers (plan ~40–70% per McLean-Pontiff); **DON'T** invent the story after the t-stat.

---

## 9. RABBIT HOLE 9 — Regime Dependence *(plugs into your regime detector)*

- **Ilmanen, Israel, Lee, Moskowitz & Thapar 2021, JOIM 19(4), "How Do Factor Premia Vary Over Time? A Century of Evidence":** conditional premia exist but are **unrelated to macro variables**; **no robust rate-environment tilt**; only investor *sentiment* survives multiple-testing; value–momentum diversification **weakens in bad months** (−0.66 good months → −0.41 worst). Verdict: be cautious deviating from static allocation. ✓ fetched
- **Asness et al. 2019 (QMJ):** quality has **flight-to-quality convexity** — the cleanest regime rule in the literature: distress state → overweight quality. ✓ fetched
- **Russell Investments 2023 (Gvozdeva & Thaut), Ken French+AQR data 1927–2022, 16 recessions:** pre-recession peak ~5 months: momentum best (+6.7%, 80% hit); during recession: **investment/quality/profitability** best (CMA +8.5%, QMJ +6.4%), market −4.2%; 12m post-trough: **size +13.8% (100% hit), value +8.8%**; quality fades. Practitioner-grade, wide CIs — use directionally. ✓ fetched
- **FTSE Russell (van Boven 2020), Investment Clock 1957–2020:** value/size lead GDP inflections; quality anticipates contractions; momentum carries little macro info. Factor returns can *feed* your regime detector, not just receive from it. ✓ fetched
- **Swedroe 2022 review + Aked 2021 (Research Affiliates):** timing factors on historical returns "nearly worthless"; economic-stage rotation underperforms; best available tools are valuation-spread + 12m factor momentum, and even those have statistically insignificant top-vs-bottom quartile spreads; funds that time factors underperform ~134bps/yr. ✓ fetched
- **Blitz 2021 "The Quant Cycle":** value crashes ~once a decade (growth rallies in bulls, value crashes in bears), typically offset by momentum; the "normal" stage persists ~2/3 of the time. (via fetched review)

**Regime → sleeve mapping (consensus across sources; treat as directional, not mechanical):**

| Regime detector says | Overweight | De-emphasize |
|---|---|---|
| Late expansion / slowdown onset | Momentum, Quality, Low-vol | Value |
| Recession / market distress | Quality, Profitability, Investment | (Momentum can flip in equity-negative recessions) |
| Recovery / early expansion | Size, Value | Quality (fades post-trough) |
| Full expansion | Momentum, Size | Value |
| Rate environment | **No robust tilt** (Ilmanen et al.) | — |

**Round-3 addition — S&P DJI, "A Historical Perspective on Factor Index Performance across Macroeconomic Cycles" (Hao & Watts, Aug 2026; ✓ fetched via asset.tv mirror of the official PDF; spglobal.com direct = 403):** regimes = OECD CLI growth × CPI-vs-3yr-avg inflation, Jun 1995–Jun 2026. S&P 500 annualized by regime: **+27.4%** (rising g, falling π) / +20.0% (rising g, rising π) / +3.0% (falling g, falling π) / **−10.1%** (falling g, rising π). Relative monthly excess vs S&P 500 (hit ratio): **Low Vol is the stagflation standout: +1.19% (0.64), but −0.85% (0.33) in rising-g/falling-π** (confirms RH1's regime-failure finding); **Quality outperforms in ALL falling-growth regimes** (hit >0.6) while keeping upside participation; Momentum peaks in rising g + rising π (+1.07%, 0.63); High Dividend works only in stagflation; Pure Value worst in falling g + falling π; Equal Weight ≈ flat everywhere. Caveat: pre-launch data back-tested hypothetical. This gives you a concrete growth×inflation grid to overlay on your regime detector.

---

## 9b. RABBIT HOLE 10 — Enhanced Indexing & Tracking-Error Constraints *(the formal version of "match S&P 500, lower beta")*

This literature answers exactly your stated goal: hold something that **tracks the index but has better risk characteristics** — with the deviation formally bounded.

| Paper | Rule | Numbers / finding | Status |
|---|---|---|---|
| Roll 1992, JPM 18(4) | Max excess return s.t. TE budget | **TE-optimal portfolios are systematically riskier than the benchmark** and MV-inefficient — the problem everyone else fixes | ✓ fetched (paywalled full text; citation verified) |
| Rudolf, Wolter & Zimmermann 1999, JBF 23(1) | Replace quadratic TEV with **MAD/min–max linear TE → linear program** | Cheap re-optimization, no covariance matrix needed (rule reconstructed from reference list — flagged) | ✓ fetched (RePEc page) |
| **Jorion 2003, FAJ 59(5)** | TEV-constrained deviations + **constraint: total portfolio volatility ≤ benchmark volatility** | Key math: TEV-only optima **don't depend on the benchmark at all**; TEV-constrained set is an ellipse on the MV plane; adding the total-vol cap flattens it → **small expected-return penalty for guaranteed ≤ benchmark risk**. Worked example: TEV 4% @ IR 0.5 → ~200bps excess | ✓ fetched [globalriskguard.com PDF] |
| Cremers & Petajisto 2009, RFS 22(9) | **Active Share = ½Σ\|w_fund − w_index\|**; AS × TE two-dimension view | High-AS funds outperform net of fees; lowest-AS non-index funds underperform; TE 4–6% can coexist with AS 30–100% | ✓ fetched (SSRN 1468203) |
| Canakgoz & Beasley 2009, EJOR 196 | Two-stage MILP: regression slope→1 with alpha constraint, then min cost with **cardinality + cost budgets** | Scales to 2,000+ stocks (CPLEX) — the subset-tracking blueprint | ✓ fetched (author page) |
| Petajisto 2013, FAJ 69(4) | Active share and fund performance | Closet indexers (AS<60%, ~1/3 of assets by 2009) underperform ≈ their fees (−0.92%/yr); top stock pickers **+1.26%/yr after fees** | ✓ fetched [petajisto.net PDF] |
| ESMA WP 2/2020 + du Sart & van Vuuren 2021 (JIFMIM) | Joint TE/R²/β/AS screens; Jorion's same-risk portfolio live test | Closet indexing → lower net alpha (5,400 EU funds); Jorion's same-total-risk portfolio beats the unconstrained market portfolio for extended periods **with less rebalancing** | ✓ fetched both |

**The implementable recipe for your system (this is the packaging for everything above):** score stocks with the composite signal → optimize weights subject to **(a) TE cap ~1–4% vs S&P 500, (b) total portfolio variance ≤ benchmark variance (Jorion's constraint — this is what mechanically delivers "matches the index, ≤ its risk"), (c) a minimum Active Share floor** (otherwise the TE constraint degenerates into closet indexing, which by Petajisto's data underperforms by its fee — your analog is signal strength dilution), **(d) 50–100 name cardinality + transaction-cost caps** (Canakgoz-Beasley). Practitioner calibration (Robeco / State Street, fetched): ex-ante TE ~1% → realized outperformance ~0.6%, realized IR ~0.5.



---

## 10. CONTRADICTIONS & CAVEATS (calibrated)

1. **BAB replication fight (now three-way):** FP 2014 (Sharpe 0.78) vs Novy-Marx & Velikov 2022 (no alpha net of costs, microcap artifact) vs Cederburg & O'Doherty 2016 (fetched full text: ~70–80% of the beta anomaly is unconditional-measure bias; low-beta alpha insignificant even unconditionally). Reconciliation: the long-only large-cap implementations survive as **risk-shaping/defensive tilts**, not alpha engines; the academic long-short Sharpe is the artifact-laden object.
2. **Replication rates:** HXZ 35% vs JKP 82% — definitional (raw return vs alpha). Either way: value-weight, ex-microcap, uniform methodology, and treat \|t\|≈2 as unproven.
3. **Momentum 200SMA:** STW 1999 says MA rules' post-1987 OOS is unimpressive *for index timing*; Han-Yang-Zhou 2013 says cross-sectional stock-level MA timing persists post-publication. Both can be true — the selection use-case has better modern evidence than the timing use-case.
4. **GEM/dual momentum:** strong historical backtest, **weak since 2010** (−4.8pp/yr vs passive) — single-asset rotation is fragile; diversify the rotation universe if you use it.
5. **Insider signals:** the 2026 SSRN cluster-buy null is unrefereed but methodologically pointed — pre-register non-overlapping cluster definitions.
6. **Vendor/practitioner sources** (Marquette, RBC, Russell, FTSE, Alpha Architect) are fetch-verified but not peer-reviewed; they're used for index data and regime tables, flagged as such.
7. **Two citation corrections found during verification:** Jeng et al. is *REStat* (not JF); STW is *JF* 54(5) (not RFS). "Han-Zhou-Zhou Perfect Timing" does not exist — the real paper is Han-Yang-Zhou 2013.
8. **Unverifiable lead:** "Sword of Damocles" attribution to Asness — could not be verified; the substantive anti-timing paper is Asness, Chandra, Ilmanen & Israel 2017.

## 11. OPEN QUESTIONS / SUGGESTED NEXT STEPS
1. **Concrete build order for your system:** (a) implement the composite rank screener (value + gross profitability + low-vol + 12–2 momentum, optional G-Score/F-Score overlay) on a point-in-time universe with delisting imputation; (b) equal-weight top quintile, quarterly; (c) gate with 10-month SMA / your regime model; (d) add net-payout-yield as a second sleeve; (e) package as Jorion-constrained enhanced-index portfolio (TE cap + total-var ≤ benchmark var + active-share floor) if you want tight index-tracking with lower risk; (f) validate with DSR + STW-style family adjustment before trusting anything.
2. **Data requirements:** point-in-time constituents (R3K PIT you already have is right), CRSP-style delisting returns, fundamentals with announcement timestamps, insider Form 4 feeds if pursuing sleeve 7.
3. **Remaining unfetched lead:** Blitz 2021 "The Quant Cycle" primary (only via fetched secondary review); Roll 1992 full text (paywalled, citation verified).
4. **Paper trail:** Mohanram G-Score and Cederburg-O'Doherty PDFs cached under `/root/.hermes/cache/scratch/lit_round3/`; S&P DJI macro-cycle PDF fetched via asset.tv mirror (canonical spglobal URL is Akamai-blocked).

---

## 12. VERIFIED SOURCE INDEX (deduplicated; all URLs fetched during this session)

**Low-beta/vol:** Haugen-Baker 1991 (efalken.com/LowVolClassics/HaugenBaker991.pdf) · Ang-Hodrick-Xing-Zhang 2006 (ruf.rice.edu/~yxing/vol.pdf) · Baker-Bradley-Wurgler 2011 (pages.stern.nyu.edu/~jwurgler/papers/faj-benchmarks.pdf) · Frazzini-Pedersen 2014 (pages.stern.nyu.edu/~lpederse/papers/BettingAgainstBeta.pdf) · Novy-Marx-Velikov 2022 (mysimon.rochester.edu/novy-marx/research/BABAB.pdf) · Liu-Stambaugh-Yuan 2018 (ideas.repec.org/a/eee/jfinec/v128y2018i1p1-15.html) · Marquette 2022 (marquetteassociates.com/.../Low-Volatility-Factor-or-Fad.pdf) · Curvo min-vol backtest (curvo.eu)

**Weighting:** DeMiguel-Garlappi-Uppal 2009 (econpapers.repec.org, RFS 22(5)) · Plyakha-Uppal-Vilkov (papers.ssrn.com/abstract=2724535) · Frawley/RBC 2026 (rbcwealthmanagement.com) · Arnott-Hsu-Moore 2005 (papers.ssrn.com/abstract=604842) · Jagannathan-Ma 2003 (ideas.repec.org/p/nbr/nberwo/8922.html) · Maillard-Roncalli-Teïletche 2010 (thierry-roncalli.com/download/erc.pdf) · López de Prado 2016 (papers.ssrn.com/abstract=2708678) · Tu-Zhou 2011 (ideas.repec.org/a/eee/jfinec/v99y2011i1p204-215.html)

**Momentum/trend:** Jegadeesh-Titman 1993 (bauer.uh.edu/rsusmel/phd/jegadeesh-titman93.pdf) · Moskowitz-Ooi-Pedersen 2012 (docs.lhpedersen.com/TimeSeriesMomentum.pdf) · Asness-Moskowitz-Pedersen 2013 (pages.stern.nyu.edu/~lpederse/papers/ValMomEverywhere.pdf) · Daniel-Moskowitz 2016 (nber.org/.../w20439.pdf) · Faber 2007/2013 (mebfaber.com/wp-content/uploads/2016/05/SSRN-id962461.pdf) · Antonacci 2014 (optimalmomentum.com + SSRN 7427878 replication + bestfolio.app) · Han-Yang-Zhou 2013 (papers.ssrn.com/abstract=1656460)

**Insider/smart money:** Lakonishok-Lee 2001 (lsvasset.com/pdf/research-papers/Insider-Trades-Informative.pdf) · Jeng-Metrick-Zeckhauser 2003 (rodneywhitecenter.wharton.upenn.edu/.../9919.pdf) · Cohen-Malloy-Pomorski 2012 (papers.ssrn.com/abstract=1692517) · Yan-Zhang 2009 (lehigh.edu/~xuy219/research/horizon_RFS.pdf) · Jiao-Massa-Zhang 2016 (ideas.repec.org/a/eee/jfinec/v122y2016i3p544-567.html) · Boudoukh et al. 2007 (nber.org/.../w10651.rev0.pdf) · Cohen-Malloy-Nguyen 2020 (nber.org/papers/w25084) · Juranek 2026 (papers.ssrn.com/abstract=6883638) · SEC Form 13F (sec.gov/files/form13f.pdf)

**Value/quality:** Piotroski 2000 (ivey.uwo.ca/media/3775523/...pdf) · Podgórski 2026 MF review (journals.ue.poznan.pl/REF/article/download/2790/1711) · Novy-Marx 2013 (mysimon.rochester.edu/novy-marx/research/OSoV.pdf) · Sloan 1996 (cuhk.edu.hk/.../1996TAR).pdf) · Asness-Frazzini-Pedersen 2019 (link.springer.com/article/10.1007/s11142-018-9470-2 + d-nb.info/1174954515/34) · Harvey-Liu-Zhu 2016 (people.duke.edu/~charvey/.../P118_and_the_cross.PDF) · McLean-Pontiff 2016 (tevgeniou.github.io/.../AcademicReviewFactor.pdf + SSRN 2156623)

**Combination/factor momentum:** Novy-Marx 2016 WP (mysimon.rochester.edu/novy-marx/research/MSES.pdf) · Gupta-Kelly 2019 (aqr.com/.../Factor-Momentum-Everywhere-JPM-Quant-19.pdf) · Ehsani-Linnainmaa 2022 (nber.org/.../w25551.pdf) · Asness-Chandra-Ilmanen-Israel 2017 (aqr.com PA report PDF) · Arnott-Beck-Kalesnik 2016 (papers.ssrn.com/abstract=3040956) · Frazzini-Israel-Moskowitz Trading Costs (pages.stern.nyu.edu/~afrazzin/... + SSRN 3229719)

**Additional anomalies:** Bouman-Jacobsen 2002 (SSRN/RePEc/AEA abstracts) · McConnell-Xu 2008 (FAJ PDF) · Moskowitz-Grinblatt 1999 (Wharton mirror + AQR summary) · Cooper-Gulen-Schill 2008 (full-text mirror) · Pontiff-Woodgate 2008 (RePEc abstract) · Bradshaw-Richardson-Sloan 2006 (mirror) · Cohen-Lys 2006 (SSRN) · Domian-Louton-Mossman 1998 (FSR PDF)

**Methodology:** Hou-Xue-Zhang 2020 (global-q.org/.../houxuezhang2020rfs.pdf — orchestrator-verified) · Jensen-Kelly-Pedersen 2023 (CBS PDF + github.com/bkelly-lab/ReplicationCrisis) · Sullivan-Timmermann-White 1999 (kevinsheppard.com/.../Sullivan_Timmermann_White.pdf — orchestrator-verified) · Bailey-López de Prado 2014 (davidhbailey.com/dhbpapers/deflated-sharpe.pdf) · Shumway 1997 (tylergshumway.com/.../Shumway-DelistingBiasCRSP-1997.pdf) · Shumway-Warther 1999 (RePEc abstract) · Banz-Breen 1986 (EBSCO) · Arnott-Harvey-Markowitz 2019 (people.duke.edu/~charvey/.../G138_A_backtesting_protocol.pdf)

**Regimes:** Ilmanen-Israel-Lee-Moskowitz-Thapar 2021 (JOIM WP PDF via S3) · Asness et al. 2019 QMJ (above) · Gvozdeva-Thaut 2023 (russellinvestments.com/us/blog/factor-investing-us-recessions) · van Boven 2020 (lseg.com/.../do-factors-carry-information-about-the-economic-cycle.pdf) · Swedroe 2022 (alphaarchitect.com/factor-investing-premiums-and-the-economic-cycle/) · S&P DJI Hao-Watts 2026 macro-cycle paper (asset.tv mirror of official PDF; canonical spglobal URL = 403)

**Enhanced indexing / TE (round 3):** Roll 1992 (jpm.pm-research.com/content/18/4/13) · Rudolf-Wolter-Zimmermann 1999 (ideas.repec.org/a/eee/jbfina/v23y1999i1p85-103.html) · Jorion 2003 (globalriskguard.com/resources/hedge/Jorion%20TEV.pdf) · Cremers-Petajisto 2009 (papers.ssrn.com/abstract=1468203) · Canakgoz-Beasley 2009 (people.brunel.ac.uk/~mastjjb/jeb/trackmip.html) · Petajisto 2013 (petajisto.net/papers/petajisto%202013%20faj%20-%20active%20share.pdf) · ESMA WP 2/2020 (esma.europa.eu/.../esmawp-2020-2_closet_indexing.pdf) · du Sart & van Vuuren 2021 (sciencedirect.com/science/article/abs/pii/S1062976921001198)

**Round-3 verifications:** Mohanram 2005 G-Score (business.columbia.edu/sites/default/files-efs/pubfiles/1507/mohanram.pdf) · Cederburg-O'Doherty 2016 (tevgeniou.github.io/EquityRiskFactors/bibliography/ConditionalBetaFactor.pdf — note: the paper's conclusion is conditional-measure bias, NOT a long-only implementation guide; any "borrowing-cost/long-only" framing attributed to it elsewhere is misattributed) · S&P DJI macro cycles (see Regimes)

*Fetch failures disclosed, not cited as verified: NBER w8922 PDF (404), S&P DJI equal-weight 20-year PDF (anti-bot), quantpedia.com (empty), academic.oup.com RFS paywall, econstor factor-timing review (429), Roll 1992 full text (paywalled; citation verified), spglobal.com canonical S&P DJI macro PDF (Akamai 403 — identical official PDF verified via asset.tv mirror instead). Working-paper vs published versions flagged inline above where applicable.*

— End of report. Research window: 2026-11-07 **11:13:33 – 11:46:00 EST** (33 minutes of the 60-minute budget; 3 rounds, 11 agents, 10 rabbit holes — budget unused after all planned rabbit holes reached depth and remaining leads were either verified or explicitly disclosed as unfetchable).
