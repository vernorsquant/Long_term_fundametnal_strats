# The 3 Best Strategies — Final Answer
**Timed dig: 2026-11-07 13:02:33 → ~13:20 EST (20-min budget, fully used: 3 rounds, 6 agents).**
Built on the verified 65-paper base from the earlier session dig (`REPORT.md` in this directory). Specs + red-team memo: `/root/.hermes/cache/scratch/*spec*.md`, `redteam_round2_verdicts.md`. All round-2 verifications: 4/4 claims fetched and confirmed.

---

## #1 — Multi-Factor Rank Composite  (the core sleeve; BUILD)
**The rule:** rank the top-1000 liquid US stocks (ex-ADR/REIT/fund/SPAC/OTC/<$5/IPO<12m, $5M 63-day dollar-volume floor) on four equal-weighted cross-sectional percentile ranks — value (B/M, 6-mo accounting lag), profitability (GP/TA), low-risk (trailing 12m vol, inverted), momentum (12-2 return). Hold the top decile (~100 names), equal-weight, 3% name cap, monthly refresh with a 20th-percentile exit buffer. Static 25/15/28/27 signal weights (value cut by integration plan to make room for sleeve #2 — see below).
**Evidence:** value+mom corr −0.6 → 50/50 Sharpe 1.04–1.28 (AMP 2013, JF); GP/TA alpha 0.52%/mo t=4.49 (Novy-Marx 2013); low-vol decile 15.0%/yr Sharpe 1.12 (Marquette/S&P data); equal ranks are the anti-overfitting choice (Novy-Marx 2016); top-1000 convention kills the HXZ microcap failure mode (65–82% of anomalies are artifacts).
**Net expectation (decay-haircut ladder):** ~0.5–3.7%/yr active, central **~1–2%/yr**, IR 0.2–0.5. Red-team adverse case (25–30% long-only capture): ~1.0%/yr, IR ≈ 0.20. Budget at ~1–2%.
**Red-team fix applied:** deleted the regime-tilt layer entirely (it landed 2–3 months late and contradicted the parent-system division of labor). Regime gating belongs to YOUR regime detector; this sleeve runs static.
**Why #1:** best evidence-per-complexity; carries four half-decorrelated premia in one book.

## #2 — Net Payout Yield, annual top decile  (the insider-intuition sleeve at scale; BUILD)
**The rule:** NPY = (−`ncfdiv` − `ncfcommon`)/`marketcap` from Sharadar SF1 annual data; universe $500M+ US commons (ex-financials/ADR/IPO<2y); top decile, ≥50 names, EW, 3% cap; rebalance April 1 into the coordinated monthly session, hold 12m; guard `ncfo > 0`.
**Evidence (round-2-verified from the paper's own table):** D10−D1 = **0.73%/mo** (~9.5%/yr, Jul 1984–Dec 2003, monthly units confirmed); D10 firms ~2.5× D1 market cap (capacity-friendly, though D7–D8 are the true mega caps); 1926–2002 predictability; net-of-haircut (~×0.42) **~2–3.5%/yr net**. It is "management buying its own stock at scale" — your insider-trading idea in its large-cap, capacity-viable form. Data: free, three Sharadar fields.
**Red-team verdict:** BUILD NPY-only. The insider-cluster transaction overlay was **cut** (guard-stack intersection ≈ empty live; 0.4–0.6%/yr contribution for full event infrastructure; naive version nets ~0–0.5%).
**Watch:** buys ride value/issuance factor droughts with no exit signal → risk-module scenario below. If any backtest shows >4% net, treat as a bug, not skill.

## #3 — SMA-Gated Momentum Basket  (your 200-SMA idea, conditional BUILD)
**The rule:** rank by 12-1 momentum, top decile (~100 names, EW, monthly); hold each name only while its month-end close > its own 10-month SMA; gated-out → 13-week T-bills. **Fold into sleeve #1 as its momentum leg** (gated 12-1 replaces raw 12-2), 80/20 AUM split with sleeve #2.
**Evidence:** HYZ published JFQA version confirms the strategy is **long-only-with-T-bills** (stock-or-cash, no shorts — Sharpe 2.2–3.8 across vol deciles 1963–2009, alphas >9.98%/yr at 25bp/trade); Faber's index version works (MaxDD −50.3% vs −83.5%) but is redundant with your regime detector; DM crash math says the gate prevents 1932/2009-style momentum blowups.
**Red-team: conditional.** The 13.38%/yr HYZ anchor has a 95% CI of 2.5–24.2% (one decade, bull-dominated) and conflates momentum premium with gate value; real gate-marginal alpha ≈ **1–3%/yr**. The paper's deciles are small-cap-heavy EW CRSP (value-weighted largest-decile alpha only 3–5%/yr).
**Pre-registered go/no-go (decide before backtesting):** IS 1965–2010, holdout 2011+ evaluated ONCE; pass = holdout Sharpe ≥ +0.15 vs the **ungated** basket AND MaxDD ≤ 75% of ungated. Fail → revert leg to raw momentum, no structural change.

---

## Integration (round-3 plan)
- **Books:** Sleeve A = composite w/ gated momentum leg (80% AUM) / Sleeve B = NPY (20% AUM), with spec-1 value leg cut 25→15% so no factor exceeds 40% of active risk (verified shares: value 39.9 / prof 21.3 / low-vol 20.3 / mom 18.5).
- **Calendar:** one coordinated monthly session, 3rd trading day after month-end; NPY folds into April's session. Monthly reset of the 80/20 split is what enforces the factor caps.
- **Risk module — add 3 scenarios:** (1) mapped factor droughts (value 2007–17, mom 2009–11/2016–17, prof 2018–20) through the exposure matrix; (2) B/M+NPY joint crash at ρ=1.0 sized 2× the 1998–2000 worst; (3) V-bottom double-de-risk (gate flips + parent de-risks in a Mar-2009/Apr-2020-style 1-month recovery) — mitigate with a max ⅓-of-book gate transition per month.
- **Combined expectation:** net active alpha **1.2–2.3%/yr, central ~1.75%**, active vol ~3.0–4.1%, **IR ≈ 0.3–0.76, central ~0.5** — doesn't beat the best sleeve's IR (overlap costs ~0.6 factor corr) but ~2× the alpha per unit capacity and kills single-factor-drought dependence. Gate pass adds ~+0.3–0.7%/yr; gate fail costs nothing structurally.
- **Round-2 verification bonus:** low-vol's rebound-lag is now multi-source verified (2009: low-vol index +19.2% vs S&P +26.5%; USMV trailed 25.9pp Apr 2020–May 2021; trailed in 9 bull years) — relevant to your regime detector's growth-led-rebound state. FIM break-even sizes confirmed long-short (value $83B, momentum $52B US draft figures; $214B/$56B in the 2021 revision).

**Honest bottom line:** these three are not a get-rich engine — combined they're a ~1.5–2%/yr net active-alpha machine at IR ~0.5 that survives replication scrutiny, costs little to run, and slots cleanly under your regime/risk layers. The big published numbers (Sharpe 1.0–3.8) are long-short, microcap-flavored, or pre-decay; the ladder above is the realistic version.
