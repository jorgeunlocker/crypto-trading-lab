# Reference projects — study conclusions

This document records the conclusions of studying the repositories under `external/`, as required by `ROADMAP.md` Chapter 25 (Reference projects) and Chapter 49.9 (Research code references).

Notation: `Ch. N` refers to a roadmap chapter, `N.x` to a roadmap section. "Book" refers to *Advances in Financial Machine Learning* (Marcos López de Prado, Wiley, 2018). The book is the conceptual authority in any dispute between implementations; it must be cited in documentation.

Status legend: **done** (study concluded, conclusions below) · **pending** (study not yet concluded).

| Reference | Path | Roadmap pointers | Status |
|---|---|---|---|
| AFML exercises | `external/adv-financial-ml-marcos-exercises` | Ch. 29, 38, 43–50 | **done** (Part A) |
| Backtrader | `external/backtrader` | Ch. 37 | **done** (Part B) |
| CCXT | `external/ccxt` | Ch. 26 | **done** (Part D) |
| Freqtrade | `external/freqtrade` | Ch. 33, 39, 57 | **done** (Part E) |
| Hummingbot | `external/hummingbot` | Ch. 56, 26 | **done** (Part F) |
| Jesse | `external/jesse` | Ch. 43–50 | **done** (Part G) |
| TA-Lib | `external/ta-lib` | Ch. 31 | **done** (Part H) |
| VectorBT | `external/vectorbt` | Ch. 37, 45 | **done** (Part C) |
| AFML book exhibit compilation | local reference copy (not in this repository) | Ch. 43–50, 47.4 | **inventoried** (A.9) |

---

# Part A — AFML(Advances in Financial Machine Learning) exercise repository (`external/adv-financial-ml-marcos-exercises`)

## A.1 The reference at a glance

| Property | Value |
|---|---|
| Upstream | https://github.com/fernandodelacalle/adv-financial-ml-marcos-exercises |
| License | MIT (© 2021 Fernando) — code may be adapted with attribution, after review and testing |
| Content | Author's solutions to the book's exercises + the book's own code snippets (`src/snippets/`) |
| Language stack | Python 3.6-era: pandas 0.23.4, numpy 1.15.0, scikit-learn 0.19.2, scipy 1.1.0, pyarrow 0.9.0 |
| Tests / type hints | none — research-notebook quality throughout |

**Key structural fact:** the *snippet files* cover considerably more of the book than the *notebooks* do. Several techniques (notably purged cross-validation and the whole labeling suite) have complete, verbatim book code in `src/snippets/` while their exercise notebooks are empty stubs. Conversely, some roadmap-relevant techniques are absent from the repository entirely (fractional differentiation, PBO, DSR/PSR, CPCV) and must be implemented from the book.

**Second reference — the book's own exhibits.** The developer also holds the book's exhibit compilation as a local reference copy, outside this repository (see A.9). It contains **all 97 book snippets** — including the ones this repository lacks — plus the book's equations, figures and tables, though it is *not* the full book text. A.9 inventories it; the per-technique specifications that turn its exhibits into implementable requirements live in `docs/en/developers/afml-techniques.md`.

## A.2 Coverage status by book chapter

| Book chapter / topic | Notebook | Snippet code | Coverage |
|---|---|---|---|
| 2. Financial data structures (bars, CUSUM) | `ch2.ipynb` — rich (64 cells) | `src/snippets/ch2.py`, `features/bars.py` | **strong** |
| 3. Labeling (triple-barrier, meta-labeling) | `ch3.ipynb` — partial (20 cells; 3.1 started, 3.2–3.5 headers only) | `src/snippets/ch3.py` — complete suite (3.1–3.8) | **strong (code), weak (worked examples)** |
| 4. Sample weights by uniqueness | `ch4.ipynb` — empty stub (1 cell, mislabeled title) | none | **absent** |
| 5. Fractionally differentiated features | `ch5.ipynb` — empty stub | none | **absent** |
| 6. Ensemble methods | `ch6.ipynb` — partial (9 cells; Snippet 6.1 only) | none | **weak** |
| 7. Cross-validation in finance (purging, embargo) | `ch7.ipynb` — mostly empty (imports only) | `src/snippets/ch7.py` — complete suite (7.1–7.4) | **strong (code), no worked examples** |
| 8. Feature importance (MDI, MDA, SFI, PCA) | `ch8.ipynb` — rich (14 cells, MDI/MDA/SFI runs + plots) | `src/snippets/ch8.py` — complete suite (8.2–8.8) | **strong** |
| 9. Hyper-parameter tuning | `ch9.ipynb` — empty stub | none | **absent** |
| 10. Bet sizing | — | none | **absent** |
| 11–12. Backtest statistics, PBO/CSCV, CPCV | — | none | **absent** |
| 14. Backtest statistics (PSR, DSR) | — | none | **absent** |
| 17–18. Structural breaks, entropy features | — | only CUSUM (Snippet 2.4) | **mostly absent** |
| 20. Multiprocessing | — | `src/snippets/ch20.py` — complete suite (20.5–20.10) | **strong** |
| Data preparation (Kibot tick data, MAD outliers) | `data_load_and_clean.ipynb` (18 cells) | `data/clean_data.py` | **didactic** |

> This table covers the *exercise repository only*. Rows marked **absent** are absent from that repository — not from the project. For each of them the book's own exhibit compilation supplies the snippet or equation (see A.9), and `afml-techniques.md` records the resulting specification.

## A.3 Technique-to-exercise mapping (roadmap → reference)

This is the operative table for implementers. "Adoption" follows the roadmap rule that adapted code must be reviewed against the book's definitions and tested before merging (Ch. 49.9). Status values: **usable reference** = code exists in the exercise repository; **absent in repo, exhibits available** = the book's snippet or equation is available in the local exhibit compilation (A.9) and the technique is specified in `afml-techniques.md`; **absent** = neither code nor formula is available locally.

