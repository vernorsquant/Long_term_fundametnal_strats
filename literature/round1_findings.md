# Round 1 findings (dispatched 11:14:54 EST, completed ~11:24 EST, 2026-11-07)
# 5 agents, one facet each. All URLs below were fetched by the researching agent.

## Facet 1: Low-beta / low-volatility anomaly
- Haugen & Baker 1991, "The Efficient Market Inefficiency of Capitalization-Weighted Stock Portfolios," JPM 17(3):35-40. Min-var optimizer on top-1000, quarterly, 24mo covariance, max 1.5%/stock, 15%/industry: 1972-89 OOS 14.7%/yr @ 16.5% vol vs Wilshire 5000 12.0% @ 18.8%. https://www.efalken.com/LowVolClassics/HaugenBaker991.pdf
- Ang, Hodrick, Xing & Zhang 2006, "The Cross-Section of Volatility and Expected Returns," JF 61(1):259-299. High-IVOL quintile earns NEGATIVE avg return 1963-2000; FF3 alpha spread low-high IVOL = 1.31%/mo. http://www.ruf.rice.edu/~yxing/vol.pdf (WP version)
- Baker, Bradley & Wurgler 2011, "Benchmarks as Limits to Arbitrage," FAJ 67(1):40-54. $1 in low-vol quintile -> $59.55 vs $0.58 high-vol (1968-2008); low-beta alpha +2.60%/yr vs high-beta -5.06%. https://pages.stern.nyu.edu/~jwurgler/papers/faj-benchmarks.pdf
- Frazzini & Pedersen 2014, "Betting Against Beta," JFE 111(1):1-25. BAB Sharpe 0.78 (1926-2012), ~2x value's Sharpe; leverage-constraint rationale. https://pages.stern.nyu.edu/~lpederse/papers/BettingAgainstBeta.pdf
- Novy-Marx & Velikov 2022, "Betting Against Betting Against Beta," JFE 143(1):80-106. Rank-weighting = equal-weighting microcaps ($1.05 per $1 committed to bottom-1% caps); costs cut BAB ~60%; no significant alpha net of costs vs FF5. https://mysimon.rochester.edu/novy-marx/research/BABAB.pdf (WP)
- Liu, Stambaugh & Yuan 2018, "Absolving Beta of Volatility's Effects," JFE 128(1):1-15. Beta anomaly = IVOL in disguise; insignificant controlling IVOL or excluding overpriced stocks. https://ideas.repec.org/a/eee/jfinec/v128y2018i1p1-15.html
- Practical: S&P 500 Low Vol Index (1-yr vol rank, quarterly, inverse-vol weights): low-vol decile 15.0%/yr Sharpe 1.12 vs high-vol 5.8%/yr Sharpe 0.05 (1980-2021); 2008 downside capture ~45%; BUT 2020 SPLV -1.1% vs S&P +18.4% (regime failure in growth-led rebound). Min-vol index still fell ~41% peak-trough in GFC. (Marquette Associates Apr 2022; Curvo backtest)
- Key weaknesses: sector tilts (staples/utilities), lags growth-led recoveries, long-only large-cap implementations survive the BAB critique, beta/IVOL/total-vol largely price the SAME stocks (don't stack).

## Facet 2: Weighting / diversification schemes
- DeMiguel, Garlappi & Uppal 2009, "Optimal Versus Naive Diversification," RFS 22(5):1915-1953. None of 14 optimized models consistently beats 1/N; MV needs ~3000 months for 25 assets. (EconPapers page verified)
- Plyakha, Uppal & Vilkov 2012/2017, "Why Does an Equal-Weighted Portfolio Outperform...?" EDHEC WP (SSRN 2724535). Monthly-rebalanced EW beats VW/PW on alpha+Sharpe; alpha comes from rebalancing = contrarian reversal; survives 50bp costs.
- Frawley (RBC WM, Jan 2026) "The Great Narrowing": top-10 S&P 500 weight ~19% (1990) -> 40.7% (YE2025); EW beat CW ~1.5%/yr 2003-2022; CW beat EW ~32% over 3 yrs through 2025. Regime-dependent.
- Arnott, Hsu & Moore 2005, "Fundamental Indexation," FAJ 61(2):83-99. Weight by book/sales/cash flow/dividends/employment = embedded contrarian/value tilt. (SSRN 604842)
- Jagannathan & Ma 2003, "Risk Reduction in Large Portfolios," JF 58(4):1651-1683. Nonnegativity constraints = implicit shrinkage; GMV+constraints works as well as fancy estimators.
- Maillard, Roncalli & Teiletche 2010, "The Properties of Equally Weighted Risk Contribution Portfolios," JPM 36(4):60-70. Proven: sigma(GMV) <= sigma(ERC) <= sigma(1/N). https://thierry-roncalli.com/download/erc.pdf
- Lopez de Prado 2016, "Building Diversified Portfolios that Outperform Out of Sample," JPM 42(4):59-69 (SSRN 2708678). HRP: hierarchical clustering + recursive bisection; works on singular covariance; lower OOS variance than CLA.
- Tu & Zhou 2011, "Markowitz meets Talmud," JFE 99(1):204-215. Blend 1/N with one sophisticated rule -> beats 1/N in most scenarios.

## Facet 3: Momentum / trend
- Jegadeesh & Titman 1993, JF 48(1):65-91. 6m/6m decile = 12.01%/yr compounded excess (1965-89). https://www.bauer.uh.edu/rsusmel/phd/jegadeesh-titman93.pdf
- Moskowitz, Ooi & Pedersen 2012, "Time Series Momentum," JFE 104(2):228-250. 12m own-return sign rule; 58/58 futures positive; portfolio Sharpe >1. http://docs.lhpedersen.com/TimeSeriesMomentum.pdf
- Asness, Moskowitz & Pedersen 2013, "Value and Momentum Everywhere," JF 68(3):929-985. Momentum significant in every asset class; value-momentum correlation ~ -0.60; 50/50 combo beats either alone (Sharpe ~1.0-1.3). https://pages.stern.nyu.edu/~lpederse/papers/ValMomEverywhere.pdf
- Daniel & Moskowitz 2016, "Momentum Crashes," JFE 122(2):221-247. WML Sharpe 0.83 (1927-2010); crashes: 1932 losers +236% in 2mo vs winners +30%; 2009 losers +156% vs winners +6.5%. Fix: conditionally scale exposure in "panic states" (post-decline + high vol). https://www.nber.org/system/files/working_papers/w20439/w20439.pdf
- Faber 2007/2013, "A Quantitative Approach to Tactical Asset Allocation," JWM. 10-month SMA (~200d) on S&P: Sharpe 0.55 vs 0.32 B&H, MaxDD -50.3% vs -83.5% (1901-2012); OOS 2006-2012 6.01%/Sharpe 0.61/MaxDD -9.4% vs 3.94%/0.16/-46%. SMA 3-12m all work. https://mebfaber.com/wp-content/uploads/2016/05/SSRN-id962461.pdf
- Antonacci 2014, Dual Momentum (GEM): 1950-2018 author backtest 15.8%/yr Sharpe 0.96 MaxDD -17.8%; independent replication 1971-2026: 15.18% CAGR, MaxDD 21.7%, Sharpe 0.83; BUT underperformed passive by ~4.8pp/yr since 2010.
- Han, Yang & Zhou 2013, "A New Anomaly: The Cross-Sectional Profitability of Technical Analysis," JFQA 48(5):1433-1461. Vol-decile portfolios held only while close>MA (robust 10-200d): Sharpe ~3.1-3.2 vs B&H; post-publication 2010-2020 alphas persist (13.38%/yr, t=2.75). https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1656460
- Note: "Han-Zhou-Zhou Perfect Timing" does not exist under that title; correct paper is Han-Yang-Zhou 2013 (+ Han-Zhou-Zhu 2016 trend factor JFE).

## Facet 4: Insider / smart-money signals
- Lakonishok & Lee 2001, "Are Insider Trades Informative?" RFS 14(1):79-111. Top-buy vs top-sale firms: +7.8% next 12mo (+4.8% size/BM-adjusted), 1975-95; concentrated in SMALL caps ("even strong buy signals convey almost no information" for large caps); insider SELLING has no predictive power. https://www.lsvasset.com/pdf/research-papers/Insider-Trades-Informative.pdf
- Jeng, Metrick & Zeckhauser 2003, REStat 85(2):453-471 (not JF). Insider purchase portfolio >50bps/mo abnormal (1975-96); 1/4 of alpha in first 5 days, 1/2 within first month. https://rodneywhitecenter.wharton.upenn.edu/wp-content/uploads/2014/04/9919.pdf
- Cohen, Malloy & Pomorski 2012, "Decoding Inside Information," JF 67(3):1009-1043. Filter out "routine" trades (same calendar month yearly); opportunistic trades only -> 82bps/mo abnormal. https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1692517
- Yan & Zhang 2009, RFS 22(2):893-924. Copy SHORT-TERM (high-turnover) institutions' 13F position increases: +2.16% first year (1980-2003); long-term institutions = zero signal. https://www.lehigh.edu/~xuy219/research/horizon_RFS.pdf
- Jiao, Massa & Zhang 2016, "Short Selling Meets Hedge Fund 13F," JFE 122(3):544-567. Signal = HF holdings UP + short interest DOWN -> ~10%/yr predictive power. https://ideas.repec.org/a/eee/jfinec/v122y2016i3p544-567.html
- Boudoukh, Michaely, Richardson & Roberts 2007, "On the Importance of Measuring Payout Yield," JF 62(2):877-915. Net payout yield (div + net buybacks)/mcap top decile; 1926-2002; total payout yield predicts returns, dividend yield alone flat. https://www.nber.org/system/files/working_papers/w10651/revisions/w10651.rev0.pdf
- Cohen, Malloy & Nguyen 2020, "Lazy Prices," JF 75(3):1371-1415. YoY 10-K/10-Q text changes -> up to 188bps/mo alpha 1995-2014 (NLP signal). https://www.nber.org/papers/w25084
- Caution: Juranek 2026 SSRN 6883638 — naive insider cluster-buy backtests may be inflated by overlapping observations (null result with robust inference, 2015-24 S&P500). Use count-based non-overlapping clusters (>=3 buyers, 0 sellers, trailing month) + block tests.
- Frictions: Form 4 lag (2 business days now; 10th-of-month pre-2002 in old studies), 13F 45-day lag + $100M threshold, small-cap bias, signal rarity.

## Facet 5: Value/quality composites + replication lens
- Piotroski 2000, JAR 38(Supp):1-41. F-Score 9 binary tests within high-B/M quintile; high-F (8-9) +13.4% market-adjusted vs +5.9% full high-BM (1976-96); WML 23%/yr. Works in small/no-coverage names. https://www.ivey.uwo.ca/media/3775523/value_investing_the_use_of_historical_financial_statement_information.pdf
- Greenblatt 2006 Magic Formula (book; verified via Podgorski 2026 lit review, journals.ue.poznan.pl/REF/article/download/2790/1711): ROC=EBIT/(NWC+NPP&E) + EY=EBIT/EV ranks, buy 20-30, annual. Original 30.8%/yr vs 12.4% S&P 1988-2004; independent replications smaller; EY-only often >= full formula (Finland: EY-only 20.57% vs MF 19.26%).
- Novy-Marx 2013, "The Other Side of Value," JFE 108(1):1-28. GP/assets = (REV-COGS)/TA; FF3 alpha 0.52%/mo (t=4.49); 50/50 value+profitability Sharpe 0.85 vs 0.34 market; v-p correlation -0.58. https://mysimon.rochester.edu/novy-marx/research/OSoV.pdf
- Sloan 1996, The Accounting Review 71(3):289-315. Accruals decile hedge +10.4% yr1 (t=4.71), decays after; long-only use: exclude top accrual decile.
- Asness, Frazzini & Pedersen 2019, "Quality Minus Junk," RAS 24(1):3-66. Quality = profitability+growth+safety z-sum; 4-factor alpha 1.05%/mo (t=9.31) US 1957-2016; positive in 23/24 countries; negative market/value/size betas. https://d-nb.info/1174954515/34
- Harvey, Liu & Zhu 2016, RFS 29(1):5-68. 316 factors catalogued; new factors need t>3.0; "most claimed research findings are likely false." https://people.duke.edu/~charvey/Research/Published_Papers/P118_and_the_cross.PDF
- McLean & Pontiff 2016, JF 71(1):5-32. 97 predictors: -26% OOS (bias), -58% post-publication (learning). Plan live expectations at ~40-70% of published premium. https://tevgeniou.github.io/EquityRiskFactors/bibliography/AcademicReviewFactor.pdf
- Not fetched / follow-up: Mohanram 2005 G-Score (growth-side complement).