| Roadmap section | Technique | Where in the reference | Status | Adoption guidance |
|---|---|---|---|---|
| 29.1, 49.1 | Tick / volume / dollar bars | `features/bars.py` (`tick_bar`, `volume_bar`, `dollar_bar`, `volume_bar_cum`, `dollar_bar_cum`); `ch2.ipynb` exercise 2.1(a)–(e): bar counts, serial correlation, monthly variance, Jarque-Bera | usable reference | Reimplement with vectorized cumulative thresholds; the notebook's statistical comparisons (2.1b–e) are a ready-made acceptance-test template |
| 29.1, 49.1 | Dollar *imbalance* bars | `ch2.ipynb` cell "implement dollar imbalance bars" (exercise 2.2) | attempt only, incomplete | Implement from the book's chapter 2; do not treat the notebook cell as a correct reference |
| 49.2 | Triple-barrier labeling | `src/snippets/ch3.py`: `applyPtSlOnT1` (Sn. 3.2), `addVerticalBarrier` (Sn. 3.4), `getEvents` (Sn. 3.3); `ch3.ipynb` 3.1 | usable reference | Verify intrabar-ambiguity handling against Ch. 37.5 (the book's snippet evaluates barrier touches on the close path; document that choice explicitly) |
| 49.2 | Dropping rare labels | `src/snippets/ch3.py`: `dropLabels` (Sn. 3.8) | usable reference | Wire into the labeling pipeline as an explicit, recorded step |
| 49.3 | Sample weights by uniqueness | no repo code; book ch. 4 exhibits: Sn. 4.1–4.11 (the compilation, pp. 35–44), Eq. 32 | **absent in repo**, exhibits available | Specified in `afml-techniques.md` §3.1; port with vectorized interval counting |
| 49.4 | Purging of overlapping training observations | `src/snippets/ch7.py`: `getTrainTimes` (Sn. 7.1) | usable reference | Port with unit tests on interval-overlap edge cases |
| 49.4 | Embargo | `src/snippets/ch7.py`: `getEmbargoTimes` (Sn. 7.2) | usable reference | Same; embargo length must become an explicit parameter (Ch. 49.4) |
| 49.4 | Purged K-Fold CV class | `src/snippets/ch7.py`: `PurgedKFold` (Sn. 7.3), `cvScore` (Sn. 7.4) | usable reference | The central CV component for Ch. 49.4/38.8; verify against the book's definition before merging |
| 45.3 | Combinatorial purged CV (CPCV) | no repo code; book ch. 12 exhibits: Eq. 39–44 (the compilation, pp. 96–99), Figs. 12.1–12.2 | **absent in repo**, equations available | Specified in `afml-techniques.md` §3.4; build on `PurgedKFold`; count paths and disclose data re-use (Ch. 45.3) |
| 49.5 | Probabilistic Sharpe ratio (PSR) | no repo code; book ch. 14 exhibit: Eq. 52 (the compilation, p. 126) | **absent in repo**, formula available | Specified in `afml-techniques.md` §3.5; requires recorded skewness/kurtosis/sample length |
| 49.5 | Deflated Sharpe ratio (DSR) | no repo code; compilation Eq. 53 (p. 127) + primary paper Eqs. 1–2 | **absent in repo**, specified | Specified in `afml-techniques.md` §3.6 from Bailey & López de Prado (2014); requires the trial count (Ch. 49.5: no trial count → no deflated estimate) |
| 49.6 | PBO via CSCV | no repo code; compilation Figs. 11.1–11.2 (p. 94) + primary paper Algorithm 2.3 | **absent in repo**, specified | Specified in `afml-techniques.md` §3.7 from Bailey, Borwein, López de Prado & Zhu (Algorithm 2.3); combine with the optimization trial records of Ch. 39 |
| 48.1, 48.3 | Hyper-parameter tuning with purged CV | no repo code; book ch. 9 exhibits: Sn. 9.1–9.4 (the compilation, pp. 79–82) | **absent in repo**, exhibits available | Specified in `afml-techniques.md` §3.8; requires scikit-learn (Ch. 4.0 STOP procedure) |
| 49.8, 58 | Bet sizing from probabilities (+ limit price) | no repo code; book ch. 10 exhibits: Sn. 10.1–10.4 (the compilation, pp. 87–91) | **absent in repo**, exhibits available | Specified in `afml-techniques.md` §3.9; research-tier only, must pass the risk manager (Ch. 58) |
| 49.7 | MDI feature importance | `src/snippets/ch8.py`: `featImpMDI` (Sn. 8.2); `ch8.ipynb` | usable reference | Tree-ensemble only; run within purged CV, never plain K-Fold |
| 49.7 | MDA feature importance | `src/snippets/ch8.py`: `featImpMDA` (Sn. 8.3); `ch8.ipynb` | usable reference | Score on purged/embargoed splits; report uncertainty |
| 49.7 | Single-feature importance (SFI) | `src/snippets/ch8.py`: `auxFeatImpSFI` (Sn. 8.4); `ch8.ipynb` | usable reference | — |
| 49.7 | Clustered / orthogonalized importance | `src/snippets/ch8.py`: `get_eVec` + `orthoFeats` (Sn. 8.5), `kendal_weighted` (Sn. 8.6) | usable reference | Supports the roadmap rule that correlated feature clusters distort single-feature importance (Ch. 49.7) |
| 49.8 | Meta-labeling events/bins | `src/snippets/ch3.py`: `getEvents` (Sn. 3.6), `getBins` (Sn. 3.7) | usable reference | Meta-label output remains a research signal candidate; never bypasses 33.6 |
| 47.4 | Symmetric CUSUM filter (structural breaks/events) | `src/snippets/ch2.py`: `getTEvents` (Sn. 2.4) | usable reference | Threshold `h` must be a recorded parameter |
| 47.4 | Structural-break tests (SADF/GSADF, CUSUM on residuals) | no repo code; book ch. 17 exhibits: Sn. 17.1–17.4 (the compilation, pp. 164–165), §17.3 (p. 156) | **absent in repo**, exhibits available | Specified in `afml-techniques.md` §3.10; minimum sample length, deterministic terms and lag order are recorded parameters |
| 47.4 | Fractional differentiation | no repo code; book ch. 5 exhibits: Sn. 5.1–5.4 (the compilation, pp. 48–53), Table 5.1 | **absent in repo**, exhibits available | Specified in `afml-techniques.md` §3.2; differentiation order `d` must be recorded (Ch. 47.4) |
| 47.4 | Entropy features | no repo code; book ch. 18 exhibits: Sn. 18.1–18.4 (the compilation, pp. 168–172) | **absent in repo**, exhibits available | Specified in `afml-techniques.md` §3.11; document the quantization scheme |
| 50 | Ensembles (bagging theory) | `ch6.ipynb` (Snippet 6.1 bagging accuracy, exercise 6.1 discussion) | weak reference | Ensemble design follows Ch. 50; this reference only illustrates bagging variance reduction |
| 10 | Multiprocessing framework | `src/snippets/ch20.py`: `mpPandasObj` (Sn. 20.7) and helpers (20.5–20.10) | usable reference | Concept/design reference for parallel research jobs; the app's own background processing follows Ch. 12 |
| 29 | Data cleaning (MAD outliers, tick-data preparation) | `data_load_and_clean.ipynb`, `data/clean_data.py`: `mad_outlier` | didactic reference | Informs Ch. 29 policies; the notebook works on Kibot E-mini/IVE and WDC tick files, not crypto data |

## A.4 Notebook inventory (what each actually contains)

- `ch2.ipynb` — 64 cells. Loads cleaned IVE tick data; builds minute/tick/volume/dollar bars; the full exercise 2.1 battery: weekly bar counts, return autocorrelation per bar type, monthly variance-of-variance, Jarque-Bera normality; dollar imbalance bar attempt (2.2); Bollinger-band crossings on dollar bars (2.4); 2.5 stub.
- `ch3.ipynb` — 20 cells. Daily volatility (`getDailyVol`), dollar bars, triple-barrier events on one day of data; exercises 3.2–3.5 are header-only.
- `ch4.ipynb`, `ch5.ipynb`, `ch9.ipynb` — title cells only. **No content.**
- `ch6.ipynb` — 9 cells. Exercise 6.1 discussion placeholder; Snippet 6.1 (bagging classifier accuracy) via `scipy.special.comb`.
- `ch7.ipynb` — 10 cells. Imports `getTrainTimes`; no worked exercises.
- `ch8.ipynb` — 14 cells. Synthetic dataset (Sn. 8.7); MDI, MDA, and SFI runs with the author's own Snippets 8.8–8.10 helper and plots.
- `data_load_and_clean.ipynb` — 18 cells. Kibot tick-data ingestion, MAD outlier filtering, parquet output; used to produce the `clean_IVE_tickbidask.parq` the other notebooks load.

## A.5 Snippet-file inventory (verbatim book code)

| File | Functions / classes | Book snippets |
|---|---|---|
| `src/snippets/ch2.py` | `getTEvents` | 2.4 (symmetric CUSUM filter) |
| `src/snippets/ch3.py` | `getDailyVol`, `applyPtSlOnT1`, `getEvents`, `addVerticalBarrier`, `getBinsOld`, `getBins`, `dropLabels` | 3.1, 3.2, 3.3, 3.4, 3.5, 3.6, 3.7, 3.8 |
| `src/snippets/ch7.py` | `getTrainTimes`, `getEmbargoTimes`, `PurgedKFold`, `cvScore` | 7.1, 7.2, 7.3, 7.4 |
| `src/snippets/ch8.py` | `featImpMDI`, `featImpMDA`, `auxFeatImpSFI`, `get_eVec`, `orthoFeats`, `kendal_weighted`, `getTestData`, `featImportance` | 8.2–8.8 |
| `src/snippets/ch20.py` | `linParts`, `nestedParts`, `mpPandasObj`, `processJobs_`, `reportProgress`, `processJobs`, `expandCall` | 20.5–20.10 |
| `src/data/clean_data.py` | `mad_outlier` | chapter 3 exercise material |
| `src/features/bars.py` | `tick_bar`, `volume_bar`, `dollar_bar`, `volume_bar_cum`, `dollar_bar_cum` | author's own implementation (not verbatim book code) |
| `src/visualization/candle.py` | `plot_candlestick_ohlc` | plotting helper |

## A.6 Code-quality review (blocking issues for direct reuse)

- **pandas 0.23 idioms.** `bars.py` mutates slices (`volume_bar.loc[idx, 'cum_vol'] = ...` on a filtered frame) — under modern pandas this raises `SettingWithCopyWarning` and can silently misbehave. Any port must operate on explicit copies.
- **Python-loop bar construction.** `volume_bar`/`dollar_bar` accumulate in O(n) Python loops. Fine didactically; must be vectorized (cumsum + searchsorted) for production datasets.
- **No tests, no type hints, no package metadata.** The repo is a notebook companion, not a library. Adapted code must enter this project's test suite (Ch. 14) with provenance comments.
- **Close-path barrier evaluation.** The book's `applyPtSlOnT1` evaluates barriers on close prices. That is a legitimate but consequential simplification; Ch. 49.2 requires documenting whether high/low paths or close-only paths are used, and Ch. 37.5 intrabar-ambiguity rules apply.
- **Stack vintage.** Python 3.6 / pandas 0.23 / sklearn 0.19 APIs differ from the project's target stack; assume API migration work in every port.

## A.7 Gaps that require original implementation

The exercise repository does **not** cover: sample weights by uniqueness (book ch. 4), fractional differentiation (ch. 5), hyper-parameter tuning with purged CV (ch. 9), bet sizing (ch. 10), PBO/CSCV (ch. 11–12), CPCV (ch. 12), PSR/DSR (ch. 14), structural-break tests (ch. 17), and entropy features (ch. 18). Each is mapped to a roadmap section in A.3.

The book's own exhibit compilation (A.9) narrows the gap: it supplies the code snippets for chs. 4, 5, 9, 10, 17 and 18, and the equations for chs. 12 and 14. It does **not** supply the book's prose definitions; where those were missing, the technique was specified from its **primary paper** instead — DSR and PBO/CSCV are now unblocked this way (see `afml-techniques.md` §3.6–§3.7 and its §5 references list); PBO/CSCV in particular is built on the published CSCV algorithm rather than on the book's chapter, whose prose is not in the compilation.

For every technique in this list the implementation must be written from the specification in `docs/en/developers/afml-techniques.md` (our own definitions, exhibit pointers, mandatory parameters and open questions) and reviewed against the printed book (Ch. 49.9), which is cited as authority and never redistributed.

## A.8 Verification checklist before adopting any code from this repository

- [ ] The adapted function reproduces the book's snippet definition (line-level review, not just behavioral similarity).
- [ ] Unit tests cover the edge cases relevant to the roadmap section (interval overlap and embargo boundaries for Ch. 49.4; intrabar ambiguity for Ch. 49.2; NaN handling everywhere).
- [ ] The port passes the project's lint and test suite on the target stack.
- [ ] Provenance is recorded: source file and snippet number in the docstring, and the technique-to-reference mapping in A.3 updated if needed.
- [ ] MIT license attribution is preserved where code, not just ideas, is taken.
- [ ] The roadmap checklist item that required the study (Ch. 25) is checked for this repository.

## A.9 The book's exhibit compilation (local reference)

One-off inventory taken 2026-09-13; the method and the per-technique
consequences are recorded in `afml-techniques.md` §1.

The compilation is a local reference copy held by the developer, **outside this
repository**; it is not tracked and never will be. Only its structure is
recorded here, not the file itself.

| Property | Value |
|---|---|
| Source | the book's exhibit compilation (local reference copy, not in this repository) |
| Pages | 218 |
| Structure | pp. 1–10 = *List of Exhibits* (tables, figures, equations, snippets + page); pp. 11–218 = the exhibits grouped by chapter (`c01`–`c22`) |
| Content | **all 97 book snippets**, plus the book's equations, figures and tables |
| Narrative | fragments only (≈20,600 words in the body, median 69 words/page, 75 of 209 body pages under 50 words) — **this is not the full book** |

Page pointers for the roadmap-relevant chapters:

| PDF chapter | Topic | Key exhibits |
|---|---|---|
| c02 | Financial data structures, CUSUM filter | Table 2.1; Figs. 2.1–2.3; Sn. 2.1–2.4 (pp. 21–23) |
| c03 | Labeling, meta-labeling | Sn. 3.1–3.8 (pp. 26–34); Figs. 3.1–3.2 |
| c04 | Sample weights / uniqueness | Sn. 4.1–4.11 (pp. 35–44); Eq. 32; Figs. 4.1–4.3 |
| c05 | Fractional differentiation | Sn. 5.1–5.4 (pp. 48–53); Table 5.1; Figs. 5.1–5.5 |
| c07 | Cross-validation in finance | Sn. 7.1–7.4 (pp. 63–67); Figs. 7.1–7.3 |
| c08 | Feature importance | Sn. 8.2–8.10 (pp. 68–76); Figs. 8.1–8.4 |
| c09 | Hyper-parameter tuning | Sn. 9.1–9.4 (pp. 79–82) |
| c10 | Bet sizing | Sn. 10.1–10.4 (pp. 87–91); Figs. 10.1–10.3 |
| c11 | Dangers of backtesting (PBO) | Figs. 11.1–11.2 only (p. 94) — **no formula, no code** |
| c12 | Backtesting through CV (CSCV/CPCV) | Eq. 39–44 (pp. 96–99); Figs. 12.1–12.2 |
| c13–c14 | Backtest statistics | Sn. 13.1–13.2 (p. 103); Sn. 14.1–14.4 (pp. 120–124); Eq. 45–53; Table 14.1 |
| c17 | Structural breaks | Sn. 17.1–17.4 (pp. 164–165); §17.3 (p. 156); Eq. 59–66 |
| c18 | Entropy features | Sn. 18.1–18.4 (pp. 168–172); Eq. 67–89 |
| c20 | Multiprocessing and vectorization | Sn. 20.1–20.14 (pp. 187–199) |

Rules: the compilation is a local reference only — **never commit or
redistribute it**; page pointers refer to that copy and to the book's own
exhibit numbering; the per-technique requirements derived from these exhibits
live in `afml-techniques.md`.

---

# Part B — Backtrader (`external/backtrader`)

## B.1 The reference at a glance

| Property | Value |
|---|---|
| Path / upstream | `external/backtrader` · https://github.com/mementum/backtrader |
| License | **GPLv3** — the strictest of all references. Design inspiration only; **no code adaptation** (contrast: the AFML exercises are MIT) |
| Version | 1.9.78.123; last upstream commit 2023-04-19 — stable but effectively in maintenance mode |
| Architecture | Event-driven engine: `Cerebro` orchestrator + strategies + simulated broker + feeds + analyzers/observers + metaclass/lines machinery |
| Tests | Extensive per-indicator and comparison tests under `tests/` — a useful model for Ch. 14 |

## B.2 What maps to which roadmap chapter

### Backtesting engine (Ch. 37) — the core value of this study

- **Broker simulation** (`backtrader/brokers/bbroker.py`, ~1,240 lines): cash/value accounting, order acceptance checks (cash/margin before submission), order states including `Margin` and `Rejected` (`backtrader/order.py`), short-cash policy (`shortcash`). Direct conceptual input for 37.1 (capital and trading costs) and the order/trade domain models of Ch. 7.
- **Order types** (`order.py`): `Market, Close, Limit, Stop, StopLimit, StopTrail, StopTrailLimit, Historical` — covers 37.3 and adds trailing orders beyond the MVP set.
- **Slippage model**: `slip_perc`, `slip_fixed`, `slip_match` (cap at bar extremes), `slip_out` (allow fills outside the high–low range); configured via `set_slippage_perc` / `set_slippage_fixed`. This is a clean, parameterized cost model — exactly the shape 37.1 and 44.3 (cost sweeps) need.
- **Volume-constrained fills** (`backtrader/fillers.py`): `FixedSize`, `FixedBarPerc`, `BarPointPerc` fill only a fraction of the bar's volume. The seed for the liquidity/liquidity-slice model of 56.4.
- **Commission schemes** (`comminfo.py`, `commissions/`): `CommInfoBase` with fixed/percentage commission, stocklike/futures attributes, margin, interest, leverage. See B.4 for what crypto still needs on top.
- **Intrabar execution policy**: fills are evaluated against bar OHLC with fixed matching rules. Our Ch. 37.5 intrabar-ambiguity requirements must go further: Backtrader's policy is implicit, ours must be explicit and documented.
- **Caution — look-ahead shortcuts**: Cerebro supports *cheat-on-close/cheat-on-open* execution and strategies expose `next_open`. These deliberately break the decide-on-bar-close realism. The app must never enable such modes in evidence-producing runs (Ch. 43.5); at most, didactic comparisons with explicit labeling.

### Performance metrics and reporting (Ch. 40–42)

- **Analyzer pattern** (`analyzer.py`): hooks `next`, `notify_order`, `notify_trade`, `notify_cashvalue`/`notify_fund` + `get_analysis`. Analyzers observe the same event stream as strategies — a clean separation the roadmap's metrics chapter can adopt (metrics as observers of the backtest, not post-hoc report hacks).
- **Analyzer suite** (`analyzers/`): `annualreturn`, `calmar`, `drawdown`, `leverage`, `logreturnsrolling`, `periodstats`, `positions`, `pyfolio`, `returns`, `sharpe`, `sqn`, `timereturn`, `tradeanalyzer`, `transactions`, `vwr`. Direct menu for Ch. 40.1–40.4.
- **Observers** (`observers/`): live chart overlays — `broker` (cash/value), `buysell`, `drawdown`, `trades`, `timereturn`, `logreturns`, and **`benchmark` (buy-and-hold comparison)** — the last one is the conceptual seed for Ch. 42 inside the engine, not just in reports.
- **Sizers** (`sizers/`): `fixedsize`, `percents_sizer` — position-sizing hooks. Only two trivial stock-world sizers; crypto sizing per Ch. 58 is our own work.

### Data feeds, resampling, replay (Ch. 26, 28–29)

- **Feed abstraction** (`feed.py`, `feeds/`): `csvgeneric`, `btcsv`, `pandafeed`, `influxfeed`, `quandl`, `yahoo`, IB/Oanda/VirtualChart stores; a filter pipeline (`filters/`, `resamplerfilter.py`) between raw input and engine.
- **Resampling and replay** (`cerebro.resampledata` / `replaydata`; `Resampler`, `Replayer`, `DTFaker`): replay progressively refines the current bar intrabar — conceptually valuable for simulating lower-timeframe execution on higher-timeframe backtests (37.6), with the same intrabar-ambiguity caveats.
- **Multi-timeframe chaining** (`feeds/chainer.py`) and a trading calendar (`tradingcal.py`) + timers (`timer.py`). Caution: calendar/session concepts assume trading sessions; crypto trades 24/7 — the app must not inherit session assumptions (Ch. 29/31 documentation point).

### Parameter optimization (Ch. 39)

- `Cerebro.optstrategy` + `multiprocessing.Pool(maxcpus)` + `optreturn` (stripped result objects to survive large sweeps) + `exactbars` (memory control over long histories). A workable execution pattern — but Backtrader has **no** trial-record, objective-function, or overfitting-discipline concepts; those come from Ch. 39, 43, 44, 49.

## B.3 What Backtrader does **not** provide (verified by search)

- No walk-forward, Monte Carlo, purged CV, or robustness tooling (repo-wide search negative) — Chapters 43–49 are fully our own work.
- No crypto-specific execution: no maker/taker fee distinction, no minimum-order quantity/value rules, no exchange-rule model (Ch. 30, 37.2, 55).
- No paper-trading persistence, no risk manager, no kill-switch concept (Ch. 57–59).
- No regime analysis, no feature/ML infrastructure (Ch. 46–49).

## B.4 Adoption guidance

- [ ] **GPLv3 constraint**: design inspiration and terminology only — zero code adaptation; do not vendor or link any Backtrader code into the application.
- [ ] Where the analyzer/observer pattern is adopted, implement it from the roadmap's own specification (original code, own tests).
- [ ] Design the liquidity model (56.4) from the *concept* of the fillers; extend with partial fills, maker/taker fees, and exchange rules Backtrader lacks.
- [ ] Treat cheat-on-close/cheat-on-open as forbidden in evidence-producing runs; if ever implemented for teaching, mark outputs as non-evidence (43.5).
- [ ] Maintenance mode ⇒ pin exact reference commit in this document when concepts are adopted; re-verify on any submodule update.
- [ ] Record every adopted *idea* (not code) in this document and in the relevant roadmap chapter.

---

# Part C — VectorBT (`external/vectorbt`)

## C.1 The reference at a glance

| Property | Value |
|---|---|
| Path / upstream | `external/vectorbt` · https://github.com/polakowo/vectorbt |
| License | **Apache 2.0 with Commons Clause** ("Fair Code"). Use and modification are permitted with attribution and notice preservation, but the license does **not** grant the right to *Sell* — to charge for a product or service whose value derives substantially from VectorBT. Read `LICENSE.md` before any distribution decision. More permissive than Backtrader's GPLv3, but not unrestricted. |
| Version | 1.1.0; **actively maintained** (last commit 2026-08-02, commit `34b6d59`) |
| Architecture | Vectorized engine over numpy/pandas with numba JIT compilation: entire parameter grids are simulated in one call. The opposite paradigm to Backtrader's event loop — both are informative exactly because they differ |
| Stack requirements | pandas ≥ 3.0, numpy ≥ 2.4, numba ≥ 0.66 (llvmlite), plotly, scikit-learn; optional extras pull TA-Lib, ccxt, python-binance, alpaca-py, yfinance. Bleeding-edge requirements — Debian/PyPI availability must be verified per Chapter 4 |

## C.2 What maps to which roadmap chapter

### Backtesting and execution simulation (Ch. 37, 55)

- **`Portfolio.from_signals`** (`vectorbt/portfolio/base.py`): signal-driven simulation with broadcastable per-column parameters: `size`, `size_type`, `fees`, `fixed_fees`, `slippage`, `allow_partial`, `raise_reject`, `upon_opposite_entry`, `direction`, `val_price`, `init_cash`, `call_seq`, `group_by`. The parameter surface matches the cost/constraint model of 37.1–37.3 well.
- **Native stop engine**: `sl_stop` (including trailing), `tp_stop`, `use_stops`, `stop_entry_price`, `stop_exit_price`. Useful complement to 49.2 barrier logic and stop-order handling; note that stop resolution paths are configurable and the intrabar-ambiguity rules of 37.5 remain our own to document.
- **`Portfolio.from_orders` / `from_order_func`**: order-level and low-level order-loop constructors — a bridge toward the execution realism of Ch. 55 without abandoning vectorization.
- **Trade and drawdown records**: `Records` (`vectorbt/records/base.py`), `EntryTrades`/`ExitTrades`/`Positions` (`portfolio/trades.py`), and `Drawdowns` (`generic/drawdowns.py`, per-event depth/timing records). First-class trade records are exactly what 40.2 needs; drawdown *events* are richer than a single max-drawdown number (40.3).

### Performance metrics and statistical evaluation (Ch. 40, 43, 49.5)

- **Metrics registry pattern** (`vectorbt/returns/metrics.py` + `generic/stats_builder.py`): named, documented metric functions surfaced through a `.stats()` builder. This is the pattern to mirror for Ch. 40's metric catalog.
- **Native `deflated_sharpe_ratio` and `approx_exp_max_sharpe`**: directly relevant to 49.5. Caveats: it is a normal-approximation implementation requiring `nb_trials` explicitly — good, because 49.5 demands recorded trial counts — but it must be validated against the book's definition before use (49.9). No probabilistic Sharpe ratio found; PSR stays an original implementation.

### Data splitting and walk-forward (Ch. 38, 45)

- **`vectorbt/generic/splitters.py`**: `RangeSplitter`, **`RollingSplitter` ("rolling walk-forward splitter")**, **`ExpandingSplitter` ("expanding walk-forward splitter")** — native walk-forward range generation matches Ch. 45's window mechanics.
- **No purging or embargo anywhere in the repository** (verified by search). The splitters are not leakage-safe for overlapping labels; 38.8/49.4 remain our own work (AFML's `PurgedKFold` is the reference). Any leakage-sensitive use requires a purge+embargo wrapper around these splitters.

### Feature engineering, labeling, signals, data (Ch. 31, 33, 47, 49)

- **`vectorbt/labels/generators.py`**: `FIXLB`, `MEANLB`, `LEXLB`, `TRENDLB`, `BOLB` label generators — useful baseline labels. No triple-barrier generator (no `PROLB`); 49.2 stays AFML-based.
- **`vectorbt/signals/generators.py`**: random entry/exit generators (`RAND`, `RANDX`, `RANDNX`, `RPROB`, `RPROBX`, `RPROBCX`, `RPROBNX`) and stop/exit generators (`STX`, `STCX`, `OHLCSTX`, `OHLCSTCX`). The random generators are ideal for the null-baseline comparisons Ch. 43 encourages.
- **`IndicatorFactory`** (`indicators/factory.py`) and `basic.py`: parameterized indicator classes with built-in grids, plotting, and stats — a strong pattern reference for the Ch. 31 indicator catalog and Ch. 47 feature catalog.
- **`vectorbt/data/custom.py`**: `YFData`, `BinanceData`, `CCXTData`, `AlpacaData`, plus `SyntheticData`/`GBMData` (synthetic GBM paths — useful for method validation and Monte Carlo-style studies under explicit assumptions).

### Parameter optimization (Ch. 39)

- The core paradigm *is* the optimization story: broadcasting parameter grids through the engine simulates thousands of configurations cheaply. Excellent for controlled grid experiments (39.1, 39.3) — but mass backtesting at near-zero cost amplifies exactly the multiple-testing risks that Ch. 43.2, 49.5, and 49.6 exist to control. Discipline must scale with capability.

## C.3 What VectorBT does **not** provide (verified by search)

- No purged/embargoed CV, no PBO/CSCV, no CPCV (repo-wide search negative).
- No Monte Carlo scenario framework (random signal generators exist, but no trade-reshuffling/scenario engine for 44.4).
- No meta-labeling, no sample-weight/uniqueness tooling (Ch. 49.3, 49.8).
- No GUI, no persistence, no paper-trading loop, no risk manager, no kill switch (Ch. 57–59) — notebook-centric by design.

## C.4 Adoption guidance

- [ ] **License discipline**: Apache 2.0 + Commons Clause permits using, studying, and adapting VectorBT inside the application, but forbids selling a product/service whose value derives substantially from it. Re-read `LICENSE.md` before any commercialization or paid-hosting decision; keep the Commons Clause notice in any attribution file.
- [ ] **Dependency reality check (Ch. 4)**: numba/llvmlite JIT toolchain and pandas ≥ 3.0 / numpy ≥ 2.4 are demanding. Verify Debian/PyPI availability before making VectorBT a runtime dependency; treat it as an optional research-tier dependency until proven.
- [ ] **Vectorized ≠ event-realistic**: use `from_orders`/`from_order_func` and the stop engine as inspiration, but the evidence rules (37.5, 37.7, 43.5) remain ours; document every close-path assumption.
- [ ] **Wrap splitters with purge+embargo (49.4) before leakage-sensitive use**; never use the raw walk-forward splitters with overlapping labels.
- [ ] **DSR**: adapting the metric is license-permitted, but validate against the book and keep trial records per 49.5.
- [ ] **Actively maintained**: pin the studied commit (`34b6d59`, 2026-08-02) here; re-verify conclusions on submodule updates.

---

# Part D — CCXT (`external/ccxt`)

## D.1 The reference at a glance

| Property | Value |
|---|---|
| Path / upstream | `external/ccxt` · https://github.com/ccxt/ccxt |
| License | **MIT** (© 2024 Igor Kroitor) — the most permissive of all references; study, use, and adapt with attribution preserved |
| Version | 4.5.77; **hyperactive maintenance** (last commit 2026-09-06 — the day of this study); multi-language monorepo (JS/PHP/Python/C#/Go generated from one metadata set) |
| Scale | 105 synchronous exchange classes, 77 `pro/` WebSocket classes, plus `async_support/` asyncio mirrors (`class binance(Exchange, ImplicitAPI)`) |
| Structure note (v4) | The unified abstract API lives in `python/ccxt/base/exchange.py` (142 unified `fetch_*`/`create_order`/`cancel_order` definitions); `python/ccxt/abstract/` now holds per-exchange description files. APIs move between releases — pin the studied commit and isolate CCXT behind our adapter |

## D.2 What maps to which roadmap chapter

### Exchange adapters and market data (Ch. 26) — the core value of this study

- **Unified API**: `fetch_markets`, `fetch_ohlcv` (+ `*_ws` variants), `fetch_balance`, `fetch_ticker`, `fetch_trades`, `fetch_order_book(s)`, `fetch_my_trades`, `create_order`, `cancel_order` — one interface across ~100 exchanges. This is the same shape as our `ExchangeAdapter` interface (26): CCXT is the leading candidate to become the *implementation behind* that interface rather than a parallel abstraction.
- **WebSocket layer** (`python/ccxt/pro/`): `watch_ticker(s)`, `watch_ohlcv(_for_symbols)`, `watch_order_book`, `watch_trades(_for_symbols)` verified on `pro/binance.py` — maps onto Ch. 26's subscription requirements (tickers, candles, trades, order book).
- **Binance Spot Testnet**: explicit `testnet.binance.vision` REST URLs built into `binance.py` — matches Ch. 26.2 exactly. Binance *futures* testnet URLs also exist and must remain unused (Ch. 26 forbids futures).
- **Coinbase**: `coinbase.py` (Advanced Trade-era class; `sandbox: False` observed — sandbox support is limited), `coinbaseexchange.py` (with `'test'` URL set), `coinbaseinternational.py`. Ch. 26.3's sandbox warning (static/predefined data) remains applicable; public market data first.
- **Error taxonomy** (`python/ccxt/base/errors.py`, 23 classes): `ExchangeError` → `AuthenticationError`, `InsufficientFunds`, `InvalidOrder`; `NetworkError` → `RateLimitExceeded`, `ExchangeNotAvailable`. Direct input for Ch. 27's state machine (map onto `AUTHENTICATING`, `RATE_LIMITED`, `ERROR`, `RECONNECTING`) and for Ch. 26 adapter error translation.
- **Rate limiting**: `enableRateLimit` (default `True`) with `throttle()` in `base/exchange.py`. Our adapters must keep this enabled and still surface `RATE_LIMITED` state explicitly (Ch. 27) rather than silently waiting.

### Market precision and exchange rules (Ch. 30) and costs (37.1, 55)

- **Precision machinery** (`base/exchange.py`, `decimal_to_precision`): `DECIMAL_PLACES`, `TICK_SIZE`, `SIGNIFICANT_DIGITS` modes; `price_to_precision`, `amount_to_precision`, `cost_to_precision`; per-market `precisionMode` (some exchanges use tick size, others decimal places). This is exactly the normalization problem Ch. 30 must own — adopt the *model* (three precision modes + per-market declaration), implementing in our domain or wrapping CCXT's.
- **Market structure**: per-market `limits` (amount/cost minima) and per-market `maker`/`taker` fees incl. tiered structures (verified in `binance.py`) — feeds 37.1, 37.2, and 55 (maker/taker distinction that Backtrader lacks).

### Where CCXT ends (verified by search)

- **No data-quality framework**: no dataset identity/versioning/checksums (Ch. 29 stays ours); fetched data must enter our dataset pipeline immediately.
- **No backtesting, paper-trading, risk, or qualification tooling** (Ch. 37, 57–59 ours); CCXT is a live-exchange integration library, not a research platform.
- **Historical-candle depth is exchange-dependent**: what `fetch_ohlcv` can retrieve varies per exchange and plan; bulk historical data needs our own import pipeline (Ch. 28).
- **Async model**: `async_support/` and `pro/` are asyncio-based; bridging into the Qt/background-processing architecture (Ch. 12) is a deliberate design task, not a drop-in.

## D.3 Adoption guidance

- [ ] **Adapter isolation**: never let CCXT types leak into the central domain (Ch. 26 rule); the `ExchangeAdapter` interface owns the boundary so CCXT can be upgraded or replaced.
- [ ] **Pin the studied commit** in this document; CCXT ships breaking structural changes between minor versions (v4 moved the abstract API) — re-verify on submodule updates.
- [ ] **Debian/PyPI check (Ch. 4)**: pure-Python package (no compiled extensions); packaging metadata lives under `python/`. Verified during this study: **no `python3-ccxt` package exists in Debian's apt sources** — the PyPI route (preferably in the app's isolated environment) is the only one; confirm the current CCXT version's Python requirement during the Chapter 4 evaluation. Until then treat as the reference implementation behind `MockExchange`-parallel adapters.
- [ ] **Feature-verification rule (Ch. 25)**: CCXT normalizes ~100 exchanges, but per-exchange quirks remain; verify each claimed capability (testnet endpoints, fee structure, precision mode) per exchange before relying on it.
- [ ] **Never enable the futures testnet**; spot-only scope is a roadmap rule (Ch. 26.2).
- [ ] **Credentials**: CCXT instances take API keys directly — instances may only be constructed inside the credential store boundary (Ch. 9), never from UI code.

---

# Part E — Freqtrade (`external/freqtrade`)

## E.1 The reference at a glance

| Property | Value |
|---|---|
| Path / upstream | `external/freqtrade` · https://github.com/freqtrade/freqtrade |
| License | **GPLv3** — verbatim GNU GPLv3 `LICENSE` (35,141 bytes); `pyproject.toml:17` says `license = {text = "GPLv3"}`. **No code adaptation**: any derivative copying this code must itself be GPLv3. Design inspiration only — the Backtrader posture (Part B). The adaptable references are the permissive ones: AFML exercises (MIT, Part A), CCXT (MIT, Part D), Hummingbot (Apache-2.0, Part F), Jesse (MIT, Part G), TA-Lib (BSD-3, Part H). |
| Version | `2026.9-dev` (`freqtrade/__init__.py`), studied commit `c064be532` (2026-09-13). **Actively maintained** — pin the commit whenever a concept is adopted. |
| Architecture | One package, `freqtrade/` — 333 `.py` files, 65,363 lines: `freqtradebot.py` + `worker.py` + `wallets.py`; `commands/`; layered `configuration/` + `config_schema/`; an exchange abstraction over **CCXT** in `exchange/` (29 files); a pandas-DataFrame strategy interface in `strategy/`; Optuna-based `optimize/`; SQLAlchemy `persistence/`; opt-in `plugins/protections/`; `rpc/`; an ML pipeline in `freqai/`. |
| Tests | 142 files / 78,688 lines / ~1,650 test functions under `tests/`, plus 10 CI workflows — a stronger Ch. 14 model than Backtrader's ad-hoc suite. |

## E.2 What maps to which roadmap chapter

### Strategy/execution separation (Ch. 33) — the core structural lesson

- **Signals are data, not orders — enforced by construction.** `IStrategy` (`freqtrade/strategy/interface.py`, ~1,908 lines) contracts on `populate_indicators` / `populate_entry_trend` / `populate_exit_trend`, each taking and returning a DataFrame (`:236-280`). `grep -rn "create_order\|cancel_order\|fetch_balance" freqtrade/strategy/` returns **nothing**, which is exactly Ch. 33.6's "strategies generate signals but do not create exchange orders".
- **Signal contract**: boolean columns `enter_long`/`exit_long`/`enter_short`/`exit_short` (+ optional `enter_tag`/`exit_tag`), read at the last closed candle (`:1321-1408`); contradictory states are resolved conservatively — an `enter_long` is *discarded* if `exit_long` or `enter_short` is also set (`:1380-1390`). Direct input for Ch. 33.5's signal enum and one-signal-per-timestamp rule.
- **Decision hooks, not authority**: the strategy may veto (`confirm_trade_entry`/`confirm_trade_exit`, `:359-431`), propose a stop distance that "can never be below `self.stoploss`" (`:446-459`), a stake (`:625-652`) or an exit (`:594-623`) — but the bot owns order creation (`freqtradebot.py:1061`, `:2247`).
- **Divergence to correct, not copy**: Freqtrade copies strategy-declared `stake_amount`/`max_open_trades` into the config (`resolvers/strategy_resolver.py:58-127`). Our Ch. 33.6/58 rule is stricter — refuse strategy-supplied risk limits entirely.
- **Parameter declaration** (`strategy/parameters.py`, 422 lines): `IntParameter`/`DecimalParameter`/`CategoricalParameter`/`BooleanParameter` with `default`, `low`, `high`, `space`, `optimize`, `load` — the shape Ch. 33.4 wants. **Missing there**: per-parameter descriptions, validation rules, and any strategy version/identifier (`version()` simply returns `None`, `interface.py:870-874`; `grep -rn "strategy_id\|strategy_uuid"` → 0 hits). Those are our additions.
- **Enforcement ideas worth re-implementing (Ch. 33.7)**: `strategy_safe_wrapper` catches every exception from user code and **deep-copies the trade object** so callbacks cannot mutate domain state (`strategy/strategy_wrapper.py`, 62 lines); `StrategyResultValidator.assert_df` rejects a strategy that changed the frame's length, last close or last timestamp (`strategy/strategy_validation.py:21-42`); `ignore_expired_candle` (`interface.py:1410-1414`) refuses a signal from an expired candle.

### Dry-run (paper) mode and execution consistency (Ch. 57) — the most reusable design

- **Paper trading is a run mode, not a fork.** `RunMode` (`enums/runmode.py`) enumerates `LIVE`/`DRY_RUN`/`BACKTEST`/`HYPEROPT`; `Exchange.create_order` (`exchange/exchange.py:1487-1504`, in a 4,278-line file) returns `create_dry_run_order(...)` when `dry_run` is set. Bot, strategy, wallet and persistence code are identical between modes — precisely Ch. 57.3's "do not create a separate simplified trading logic".
- **Fill simulation**: market orders walk the L2 book and interpolate an average price (`get_dry_market_fill_price`, `exchange.py:1293-1338`), clamped to a **worst-case 5 % slippage** bound (`:1250-1252`), charged the **taker** fee; limit orders fill as maker or taker (`:1275-1291`, `:1368-1433`). **Limitations we must beat**: with no L2 book a limit order is treated as immediately crossed (`:1349-1351`), and fills are **all-or-nothing** — `filled` is set to the full amount (`:1419-1425`), so there is no partial-fill model that Ch. 57.2 requires.
- **Credentials**: dry-run strips exchange API keys unless the venue requires them (`exchange.py:258-262`, `configuration/config_secrets.py:53-70`), so paper mode is structurally incapable of placing a real order.
- **Account history**: `Wallets` (538 lines) derives balances from open trades in dry-run and from the exchange live (`wallets.py:107-186`); `WalletHistory` persists a daily per-currency equity snapshot (`persistence/wallet_history.py`, 50 lines) — a concrete answer to Ch. 57.1.
- **Shared lifecycle**: `LocalTrade` (`persistence/trade_model.py:385`) is used by both the live bot and the backtester (`grep -rn "LocalTrade" freqtrade/optimize/backtesting.py` → 20+ uses in the 1,977-line engine), the same reuse principle Ch. 57.3 demands.

### Persistence (Ch. 57.1, 39.6) — a proven ORM shape

- SQLAlchemy 2.0 with scoped sessions in `init_db` (`persistence/models.py:48-99`). `Trade` (`trade_model.py:1706-1791`) records strategy name, entry/exit tags and reasons, fees and fee currencies, precision modes, leverage, stop-loss/trailing state; `Order` (`:65-118`) records the full venue lifecycle (`order_id`, `status`, `filled`, `remaining`, `cost`, `stop_price`, timestamps). Migrations exist in both directions (`persistence/migrations.py`, 482 lines; `db_migration.py`, 81 lines) plus a generic `KeyValueStore` (214 lines).
- **Caveat**: this stores *bot trade state*, not *experiment or dataset identity* (see E.3).

### Hyperopt / parameter optimization (Ch. 39) — search machinery, not overfitting discipline

- **Optuna, seeded**: `HyperOptimizer.get_optimizer` builds TPE/GPSampler/CmaEs/NSGA-II with an explicit `random_state` (`optimize/hyperopt/hyperopt_optimizer.py:406-438`), satisfying Ch. 39.1's seed requirement.
- **Pluggable objectives**: `IHyperOptLoss` plus 12 built-ins (`constants.py:33-46`) — `OnlyProfit`, `Sharpe`/`SharpeDaily`, `Sortino`/`SortinoDaily`, `Calmar`, `MaxDrawDown`(+`Relative`,+`PerPair`), `ProfitDrawDown`, `MultiMetric`, `ShortTradeDur`. `MultiMetricHyperOptLoss` (`optimize/hyperopt_loss/hyperopt_loss_multi_metric.py:54-104`) is a good counterexample to Ch. 39.3's "must not select on net profit alone" — **but `OnlyProfitHyperOptLoss` ships in the same catalogue and no default loss is supplied** (`commands/cli_options.py:365-373`), so nothing forces a risk-adjusted objective.
- **Safety knobs map only partially to 39.2**: `--epochs` bounds combinations, `--early-stop` installs a stagnation terminator (`:432-435`, warning below 20 % of epochs at `:102-104`), and `hyperopt_min_trades` assigns `MAX_LOSS` to trials with too few trades (`:369-380`). **No wall-clock cap, no memory cap, no cancel button** beyond Ctrl-C.
- **Trial records are persisted but incomplete**: every epoch appends to a results file and the best parameters export as `<Strategy>.json` carrying only `strategy_name`, `params`, `ft_stratparam_v`, `export_time` (`optimize/hyperopt_tools.py:63-103`). No dataset version, timerange, fee/slippage model or experiment id — Ch. 39.1/39.6 are **not** satisfied by this export.
- **Overfitting protection is guidance, not code**: `grep -rni "overfit" freqtrade/ --include=*.py` finds only FreqAI dropout comments and one config description.

### Bias-detection tooling (Ch. 43.5) — a bonus for our evidence rules

- `lookahead-analysis` and `recursive-analysis` subcommands (`commands/arguments.py:700-718`) backed by `optimize/analysis/` (`lookahead.py` 287 lines, `recursive.py` 229). They re-run a strategy over truncated data and compare emitted signals to detect look-ahead, and perturb startup-candle counts to detect recursive warm-up bias. Research-integrity tooling, not capital protection — and a concrete precedent for Ch. 43.5.

### Capital protection vs performance optimization (Ch. 25 mandate)

**Protects capital** (eligible to back a capital-protection feature):

- **Paper by default**: when the `dry_run` key is absent the mode is `DRY_RUN` (`configuration/configuration.py:472` — `RunMode.DRY_RUN if config.get("dry_run", True) else RunMode.LIVE`); dry-run also defaults to an in-memory database (`:149-157`) and strips API keys.
- **A mandatory, negative stop-loss**: `stoploss` is required and its schema forces it negative (`config_schema/config_schema.py:122-126`, `maximum: 0`, `exclusiveMaximum: true`); `stoploss == 0` is rejected (`configuration/config_validation.py:128-132`); a custom stop may only tighten, never loosen (`interface.py:459`).
- **Position and exposure caps**: `max_open_trades`, `stake_amount`, `tradable_balance_ratio` (default **0.99**) enforced in `Wallets.get_trade_stake_amount` (`wallets.py:347-406`), with a guard that both cannot be unlimited (`config_validation.py:102-110`). Sizing is fixed-stake or split-the-balance only.
- **Entry locks ("protections")**: `StoplossGuard`, `MaxDrawdown`, `CooldownPeriod`, `LowProfitPairs` under `plugins/protections/`, orchestrated by `ProtectionManager` (`plugins/protectionmanager.py:50-94`), writing time-bounded `PairLock`s with a reason. **Critical nuance: they only block new entries** — no exit path consults them — and they are **opt-in** (`IStrategy.protections: list = []`, `interface.py:131`).
- **Cautious defaults**: bot state defaults to `STOPPED` (`freqtradebot.py:162-163`); the README leads with "Always start by running a trading bot in Dry-Run" (`README.md:13-22`).

**Only optimizes historical performance** (research chapters; must never back a protection claim): the entire `optimize/` tree — the Optuna loop, all 12 loss functions, the backtesting engine and its reports. The hyperopt "safety" limits bound the *search*, not the risk of the result.

**Honest caveats**: `MaxDrawdown` ships with `max_allowed_drawdown = 0.0`, `trade_limit = 1` and a `TODO: Implement checks to limit max_drawdown to sensible values` (`plugins/protections/max_drawdown_protection.py:23-26`); and because protections only lock entries, there is **no portfolio-level kill switch that halts the bot and flattens positions**. Our Ch. 58/59 must supply both.

## E.3 What Freqtrade does **not** provide (verified by search)

- **No walk-forward** — `grep -rni "walk.forward\|walkforward" freqtrade/ --include=*.py` → **0 hits**; over `docs/` → **0 files**. Ch. 45 is ours.
- **No Monte Carlo** — `grep -rni "monte.carlo\|trade.reshuffl" freqtrade/ --include=*.py` → **0 hits**. Ch. 44.4 is ours.
- **No cross-validation of any kind** — `purged|embargo` → **0 hits**; `cross.val|cross_val` → **0 hits**.
- **No out-of-sample split** — `out.of.sample|oos_` → **0 hits**; `docs/hyperopt.md` mentions neither. Hyperopt optimizes the whole requested timerange at once.
- **No risk-based sizing** — `risk_per_trade|risk_percent|position_sizing|atr.*siz` → **0 hits**. Sizing is `stake_amount` or balance ÷ `max_open_trades` only.
- **No kill switch / flatten-all** — `kill.switch|killswitch|flatten_all|close_all_positions` → **0 hits**. Forcing exits is manual via RPC.
- **No exposure or correlation limits** — `max_exposure|max_total_exposure|correlation` → **0 hits**; `max_open_trades` is a count, not an exposure budget.
- **No strategy identity/version registry** — `strategy_id|strategy_uuid` → **0 hits**; `version()` returns `None` and is never persisted. Ch. 33.1 is ours.
- **No dataset/experiment identity in optimization records** — the export carries only strategy name, params, format version and time (see E.2). Ch. 39.1/39.6 are ours.
- **No partial-fill simulation** — fills are all-or-nothing; `partial` in `exchange.py` refers to partial *candles*. Ch. 57.2 is ours.
- **Not verified**: nothing was installed or executed; every claim is static reading of commit `c064be532`. Live/testnet behaviour was not exercised, and `freqai/` (46 files, 7,387 lines) was not evaluated feature-by-feature because the Ch. 25 pointers cover only Ch. 33/39/57.

## E.4 Adoption guidance

- [ ] **License first — GPLv3, inspiration only.** Zero code adaptation, no vendoring, no linking; copying would oblige this application to be GPLv3. Recorded so no future reader treats Freqtrade as adaptable.
- [ ] **Adopt the dry-run pattern, not its code (Ch. 56/57.3)**: one execution path with the simulator selected behind the same order interface used by backtest/paper/live. Implement our own explicit fill model (book walk, documented slippage clamp, maker/taker fees, partial fills) and label every paper fill as simulated (57.7). Do **not** inherit the silent immediate-fill fallback or the all-or-nothing fill.
- [ ] **Harden the strategy boundary beyond Freqtrade (Ch. 33.6/58)**: reject strategy-supplied `stake_amount`/`max_open_trades` outright — configuration and the risk manager always win. Record the divergence here so nobody "fixes" it back.
- [ ] **Re-implement two enforcement ideas as original code (Ch. 33.7)**: callbacks receive deep copies of domain objects; the returned frame is validated (length, last close, last timestamp) before caching.
- [ ] **Signal contract (Ch. 33.5)**: boolean columns plus a `SignalType` enum, one resolved direction per timestamp, contradictions resolved conservatively, and — unlike Freqtrade — carry strategy id, version, market, timeframe and parameter-set id in the emitted signal.
- [ ] **Parameter metadata (Ch. 33.4)**: take the `default`/`low`/`high`/`space`/`optimize` shape, then add the description, allowed values, validation rules and version binding the roadmap demands.
- [ ] **Fill the Ch. 39 gaps rather than the Optuna gap**: record dataset version, timerange, fee/slippage model, seed and experiment id per optimization record; add a validation/out-of-sample stage, parameter-stability analysis and degradation reporting; enforce combination/time/memory caps plus a cancel button. **Do not import the unbounded Optuna search** — the roadmap allows only limited grid and random search — and do not inherit `OnlyProfitHyperOptLoss` as a selectable default.
- [ ] **Protections as a hardened concept for Ch. 58/59**: entry locks (global/per-pair, time-bounded, reason-carrying) are the right shape and `StoplossGuard`/`MaxDrawdown`/`CooldownPeriod` are a useful requirements checklist — but ours must be **default-on, config-owned, impossible for a strategy to bypass**, and complemented by the kill switch (halt + flatten) and risk-based sizing that Freqtrade lacks.
- [ ] **Never cite hyperopt results as capital protection.** Every loss function and the backtest engine belong to Ch. 39/44–45; only the stop-loss, sizing caps, dry-run default, entry locks and cautious defaults may back a protection claim (Ch. 25 rule).
- [ ] **Onboarding precedent**: mirror Freqtrade's blunt "do not risk money you are afraid to lose" + dry-run-first posture (Ch. 24/57.8).
- [ ] **Pin `c064be532` (2026-09-13)** and re-verify on any submodule update; the hyperopt, protection and dry-run APIs will move. Freqtrade depends on `ccxt>=4.5.76` (`pyproject.toml`), the same library studied in Part D.

---

# Part F — Hummingbot (`external/hummingbot`)

## F.1 The reference at a glance

| Property | Value |
|---|---|
| Path / upstream | `external/hummingbot` · https://github.com/hummingbot/hummingbot |
| License | **Apache 2.0** — `LICENSE`, 201 lines, "Copyright 2023 Hummingbot Foundation"; the unmodified Apache text with no Commons Clause and no non-commercial rider. **Code adaptation is permitted** with attribution, license/notice preservation and a statement of changes (Apache-2.0 §4) — unlike Backtrader's GPLv3 and Freqtrade's GPLv3. |
| Version | **2.16.0** (`hummingbot/VERSION`), studied commit `2bfaccc48` (2026-07-30), `git describe` = `v2.16.0-2-g2bfaccc48`. Note `setup.py` hardcodes `version = "20260729"`, a dev build string — trust `VERSION` + `git log -1`. |
| Architecture | `hummingbot/` — 896 `.py` + 60 `.pyx` (Cython): `connector/` (27 spot subpackages + `gateway/` + `test_support/` + the base classes `connector_base.pyx`, `exchange_base.pyx`, `exchange_py_base.py`, `client_order_tracker.py`, `trading_rule.pyx`), `core/` (`data_type/`, `event/`, `api_throttler/`, `utils/`), `data_feed/`, `strategy/` (v1), `strategy_v2/` (`controllers/`, `executors/`, `backtesting/`), `model/` (SQLAlchemy), `client/` + `cli/`. **Caution:** `hummingbot/README.md`'s "Project Breakdown" is stale — it lists `pmm_script/`, `smart_components/` and `core/mock_api/`, none of which exist. Read the tree, not that file. |
| Tests | 665 files / **197,081 lines** under `test/`, mirroring the source. The notable asset is a **reusable abstract connector contract**: `connector/test_support/exchange_connector_test.py` (1,980 lines) and `oms_exchange_connector_test.py` (1,000 lines) drive every exchange class through the same mocked REST/WS scenarios. |

## F.2 What maps to which roadmap chapter

### Order lifecycle and tracking (Ch. 56) — the core value of this study

- **A richer order-state vocabulary** (`core/data_type/in_flight_order.py:21-32`): an 11-member `OrderState` enum — `PENDING_CREATE, OPEN, PENDING_CANCEL, CANCELED, PARTIALLY_FILLED, FILLED, FAILED`, plus `PENDING_APPROVAL/APPROVED/CREATED/COMPLETED`. It names the two states our Ch. 56 needs most and Backtrader lacks.
- **`InFlightOrder`** (`:87-393`): keeps client order id **and** exchange order id separately; accumulates executed base/quote amounts; stores fills in `order_fills: Dict[trade_id, TradeUpdate]` so a repeated fill is **idempotent** (`update_with_trade_update`, `:351`, returns `False` on a duplicate `trade_id`); exposes `is_open`/`is_done`/`is_filled`/`is_cancelled`; and uses `asyncio.Event`s plus `wait_until_completely_filled` (`:377`).
- **`ClientOrderTracker`** (`connector/client_order_tracker.py`, 444 lines) — the machinery worth imitating: three registries (`_in_flight_orders`, `_cached_orders` as a 30 s `TTLCache`, `_lost_orders`); tracking starts **before** submission (`exchange_py_base.py:400-431`) so an order that never returns an id is still recoverable; `process_order_not_found` (`:221`) counts not-found responses and after `lost_order_count_limit = 3` marks the order `FAILED` and keeps polling it so late fills are caught; `restore_tracking_states` (`:153`) rebuilds open and lost orders from JSON on restart; `TRADE_FILLS_WAIT_TIMEOUT = 5` s waits for fills before accepting `FILLED` and logs a warning when it gives up — an honest "incomplete information" path.
- **Honest caveat — there is no guarded state machine to copy.** `update_with_order_update` (`:327-341`) assigns `self.current_state = order_update.new_state` with no transition table and no validity check. Hummingbot supplies the status set and the recovery machinery, **not** the FSM; our transition table must come from Ch. 56's own specification.
- **Event taxonomy** (`core/event/events.py`, 330 lines): `MarketEvent` (`:13`), `OrderFilledEvent` (`:158`, carrying price, amount, fee, exchange trade id, leverage), `BuyOrderCreatedEvent`/`SellOrderCreatedEvent` (`:220`/`:234`), `OrderCancelledEvent`, `MarketOrderFailureEvent`. **Two generations coexist** — the legacy `in_flight_order_base.pyx` + `strategy/order_tracker.pyx`, and the current `InFlightOrder` + `ClientOrderTracker`. Adopt only the latter's shape.

### Connector architecture (Ch. 26) — the other core value

- **Template-method connector base** (`connector/exchange_py_base.py`, 1,137 lines): `ExchangePyBase` owns the whole lifecycle and leaves roughly a dozen abstract methods to the exchange — `_place_order`, `_place_cancel`, `_get_fee`, `_request_order_status`, `_all_trade_updates_for_order`, `_update_balances`, `_format_trading_rules`, `_create_order_book_data_source`, `_create_user_stream_data_source`, `supported_order_types`, plus the `_is_order_not_found_*` predicates. **This split is the single most transferable idea for our `ExchangeAdapter`: a thin per-exchange subclass over a base that owns the boring, error-prone parts.** Scale implication: a Hummingbot connector is a *subpackage* (~8 files), not one file.
- **Explicit readiness gate** (`:177-191`): `status_dict` must show symbols mapped, order books initialised, account balance (skipped when `is_trading_required` is false), trading rules and user stream before `ready` is `True`. Good input for Ch. 27's adapter state machine.
- **Pre-submission validation, failing closed** (`_create_order`, `:400-477`): quantize price and amount, start tracking, reject unsupported order types, reject `amount < min_order_size`, reject `notional < min_notional_size`, then place. Exactly the Ch. 30/56 validation chain.
- **Exchange rule model** (`connector/trading_rule.pyx`, 55 lines): `min_order_size`, `max_order_size`, `min_price_increment`, `min_base_amount_increment`, `min_quote_amount_increment`, `min_notional_size`, `min_order_value`, `max_price_significant_digits`, `supports_limit_orders`, `supports_market_orders`, buy/sell collateral tokens. **Richer than CCXT's `limits` + `precision`** (Part D) and a direct strengthening of Ch. 30.
- **Order-book sequencing** (`core/data_type/order_book_tracker.py`, 742 lines): `_order_book_diff_router` (`:522`) **rejects any diff not newer than the snapshot's `update_id`** and buffers diffs for pairs with no snapshot yet; on a snapshot, `restore_from_snapshot_and_diffs` (`core/data_type/order_book.pyx:478`) `bisect`s the diff window and replays only the later diffs. Genuine Ch. 26/29 discipline.
- **Gap detection that fails closed**: `check_candles_sorted_and_equidistant` (`data_feed/candles_feed/candles_base.py:276`) logs a warning and **resets the candle buffer** when timestamps are unsorted or non-equidistant — a good precedent for Ch. 29.
- **Rate limits as first-class data**: `core/api_throttler/` plus per-exchange declarations — `connector/exchange/binance/binance_constants.py` (115 lines) models Binance's pools (`REQUEST_WEIGHT` 6000/min, `ORDERS` 100/10 s, `RAW_REQUESTS`) with *weighted* endpoints via `LinkedLimitWeightPair`, alongside the error codes meaning "order not found" (`-2013`, `-2011`).
- **Warning — no Binance spot testnet.** `binance_constants.py:5` is `DEFAULT_DOMAIN = "com"` and no testnet domain exists in the Binance spot connector. **CCXT (Part D) remains the correct Ch. 26.2 pointer.**

### Execution realism (Ch. 56, 37.4) — borrow the discipline, reject the simulator

- **Depth-aware order-book queries** (`core/data_type/order_book.pyx`, 483 lines): `simulate_buy`/`simulate_sell` (`:283`/`:297`) walk the book level by level; `c_get_vwap_for_volume` (`:341`) returns an `OrderBookQueryResult` whose `result_volume` is `min(total_volume, volume)` (`:371`) — **insufficient depth is reported, not silently extrapolated**. This is precisely Ch. 56.4's "never assume unlimited liquidity", and the most directly reusable idea in the repository.
- **Paper-trade connector** (`connector/exchange/paper_trade/paper_trade_exchange.pyx`, 1,139 lines): wires a *real* connector's order-book tracker into a fake-balance exchange, so market data is genuine while money is not; limit orders fill when the book crosses their price (`:836`) or an incoming trade matches them (`:895`); insufficient collateral **cancels the order with a warning** (`:641-735`); market orders queue for a fixed `TRADE_EXECUTION_DELAY = 5.0` s (`:609`).
- **Verdict on the backtester**: `strategy_v2/backtesting/` (2,219 lines) is candle-only with one flat `trade_cost = 0.0002` (`backtesting_engine_base.py:209`). Borrow the live order-lifecycle discipline and the depth-query primitive; **do not adopt its backtest execution model and do not cite it as a realism reference.**

### Capital protection vs performance optimization (Ch. 25 mandate)

**Protects capital** (candidates for adoption):

- **Kill switch** (`core/utils/kill_switch.py`, 84 lines): `ActiveKillSwitch` polls profitability every 10 s and shuts the bot down when the profit/loss rate crosses the threshold — in **both** directions. **Critical negative: the default is disabled** — `hummingbot/client/config/client_config_map.py:783-784` sets `default=KillSwitchDisabledMode()`, and `KillSwitchDisabledMode.get_kill_switch` returns a `PassThroughKillSwitch()` (`:253-257`). The enabled mode's `kill_switch_rate` defaults to `Decimal("10")` (`:238-241`). **Our Ch. 59 kill switch must be enabled by default and must not depend on strategy cooperation.**
- **`BudgetChecker`** (`connector/budget_checker.py`, 161 lines): pre-trade collateral check against available balances with explicit locked-collateral accounting and `all_or_none` semantics (resize to what is affordable, or zero). The *shape* maps onto our pre-trade risk gate — **but it is a utility the strategy calls, so a strategy can bypass it**, which our rule 3 forbids; ours must live inside the risk manager.
- **Balance limits as a capital-allocation cap**: `balance_asset_limit` (`client/config/client_config_map.py:813`) enforced by `ConnectorBase.apply_balance_limit` (`connector/connector_base.pyx:361`). It limits how much capital *can be committed*, never how much can be lost.
- **Triple-barrier config** (`strategy_v2/executors/position_executor/data_types.py:17`): `stop_loss`, `take_profit`, `time_limit`, `trailing_stop`, enforced in `position_executor.py` `control_barriers` (`:465`) with fixed precedence stop-loss → trailing → take-profit → time limit. Stop-loss and time-limit protect capital; take-profit and trailing-stop optimize profit with a protective floor.
- **Non-live modes**: the paper-trade connector and `is_trading_required = False` read-only connectors (`exchange_py_base.py:149`).

**Only optimizes expected performance** (research chapters; never capital protection): inventory skew (`strategy/pure_market_making/inventory_skew_calculator.pyx`), order ladders and refresh, hanging orders (`strategy/hanging_orders_tracker.py`, 379 lines), ping-pong, Avellaneda's `risk_factor`/`order_amount_shape_factor` (an inventory-risk *pricing* model), the backtester and its outputs, and all arbitrage profit logic.

**Ignore for now**: the DEX/gateway surface (gas, approvals), all 19 derivative connectors, funding/leverage, and liquidity-mining incentives — outside our spot-only scope. Hummingbot is the reference for connector *structure*, not for integration count.

## F.3 What Hummingbot does **not** provide (verified by search)

- **No dry-run/live toggle** — `grep -rni "dry_run\|dry-run"` over `*.py`/`*.pyx` matches only `hummingbot/model/db_migration/` where it means "do not execute the migration" (`base_transformation.py:48`). The only non-live mode is the whole paper-trade connector.
- **No slippage or execution-cost model in simulation** — `grep -rli "slippage"` matches 44 files, all DEX/derivative connectors, `core/gateway/`, and arbitrage/MM *config* price buffers; the same search restricted to `strategy_v2/backtesting/` and `connector/exchange/paper_trade/` returns **0 files**. Both simulators use a flat fee only.
- **No order-book or depth replay in the backtester** — `grep -rni "order_book\|depth" hummingbot/strategy_v2/backtesting/` → **0 matches**.
- **No latency simulation** — `grep -rni "latency" hummingbot/strategy_v2/` → **0 matches**. (`core/data_type/order_book_tracker.py:26` defines a `LatencyStats` class, but it *measures* live latency rather than injecting it; the only simulated delay is the paper market-order queue constant.)
- **No partial-fill model in the paper simulator** — `grep -rni "partial" .../paper_trade_exchange.pyx` → **0 matches**; an order fills in full or is cancelled. Partial fills *are* handled and tested on the **live** path (`OrderState.PARTIALLY_FILLED`, `in_flight_order.py:26,181`; `test/hummingbot/connector/exchange/binance/test_binance_exchange.py::test_update_order_status_when_order_has_not_changed_and_one_partial_fill`). The simulator and the live tracker are not equivalent.
- **No guarded order state machine** — `update_with_order_update` (`in_flight_order.py:327-341`) assigns state unconditionally.
- **No Binance spot testnet** — see F.2; CCXT (Part D) is the better Ch. 26.2 pointer.
- **No walk-forward, Monte Carlo, purged/embargoed CV, PBO or deflated Sharpe** — `grep -rni "walk_forward|walk-forward|monte_carlo|deflated_sharpe|purged|embargo|combinatorial"` over `*.py`/`*.pyx` returns only two unrelated English uses of "purged" in a KuCoin perpetual connector comment. Chapters 43–49 remain entirely ours.
- **No reliable in-repo architecture document** — the README's "Project Breakdown" lists three directories that do not exist. Recorded so no future reader trusts it.
- **Not verified**: persistence (`hummingbot/model/`, 19 ORM modules driven by `connector/markets_recorder.py`, 638 lines) exists but is coupled to Hummingbot's own models; a full migration study was **not** performed, and nothing was installed or executed.

## F.4 Adoption guidance

- [ ] **License first — Apache 2.0 permits adaptation.** If any code is adapted: retain the license text and notices, state the changes, keep copyright notices (Apache-2.0 §4), and record the pinned commit. The default remains concepts + original implementation + our own tests.
- [ ] **Order lifecycle**: implement a **guarded** transition table from Ch. 56's own specification. Borrow only these concepts: separate client vs exchange order id; trade-id-keyed fill storage for idempotent processing; a multi-tier active/cached/lost registry with a bounded not-found counter; tracking started *before* submission; and reconstruction from persisted tracking state on restart.
- [ ] **Adapter shape**: adopt the thin-subclass-over-a-fat-base split, keeping our `ExchangeAdapter` as the boundary so no vendored type reaches the central domain (Ch. 26).
- [ ] **Exchange rules (Ch. 30)**: extend our rule model toward `TradingRule`'s field set (min/max size, base and quote increments, min notional, min order value, significant digits, supported order types) — more complete than CCXT's `limits` + `precision`, implemented natively as a *model*.
- [ ] **Adapter test contract (Ch. 14/26)**: build one abstract, mocked-exchange test suite every adapter must pass, modelled on `connector/test_support/exchange_connector_test.py` — including the not-found/lost-order and partial-fill scenarios it covers. This is the strongest idea in the repository for *verifying* a connector rather than assuming it works.
- [ ] **Ch. 56.4 liquidity**: implement an order-book query returning `(price, filled_volume)` that **reports** insufficient depth (`min(total_volume, volume)` shape) and warns when a simulated order exceeds plausible depth — our own domain implementation.
- [ ] **Ch. 26/29 data integrity**: adopt the snapshot-vs-diff discipline (reject diffs not newer than the snapshot, buffer pre-snapshot diffs, rebuild by replaying later diffs) and gap detection that fails closed.
- [ ] **Do not adopt the backtester's execution model** — candle-only, one flat cost, no depth, no latency, no partial fills. Hummingbot is useful mainly as corroboration that these are easy to omit and hard to notice.
- [ ] **Kill switch (Ch. 59)**: borrow the mechanism, **invert the default** — Hummingbot ships it disabled; ours must be enabled by default and independent of strategy cooperation.
- [ ] **Pre-trade risk gate (Ch. 58)**: adopt `BudgetChecker`'s shape (resize-or-reject with locked-collateral accounting) **inside** the risk manager, so no strategy can bypass it (rule 3).
- [ ] **Reject as capital protection**: inventory skew, level ladders, refresh/hanging-order logic, ping-pong, Avellaneda risk-factor pricing, and all arbitrage profit logic — they optimize expected return and belong to Ch. 39/44–45.
- [ ] **Pin `2bfaccc48` (v2.16.0, 2026-07-30)** and re-verify on any submodule update. Given that Hummingbot's own README is already stale, never rely on its documentation of the layout — re-list the tree.

---

# Part G — Jesse (`external/jesse`)

## G.1 The reference at a glance

| Property | Value |
|---|---|
| Path / upstream | `external/jesse` · https://github.com/jesse-ai/jesse |
| License | **MIT** (`LICENSE`, 21 lines, "Copyright (c) 2020 Jesse.Trade") — use, modification, distribution and **commercial** use permitted, conditioned only on preserving the notice. **Code adaptation is permitted** with attribution. Two non-license couplings: the live/paper runtime is a **separate package** (`jesse_live`, imported only at `controllers/live_controller.py:34`) whose license was **not** inspected; and the MCP research tools are metered against the commercial `jesse.trade` backend (`mcp/USAGE_LIMITS.md`). |
| Version | 3.1.4 (`version.py`), studied commit `60f882e8` (2026-09-12). |
| Architecture | `jesse/` — 602 `.py` files / 60,572 lines excluding vendored SDK code. `modes/` is the engine (`backtest_mode.py` 2,307 lines, a functional pipeline rather than a class hierarchy; `optimize_mode/` 939, `monte_carlo_mode/` 747, `significance_test_mode/` 302, `import_candles_mode/` with 11 drivers); `research/` (16 files) is a scriptable API; `strategies/Strategy.py` (1,874); `indicators/` (175 files); `services/` (48 files); `mcp/` (9,926 lines, an MCP server) and `controllers/` + `routes/` (FastAPI + Peewee/PostgreSQL + Redis). **Weight warning**: of the 89 MB under `jesse/`, 27 MB is a prebuilt Nuxt dashboard bundle and 58 MB is vendored native code (48 MB under `drivers/Lighter/signer_libs/`, 9.6 MB under `drivers/Apex/omni_files/`). |
| Tests | 61 files / 22,318 lines / 716 top-level test functions, plus 156 strategy packages used as regression fixtures. Dedicated research tests exist: `test_rule_significance_testing.py` (324 lines), `test_research_monte_carlo.py` (275), `test_simulator_parity.py` (79). **Not run** — see G.3. |

## G.2 What maps to which roadmap chapter

### Backtest engine and intrabar execution (Ch. 37.4, 37.5) — mechanics worth borrowing

- **Two simulators in one file**: `_step_simulator` (`modes/backtest_mode.py:1042`, the dense 1-minute loop) and `_timestamp_simulator` (`:900`, an event-plan replay for sparse observation data), both reaching the same price-effect code.
- **Intrabar fill ordering is simulated, not assumed away**: `_simulate_price_change_effect` (`:1621`) takes the 1 m candle, **splits it at each resting order's price** (`candle_service.split_candle`, `services/candle_service.py:125`), executes, re-splits the remainder in a loop (`:1660-1688`), then restores the real candle (`:1690-1699`); `_sort_execution_orders` (`:2265`) orders competing fills. This is the closest thing among our references to Ch. 37.5 — but the assumed path is **implicit**; ours must state path, tie-break rule and gap policy explicitly.
- **Gap-through handling**: `_order_is_crossed_by_opening_gap` / `_execute_opening_gap_orders` / `_get_opening_gap_and_executing_orders` (`:1447`, `:1517`, `:1569`) fill orders crossed by an opening gap at the exposed open. Direct input for 37.4.
- **No-look-ahead mechanism, verified in code**: higher-timeframe candles are aggregated and published only when the bucket completes (`_simulate_new_candles`, `:1970`; boundary test `:2016`), and strategies receive closed candles only (`Strategy.get_candles`, `strategies/Strategy.py:1521`). Source-level confirmation; not reproduced by execution.
- **Costs are taker-only**: one `fee_rate` per exchange (`models/SpotExchange.py:11`, `models/FuturesExchange.py:85-93`); maker/taker appears only in comments. Futures add margin rejection (`models/FuturesExchange.py:112-119`) and isolated-mode liquidation at the bankruptcy price (`modes/backtest_mode.py:1714-1761`).
- **The order API is a "smart order" DSL**: `Market`/`Limit`/`Stop` are *inferred* from order price vs current price; there is no order-type parameter (`mcp/resources/strategy.md:129-152`). Useful contrast for our explicit Ch. 37.3 order model — Jesse trades explicitness for ergonomics, and we should not.

### Optimization (Ch. 39) — real, but materially weaker than advertised

- **`modes/optimize_mode/Optimize.py` (639 lines) + `fitness.py` (131) + `research/optimize/__init__.py` (453)** implement a working parallel search with Ray workers, resumable trials and early rejection below 5 training trades (`fitness.py:98-99`).
- **Optuna is a trial *store*, not the sampler — verified.** `optuna.create_study(..., storage='sqlite:///./storage/temp/optuna/optuna_study.db')` (`Optimize.py:150-155`) persists trials and each finished trial is written with `optuna.create_trial(...)` + `study.add_trial` (`:267-320`), but **parameters are drawn with `np.random.randint/uniform/choice`** (`_generate_trial_params`, `:232-265`), and the research mirror repeats it with the comment *"The study is created for completeness/future use; random sampling is done directly via _generate_trial_params"* (`research/optimize/__init__.py:223-224`). `suggest_int|suggest_float|suggest_categorical|study.optimize|TPESampler|RandomSampler|enqueue_trial` across `jesse/` and `tests/` → **no matches**. **Any description of Jesse as "Optuna-driven optimization" is wrong**; write "random search with Optuna used for persistence".
- **Selection is in-sample**: `get_fitness` scores on the training window only (`fitness.py:91`); testing-window metrics are computed and displayed (`Optimize.py:370-378`) but ranking uses the training `score` alone (`:364-401`). The "out-of-sample" column is a report, not a selection filter.
- **No cross-validation**: `_get_training_and_testing_candles` (`modes/optimize_mode/__init__.py:153-169`) loads exactly two fixed windows. The README's "easy cross-validation" is not implemented.
- **Not reproducible**: no seed is set or recorded anywhere in the sampling path (`grep -rn "seed" research/optimize/__init__.py modes/optimize_mode/*.py` → no matches). Objectives: sharpe, calmar, sortino, omega, serenity, smart sharpe, smart sortino (`fitness.py:51-80`) — all historical ratios, no risk-of-ruin or robustness objective.

### Rule significance testing (Ch. 43.2) — the strongest statistical artifact in any of our references

- **`research/rule_significance_testing/rule_significance.py` (241 lines)** is a two-phase test: phase 1 runs a **signal-only backtest** (`simulator.py`, 183 lines — reuses the real replay plan and candle publication while skipping all order submission), phase 2 resamples the rule's returns to build a null distribution. **This is the one mechanism worth adapting into our own code.**
- **It gets the alignment right**: the signal emitted at bar *t* is scored against bar *t+1*'s log return, "the first return that was not already known at signal time" (`:168-171`), and returns are **detrended by the mean** so a no-edge rule has expectation zero regardless of the period's drift (`:183-189`).
- **The null generator is a stationary bootstrap** (`bootstrap.py`, 84 lines): geometrically distributed block lengths with `mean_block_length=10` (`:16-22`, `:50`), restarts at uniformly sampled indices with wrap-around to keep the resample stationary (`:55-66`), and **a single seeded RNG kept across batches so changing the batch count cannot change a seeded experiment** (`:37-39`). p-value = fraction of simulated means ≥ observed (`rule_significance.py:224`); the report carries `n_observations`, `p_value` and `bootstrap_mean_block_length` (`:226-237`).
- **Explicit assumption guards**: `MIN_OBSERVATIONS = 30` with a "results may be unreliable" warning (`common.py:17`, `rule_significance.py:173-177`), plus warnings for NaN bars (`:146`) and dropped NaN returns (`:166`). Default seed 42 in the runner (`modes/significance_test_mode/SignificanceTestRunner.py:42`).
- **Documented-vs-implemented discrepancy**: the README and `mcp/tools/significance_test.py:53-54,72` describe it as *"runs n_simulations random-entry variants on the same candles"*. `grep -rni "random.entry|random_entry" jesse/` finds **only those two documentation strings** — no random-entry strategy is ever constructed. The implemented method (bootstrap of zero-centred own-returns) is defensible, arguably better, but the description is not literal and **we must not repeat it**.
- **Missing**: effect size, confidence interval and multiple-testing context in the returned dict; the p-value is one-sided by design. Those are our additions for 43.2.

### Monte Carlo (Ch. 44.4) — two real modes, no honesty scaffolding

- **Trade-order shuffle**: `research/monte_carlo/monte_carlo_trades.py` (609 lines) copies the completed-trade list, `random.shuffle`s it (`:99-131`), and rebuilds equity by adding per-trade PnL at re-spaced timestamps (`:292-317`), recomputing metrics (`:320-354`). This *is* trade-reshuffling, and it destroys temporal structure by construction — exactly the mode 44.4 permits only **with a warning**.
- **Candles-based**: `research/monte_carlo/monte_carlo_candles.py` (497 lines) plus `candle_pipelines/moving_block_bootstrap.py` (91 lines) resamples moving blocks of (Δclose, Δhigh, Δlow) and rebuilds OHLC, with the block size a **heuristic, not a parameter** — `max(10, batch_size // 10)` (`:24-29`); `gaussian_noise.py` (63 lines) adds Gaussian noise. Honest reading: this re-samples the **same historical series**, so "robustness under different market conditions" is bounded by the sample; it does not synthesize new regimes.
- **Seed handling**: `BASE_RANDOM_SEED = 42` (`research/monte_carlo/common.py:16`) is hard-coded and neither user-settable (`:134-153`) nor returned (`:194-212`).
- **Honesty gap**: `_calculate_confidence_intervals` (`:395-466`) stamps `is_significant_5pct`/`is_significant_1pct` and `_generate_interpretation` (`:480-512`) prints "significant (p < 0.05)" and *"X out of N metrics being statistically significant"* — with **no correction for comparing 4–7 metrics**, and a search for `warning|caveat|assumption|not a prediction` in the Monte-Carlo package returns only an import line. **Our 44.4 warnings (temporal structure destroyed, MC is not a forecast, scenario definition explicit) have no counterpart here; we must add them, not inherit them.**

### Machine-learning pipeline (Ch. 47–50) — real plumbing, no AFML machinery

- **`research/ml.py` (1,142 lines) is the whole pipeline**, and all three stages are genuinely implemented: **gather** runs a real backtest and harvests `strategy._ml_data_points` (`:52-138`; strategy side is `record_features`/`record_label`, `strategies/Strategy.py:122-163`), with labels **entirely user-defined** — the framework never computes an outcome; **train** sorts by time (`:212`), makes a **chronological** split with `test_ratio=0.2` (`:239-248`) and fits `StandardScaler` on the training part only (`:259-262`); **deploy** lazily loads artifacts and predicts inside the strategy (`:299-350`).
- **Feature importance is time-series-aware but incomplete**: `_compute_feature_importance` (`:690-759`) averages RFE ranking, ANOVA F-value, |Pearson| and CV-impact into consensus ranks, with `TimeSeriesSplit(n_splits=5)` (`:707`). Caveats verified: the scaler was already fit on the **whole** training split before this CV (`:261`), and there is no purging, no embargo, no clustered importance (49.7 stays ours).
- **Persistence is three pickles and nothing else**: `joblib.dump` of `model.pkl`, `scaler.pkl`, `feature_importance.pkl` (`:360-364`), with **no metadata sidecar** — no dataset identity, label definition, feature-list version, hyperparameters, git revision or library versions. Ch. 48.5 is entirely ours. **Security caution**: `joblib.load` deserializes pickles, i.e. arbitrary code execution from an untrusted model file — it conflicts with our rule 3 posture and must be replaced by a documented safe format plus a manifest.
- **"Gather mode"/"deploy mode" is a naming convention, not a state machine**: `self.ml_mode = getattr(type(self), 'ml_mode', 'gather')` (`Strategy.py:96`) is the **only** occurrence of `ml_mode` in the framework and nothing reads it.
- **No triple-barrier, meta-labelling, sample weighting or uniqueness machinery**: the only mention of "triple-barrier" in the whole package is a docstring example of what a *user* could supply (`ml.py:174`); `fractional|frac_diff|shannon|entropy|cusum` → no matches. 49.1–49.3 and 49.8 have no counterpart.

### Metrics, indicators and the research API (Ch. 40–42, 31, 52)

- **`services/metrics.py` (508 lines)** provides sharpe/smart sharpe (`:71`), sortino/smart sortino (`:90`), an autocorrelation penalty (`:114`), calmar (`:124`), max drawdown (`:160`), max underwater period (`:171`), CAGR (`:208`), omega (`:237`), serenity (`:258`), ulcer (`:270`), CVaR (`:287`) and a 40-field trade report (`:309-487`) — a good checklist for 40.1–40.4. Absent: probabilistic Sharpe, deflated Sharpe, PBO (49.5/49.6 stay ours).
- **174 exported indicators** in `indicators/__init__.py` (174 import lines) over 175 modules. The README claims "300+ indicators"; the tree does **not** contain 300 — a verified overstatement, and a reminder that our Ch. 31 catalog is sized from our own requirements. 93 of the 175 modules import from the separate `jesse_rust` wheel (`requirements.txt`), which is **not in this tree**.
- **`research/__init__.py` is a clean "everything callable from a script" surface** worth mirroring for Ch. 52 — but it is **not a standalone library**: several entry points raise `FileNotFoundError('Invalid directory: ".env" file not found…')` unless run from a Jesse project root (`research/candles.py:17-24`), and the runtime pulls PostgreSQL, Redis and FastAPI.
- **`modes/import_candles_mode/drivers/`** holds 11 exchange adapters with **per-exchange quirk notes in comments** — Kraken's "rolling window; older data cannot be retrieved regardless of `since`" (`Kraken/KrakenSpotMain.py:16`), Lighter's ~130-day window (`Lighter/LighterMain.py:14`). A ready-made checklist for Ch. 26; verify each against live docs, since these are comments and not tests.

### Capital protection vs performance optimization (Ch. 25 mandate)

**Genuinely protects capital** (formulas eligible for adoption):

- **Risk-budget sizing**: `utils.risk_to_qty(capital, risk_per_capital, entry, stop, precision, fee_rate)` (`utils.py:148-167`) over `risk_to_size`, which hard-caps size at `min(temp_size, capital_size)` (`:170-186`) and raises on zero risk distance (`:181-182`). A position can never be allocated more than the capital.
- **Per-trade risk ceiling**: `utils.limit_stop_loss(entry, stop, trade_type, max_allowed_risk_percentage)` clamps the stop distance so the loss cannot exceed a configured percentage of entry (`:106-120`) — a small, directly adoptable primitive.
- **Rejecting input guards**: `Broker._validate_qty` raises on zero quantity (`services/broker.py:24-26`); `estimate_risk`/`size_to_qty` raise `TypeError` on NaN rather than sizing silently (`utils.py:100-103`, `:203-204`).
- **One simulator serves backtest, paper and live**: `services/order_service.py:39` routes non-live modes to the same simulated path, `helpers.py:488` recognises `trading_mode == 'papertrade'`, and the real loop is gated behind `has_live_trade_plugin()` (`helpers.py:1294-1303`). Adopt the *concept*; do not expect this repo to give you a runnable dry-run mode — the loop lives in the external `jesse_live` package.

**Only optimizes historical performance**: the random hyperparameter search over a single fixed split, every objective function (all historical ratios), and the Monte-Carlo percentile ranks with their "significant (p < 0.05)" verdicts.

**The gap this leaves for us**: `grep -rni "kill.switch|max_drawdown_limit|daily_loss|circuit.break" jesse/` matches only a Monte-Carlo metric constant (`research/monte_carlo/common.py:20`). **There is no kill switch, daily-loss limit, max-drawdown stop, portfolio exposure cap, or risk manager a strategy cannot bypass.** Capital protection in Jesse is a set of **opt-in helpers the strategy author must call correctly**, and a Jesse strategy can size arbitrarily. We may adapt the formulas, but they must sit **behind our enforcement boundary**, not be offered as optional utilities.

## G.3 What Jesse does **not** provide (verified by search)

- **No purged/embargoed CV, no CPCV, no PBO/CSCV, no deflated or probabilistic Sharpe** — `grep -rni "purged|embargo|deflated|probability of backtest|combinatorial|CPCV|false strategy|probabilistic sharpe" jesse/ --include=*.py` matched only session-deletion HTTP routes ("Successfully purged N sessions"). Ch. 44.8, 49.4, 49.5, 49.6 remain ours.
- **No walk-forward** — `grep -rni "walk.forward|walkforward|out.of.sample"` matched only MCP tool *description* strings and unrelated driver comments; the optimization windows are two fixed ranges loaded once. Ch. 45 stays ours.
- **No cross-validation inside optimization** — `cross_val|kfold|TimeSeriesSplit` matched only `research/ml.py` feature-importance CV. The README's "easy cross-validation" is not implemented.
- **No Optuna-driven sampling** — the `suggest_*`/`study.optimize`/sampler searches return **no matches** (see G.2).
- **No optimization seed** — `grep -rn "seed" research/optimize/__init__.py modes/optimize_mode/*.py` → **no matches**.
- **No slippage, spread, market-impact or volume-limited fill model** — `grep -rni "slippage" jesse/ --include=*.py` → **no matches**; cost is the single per-exchange taker `fee_rate`, with no maker/taker distinction.
- **No Monte-Carlo seed field** — the only MC seed is the hard-coded `BASE_RANDOM_SEED = 42`; `seed` in the MC model, controller and MCP tool → **no matches**.
- **No risk warnings in the Monte-Carlo research code** — the search matched a single import line. No "shuffling destroys temporal structure", no "not a forecast", no multiple-testing note.
- **No AFML labeling or weighting machinery** — one docstring line only (see G.2).
- **No kill switch / risk manager / global exposure limit** — only the MC constant.
- **No live or paper-trading engine in this repository** — the execution runtime is the separate `jesse_live` package (`controllers/live_controller.py:34`); its feature set is **not verified here** and must not be attributed to the MIT repo.
- **No model/experiment metadata** — only three `joblib.dump` calls; no manifest, sidecar, dataset hash or hyperparameter record.
- **Not verified by execution**: nothing was installed or run. `python3 -c "import jesse"` fails with `ModuleNotFoundError: No module named 'arrow'` (`helpers.py:14`), the first of several missing dependencies (`peewee`, `redis`, `jesse-rust`, `scikit-learn`, `ray`, `mcp`…), and `jesse-rust==1.2.0` is PyPI-only. Per rule 2 nothing was installed. **Therefore no test suite was run and no experiment was executed: every behavioural statement above is read from source**, including the no-look-ahead, partial-fill, no-slippage and non-reproducibility conclusions. The 174-indicator count and all line/file/test counts come from the checkout at the pinned commit.
- **Not verified**: whether the bundled dashboard matches the Python API at this commit; the licence and behaviour of `jesse_live`, `jesse-rust` and `dashboard-v1`; and whether the commercial `jesse.trade` services are required beyond MCP metering and exception reporting.

## G.4 Adoption guidance

- [ ] **License first — MIT, and this one is adaptable.** Preserve the copyright and permission notice in any derived work, including commercial. Two boundaries that are *not* license terms but are real: `jesse_live` is a separate package with an **unverified license** — treat its code as unadaptable until read; and the MCP tools depend on the commercial `jesse.trade` plan backend, so copy no license-gating logic expecting it to work self-hosted.
- [ ] **Adopt the significance-test design for Ch. 43.2 — the single most valuable thing in Jesse.** Reimplement, with attribution: the signal-only replay that never submits orders, next-bar return alignment, mean-detrending so a no-edge rule has zero expectation, a **stationary** bootstrap with a documented geometric block scheme, a seed that is **recorded** and stable across batch-count changes, and explicit minimum-sample and NaN warnings. Then go beyond it: return effect size and a confidence interval, and add multiple-testing context — Jesse returns neither.
- [ ] **Copy the intrabar fill mechanism conceptually into 37.5, and make it explicit**: the split-at-order-price loop with re-sorting of competing orders plus the opening-gap path is stronger than an implicit close-only assumption. State the assumed path, the tie-break rule and the gap policy in the 37.5 documentation where Jesse leaves them implicit.
- [ ] **Adopt the capital-protection primitives, but enforce them.** Port the arithmetic of `risk_to_qty`/`risk_to_size` including the `min(size, capital)` cap and `limit_stop_loss`'s risk ceiling into Ch. 58, **behind our risk manager**, so a strategy cannot bypass them. Jesse proves the formulas are convenient; it also proves that leaving them as opt-in helpers protects nothing.
- [ ] **Borrow the exchange-driver quirk checklist for Ch. 26**: walk the 11 drivers for documented provider limits and convert each into an explicit capability note in our adapter interface. Verify every claim against current exchange documentation — these are comments, not tests.
- [ ] **Adopt the "one simulator serves backtest, paper and live" concept (Ch. 55, 57)** so our dry-run mode cannot drift from the backtest engine.
- [ ] **Do not adopt, and say so in our docs**: the Optuna-as-optimizer framing (it is a trial store over `np.random` search); the single fixed split presented as cross-validation; the unseeded, non-reproducible optimizer; Monte-Carlo "significant (p < 0.05)" verdicts and the unadjusted "X out of N metrics" summary (44.4 requires the opposite); `joblib`/pickle model artifacts without a manifest (48.5 and rule 3); the `ml_mode` attribute treated as an enforced state machine; the README's "300+ indicators" (174 exported modules) and its "random-entry variants" description (no random-entry simulation exists).
- [ ] **Dependency reality check (Ch. 4) — reject as a runtime dependency.** Jesse requires FastAPI + uvicorn, Peewee + `psycopg2-binary` (PostgreSQL), Redis, `ray`, `optuna`, `scikit-learn`, `matplotlib`, `mcp`, `statsmodels`, plus the PyPI-only binary `jesse-rust==1.2.0`; its research API refuses to run outside a project root with a `.env`. Study and adapt individual algorithms; do not vendor the framework.
- [ ] **Pin `60f882e8` (v3.1.4, 2026-09-12)** and re-run the G.3 per-feature checks on any submodule update — the optimizer, Monte-Carlo and ML modules are all actively changing, and the documentation has already drifted from the implementation in at least three places.

---

# Part H — TA-Lib (`external/ta-lib`)

## H.1 The reference at a glance

| Property | Value |
|---|---|
| Path / upstream | `external/ta-lib` · https://github.com/TA-Lib/ta-lib |
| License | **BSD 3-Clause** (`LICENSE`, © 1999-2026 Mario Fortier). **Code adaptation is permitted** with the notice preserved — the permissive family of the MIT AFML exercises, unlike Backtrader's GPLv3. |
| Version | `VERSION` 0.8.1; studied commit `8b94ee7ff` (2026-09-12), `git describe` = `v0.8.1-2-g8b94ee7ff`. Actively developed again after a long hibernation. |
| Architecture | One generated corpus, many backends. `ta_codegen/input/<name>/` holds one directory per function (`atr.c`, `atr.md`, `atr.yaml`); a Rust generator renders the C library in place into `src/ta_func/` and the public headers into `include/`, plus the Rust/Java/C# sources under `ta_codegen/output/` (most of the 96 MB tree). **Not built in this checkout**: no CMake cache, no `*.so`/`*.a`/`*.o`, `bin/` and `lib/` hold only a `HOLDER` placeholder, `git status` clean. |
| Tests | `src/tools/ta_regtest/` — 66 per-function C test files, `test_abstract.c` (3,779 lines) for the catalog interface, `test_codegen.c` (10,314 lines) comparing each language binding against the C reference, a frozen golden reference, and built-in price data with no network dependency. |

## H.2 What maps to which roadmap chapter

### Indicator catalog and naming (Ch. 31) — the roadmap's explicit pointer

- **A machine-readable catalog exists, in three forms.** `ta_func_list.txt` (201 lines: abbreviation + English name), `ta_func_api.xml` (9,106 lines, 201 `<FinancialFunction>` entries with `GroupId`, flags, required inputs, optional inputs with type/range/default/`SuggestedStart-End-Increment`, and outputs), and a runtime enumeration path via `TA_FunctionDescriptionXML()` and `TA_ForEachFunc` + `TA_GetFuncInfo` (`include/ta_abstract.h`). Both files are *generated* from `ta_codegen/input/`, so catalog and documentation cannot drift.
- **Groups are the taxonomy**: exactly ten `GroupId` values summing to 201 — Pattern Recognition 61, Momentum 47, Overlap Studies 25, Math Transform 15, Volume 12, Math Operators 12, Statistic 11, Volatility 7, Price Transform 6, Cycle 5. **A third of the catalog (61) is candlestick patterns, which Ch. 31 does not ask for.**
- **Naming conventions worth adopting**: abbreviation-first (`SMA`, `ATR`, `ROC`; 57 names are three characters or shorter), prefix families (`CDL` for the 61 candlestick recognisers, `HT_` for the 6 Hilbert cycle functions), and abbreviations treated as **stable identifiers rather than labels** — `scripts/serve_version.py:122` parses `ta_func_list.txt` across two trees specifically to detect renames, and 190 of the 201 canonical docs carry an `## Aliases` section instead of renaming. That is a directly usable rule for Ch. 31.
- **Parameter ranges are data, not prose**: `ta_codegen/input/atr/atr.yaml` carries `range: [1, 100000]`, `default: 14`, `suggested: [1, 200, 1]` — the shape Ch. 31's "common parameter values" needs, and the `suggested` triple is explicitly a parameter-optimization input (Ch. 39).
- **Warm-up is computed, not documented**: `TA_GetLookback` (`include/ta_abstract.h:594`) returns the lookback for a *configured* call, so the leading undefined bars follow from the parameters instead of a written constant that drifts when a parameter changes.
- **A reusable documentation template**: the 201 canonical `ta_codegen/input/<name>/<name>.md` files share one shape — Summary / Formula / Inputs / Outputs / Parameters / Implementation / Aliases / See Also / References. Our per-indicator docs can adopt it.
- **Checked against Ch. 31's required list**: SMA, EMA, RSI, MACD, BBANDS, ATR and ROC are present under exactly those abbreviations, and `TRANGE` supplies true range. **Absent**: a drawdown indicator (0 hits for "drawdown"), a historical-volatility indicator (only the unrelated `CVI`/`RVI` mention volatility; `STDDEV` is the building block), and an average-volume indicator (no `VOLUME` entry; `AD`/`OBV`/`ADOSC`/`MFI` consume volume but none averages it). Rolling highs and lows exist only as the generic `MAX`/`MIN`. **Those three open Ch. 31 indicators cannot be sourced from this catalog and stay our own work.**
- **Gap versus Ch. 31**: the canonical docs have no "typical misuse" or "limitations" sections and no not-a-guarantee warning (grep over `ta_codegen/input/**/*.md`: 0 files). Those three Ch. 31 items stay ours.

### Warm-up, stability and missing values (Ch. 31) — sharper than our current spec

- **A four-way stability taxonomy** (`website/src/functions/stability.md`): every function is start-independent (bounded window), initial-unstable-period (recursive but convergent), MA-type-dependent (the `optInMAType` choice decides which applies), or **path-dependent** — a running accumulation whose value depends on the start date and *never* converges, so there is no warm-up to discard at all (`AD`, `SAR`). This is a better answer to Ch. 31's "describe its warm-up period" than a written constant: it separates "drop N bars" from "this number is an artefact of where your data begins".
- **Warm-up is enforced in the API**: `TA_SetUnstablePeriod` / `TA_GetUnstablePeriod` (`include/ta_func.h`) strip extra leading bars per function or globally; 23 of the 201 functions carry the `Unstable Period` flag (counted in `ta_func_api.xml`). `website/src/api/unstable-period/README.md` states the hazard plainly: on a short series, the earliest bars silently use bad values.
- **Missing values are declared per function**: only 8 of the 201 carry a `Can Output NaN or +/-Inf` flag, and each function documents its output domain. Ch. 31's "handle missing values" is served by copying that idea — declare per indicator whether the warm-up region is `NaN`-padded or simply not emitted.

### Incremental updates (Ch. 31) — an open checkbox with a verified design to copy

- **All 201 functions carry the `Streaming` flag, and the C headers implement it**: `include/ta_func.h` declares 201 `TA_<N>_Stream` types with `TA_<N>_Open` / `_Update` / `_Peek` / `_Close` / `_OpenAndFill` (verified at `include/ta_func.h:55,130-147` for `TA_AC`). The contract is written down in `docs/streaming-api-design.md` (429 lines): open consumes history once and requires `historyLen >= lookback + 1`; `update` runs once per *closed* bar at a cost independent of the period; `peek` evaluates a still-forming bar without committing; the handle is opaque, **not serializable, and never crossed between processes** — the sanctioned checkpoint is re-opening from retained history.
- **For us**: this is the reference answer to Ch. 31's unchecked "support incremental updates where practical". Copy the shape, and add our own look-ahead rule — a `peek`-style value on a forming bar may drive a chart or a diagnostic but must never enter a signal or a recorded decision, and each indicator must state which bar a value belongs to.

### Determinism and threading (Ch. 12, 29, 43) — what must **not** be copied

- The TA functions are multi-thread safe, but the **global settings** are not: `TA_SetUnstablePeriod` and `TA_SetCandleSettings` mutate process-wide tables (`src/ta_common/ta_global.c`), and the initialisation comment notes that a library started with unset candle settings "silently changes every `CDL*` result". Two runs in one process with different warm-up settings can return different values for identical input.
- **Implication**: if any of this design is imitated, warm-up length and pattern thresholds must be **per-call or per-run parameters, never process globals**. Our determinism (Ch. 29) and evidence rules (Ch. 43) cannot tolerate a value that depends on what else ran earlier in the process. Worth copying instead: `TA_SetCandleSettings` returns `TA_BAD_PARAM` for an out-of-domain period rather than computing with it — the rejecting parameter-validation posture Ch. 31 wants.

### The Python wrapper and its packaging (Ch. 4, 25) — the dependency question

- **`external/ta-lib` is the C core, and the Python binding is a different repository.** `README.md` opens with "This is the official home for TA-Lib Core"; the binding most people mean is `ta-lib-python`, listed as a community wrapper in `website/src/.vuepress/includes/wrappers-table.md` and reached by this repo's release tooling only as a local sibling checkout (`scripts/python-dev.py` defaults to `~/ta-lib-python`).
- **Verified absent here**: no `setup.py`, `pyproject.toml`, `*.pyx`, `*.pxd` or `MANIFEST.in` outside the generated output tree; no `talib/` package. The 43 `.py` files are build/CI utilities. **Nothing in this checkout implements or verifies the Python wrapper** — any claim about `talib.SMA(...)` or pandas integration would have to come from `ta-lib-python`, which is not vendored.
- **The wrapper's documented install is a PyPI source build** against an already-installed C library (`scripts/install_tests/python.py` runs `pip install --no-binary TA-Lib TA-Lib` after the C package is in place). Not a wheel.
- **Debian packaging here covers the C library only** (`ta-lib.dpkg.in`, `.deb` digests under `dist/digests/`, install instructions at `website/src/install/c/README.md:112`). There is no packaging for the Python wrapper.
- **Verified absence in apt**: on this Debian 13 machine `apt-cache policy python3-talib`, `libta-lib0` and `ta-lib` return nothing, while the control `apt-cache policy python3-numpy` resolves normally — so the empty result reflects a real absence in the configured sources, not a broken query. *Scope: this machine's configured apt sources only; PyPI and Debian's web trackers were not queried, and nothing was installed or built.*

### Capital protection vs performance optimization (Ch. 25) — the mandatory distinction

- **TA-Lib protects no capital and cannot.** There is no position sizing, risk limit, stop, kill switch, portfolio, account or order model anywhere in the tree (see H.3). Adopting anything from it adds *measurement*, never *protection*.
- The only capital-relevant mechanisms are indirect and protect against **bad numbers rather than bad exposure**: a computed lookback plus the unstable-period machinery keeps undefined warm-up values out of downstream logic; the stability taxonomy flags values that are artefacts of the start date; parameter validation rejects out-of-domain periods instead of computing with them.
- **Everything else optimizes historical performance**: the 201-function catalog, the machine-readable `suggested` ranges (which the XML describes as optimization inputs), and the streaming API (a latency concern) are Ch. 39 and Ch. 44–45 material.
- **Adoption implication**: an indicator library must never be enrolled as a capital-protection mechanism, and enlarging the Ch. 31 catalog must never be presented as making the application safer. A wrong indicator value is a correctness risk; a missing indicator is only a feature gap.

## H.3 What TA-Lib does **not** provide (verified by search)

- **No Python wrapper source** — `find . -name "*.pyx" -o -name "setup.py" -o -name "pyproject.toml" -o -name "*.pxd" -o -name "MANIFEST.in"` (excluding `ta_codegen/output/`): no hits. `grep -rlI "_ta_lib" .` matches only `scripts/README.md` and `scripts/python-dev.py`, both describing the external checkout.
- **No risk, sizing or protection concepts** — `grep -rniI "position siz|stop.loss|portfolio|risk manage|kelly"` over `*.md *.c *.h *.yaml *.rs *.py`: **zero hits for every term**.
- **No backtesting or performance analytics** — "backtest" appears once in the whole corpus (`website/src/functions/stability.md:48`) and only as a warning that a different start date gives different numbers; `drawdown` 0 hits; there is no equity curve or metric catalog (Ch. 40–42).
- **No signals, strategy or execution layer** — functions return indicator values or candlestick classifications (e.g. `CDLDOJI` emits 100/0), never entry/exit rules; `order book` 0 hits (Ch. 30, 37, 55).
- **No drawdown, historical-volatility or average-volume indicator** — grepping `ta_func_list.txt`: `drawdown` 0, `volatility` 2 (the unrelated `CVI`/`RVI`), `average volume` 0, `^VOLUME` 0.
- **No CSV/JSON indicator metadata** — the only `.csv` outside the generated tree is `src/ta_common/ta_retcode.csv` (19 lines: the return-code table, an *input* to the generated error enum). Catalog metadata is TXT/XML only.
- **No caution content for Ch. 31** — `grep -rli "misuse|common mistake|limitation" ta_codegen/input --include=*.md`: 0 files.

## H.4 Adoption guidance

- [ ] **License**: BSD 3-Clause permits studying **and adapting** code, in source and binary form, provided the copyright notice and conditions are retained and the holder's name is not used to endorse derived work. Preserve the notice and attribute TA-Lib in any derived file — this is the permissive family, unlike Backtrader's GPLv3.
- [ ] **Dependency STOP (AGENTS rule 2) — do not treat TA-Lib as installable here.** No `python3-talib`, `libta-lib` or `ta-lib` package exists in this machine's Debian 13 sources; the repo ships `.deb` files for the C core only; the wrapper is a PyPI source build needing the C library and headers first. Nothing was installed or built. Working default: **study only, no dependency**; if a runtime dependency is ever wanted, hand the maintainer a Ch. 4 decision record.
- [ ] **Mine the catalog, do not import it.** Use `ta_func_list.txt` and `ta_func_api.xml` as the naming reference for Ch. 31 — abbreviation-first names, prefix families, groups as the taxonomy, aliases instead of renames, one generated source of truth for docs and dispatch — and adopt the `<name>.md` section shape. Never claim a TA-Lib indicator is "available" in our application without a Ch. 31 implementation and tests.
- [ ] **Respect Ch. 31's own ceiling**: it says "do not add dozens of indicators without tests" and only three indicators are missing. TA-Lib's 201 functions are a naming and metadata reference, **not a backlog**; 61 of them (candlestick patterns) are out of scope entirely.
- [ ] **Make warm-up computed and classified**: derive the lookback from the parameters and classify each of our indicators as start-independent, warm-up-dependent or path-dependent. Add the three items TA-Lib cannot supply: typical misuse, limitations, and the explicit not-a-guarantee warning.
- [ ] **Incremental design with a hard look-ahead boundary**: mirror the streaming shape (warm from history, O(1) per closed bar, explicit provisional evaluation). A provisional value must never reach a signal, a metric or a recorded decision, and each indicator must state which bar a value belongs to.
- [ ] **Refuse the process-global state.** Warm-up length, candle thresholds and every other setting must be per-run parameters, not process globals — otherwise two runs in one process can disagree on identical input (Ch. 29, Ch. 43). Keep parameter validation strict and rejecting rather than clamping silently.
- [ ] **Never book indicators as capital protection.** All protection stays in the risk manager and the kill switch (Ch. 58–59).
- [ ] **Pin the studied commit**: `8b94ee7ff` (`v0.8.1-2`, 2026-09-12, `VERSION` 0.8.1). The catalog is generated from a spec that changes whenever an indicator or parameter is touched; re-verify the counts cited here (201 functions, 10 groups, 23 unstable-period functions) on any submodule update.

---

# Study complete

All eight references under `external/` are studied. Freqtrade, Hummingbot and
Jesse concluded on 2026-10-15 (Parts E, F, G), joining the AFML exercises,
Backtrader, VectorBT and CCXT (Parts A–D) and TA-Lib (Part H). ROADMAP
chapter 25's closing requirement — document the conclusions — is met, which
means **`external/` is no longer needed to continue this work**; see the next
section.

## Disk cost, and what can now be removed

Measured 2026-10-15. The working copies under `external/` are **930 MB**, but
the real cost is the git object store: `.git/modules/` holds **11 GB**, and
the checkout is **12 GB** in total. `external/ccxt` alone — working tree plus
its object store — is **8.5 GB**, 70 % of everything.

| Reference | Study | Working tree | `.git/modules` |
|---|---|---|---|
| `ccxt` | **done** (Part D) | 512 MB | **8.0 GB** |
| `freqtrade` | **done** (Part E) | 61 MB | 764 MB |
| `vectorbt` | **done** (Part C) | 141 MB | 701 MB |
| `hummingbot` | **done** (Part F) | 21 MB | 561 MB |
| `ta-lib` | **done** (Part H) | 96 MB | 557 MB |
| `jesse` | **done** (Part G) | 92 MB | 102 MB |
| `backtrader` | **done** (Part B) | 7.8 MB | 23 MB |
| `adv-financial-ml-marcos-exercises` | **done** (Part A) | 888 KB | 1.4 MB |

**All eight studies are concluded, so all 12 GB can now be released.** Chapter
25's requirement is the study and its written conclusions, not the vendored
copies, and those conclusions are the Parts above. Nothing in `src/`, `tests/`,
`pyproject.toml` or `Makefile` references `external/`, and both CI workflows
pass `submodules: false`.

**Nothing is lost either way.** These are git submodules pinned to exact
commits of public repositories, recorded in `.gitmodules` and in the index,
so any of them comes back at the same revision with one command:

```bash
git submodule update --init external/ccxt
```

The only cost of releasing one is re-downloading its history if you ever need
the vendored copy again — and the project uses `ccxt` from PyPI
(`pip install -e ".[ccxt]"`), not from `external/`, so normal work never needs
any of them. To reclaim the space:

```bash
for r in ccxt freqtrade vectorbt hummingbot ta-lib jesse backtrader \
         adv-financial-ml-marcos-exercises; do
  git submodule deinit -f "external/$r"
  rm -rf ".git/modules/external/$r"
done
```

The full release-and-restore cycle was tested on 2026-10-15 on the smallest
submodule, `external/adv-financial-ml-marcos-exercises`: after `deinit` and
the `rm`, `git submodule update --init` brought the identical revision back
(`57ac9979…`), `git status` stayed clean, and no committed state changed.
Expect the same for the rest; the only difference is the download size on
restore.

Leave the `.gitmodules` entries in place. They cost nothing at clone time
(submodules are not fetched by default) and they carry the pinned revisions
the Parts above refer to. A fork keeps the option to re-fetch without any
change to this document.

## Maintenance rules for this document

- [ ] Update A.3 whenever an implementation of a technique lands, noting the project module that fulfills it, and mirror the status change in `afml-techniques.md`.
- [ ] Each Part pins the commit it studied; if a reference is re-fetched at a newer commit, re-verify that Part's claims before trusting them again — several of these projects move fast, and their own documentation has already drifted (see Parts E, F and G).
- [ ] Keep A.9 in sync with `afml-techniques.md`; if the local compilation is replaced, re-verify the page pointers.
- [ ] Resolve the open questions recorded in `afml-techniques.md` before implementing the affected technique (Ch. 49.9).
- [ ] Never commit or redistribute the book — printed text or exhibit compilation (book rule: cite by bibliographic reference only).
- [ ] Every conclusion must be backed by the referenced files, not by the book's table of contents.

