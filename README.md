# MC//Cost Risk

**Monte Carlo cost risk analysis in a single HTML file. Know your P80.** **[• Live demo](https://frantis3k.github.io/monte-carlo-price-risk-modeling/mc_cost_risk.html)**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![No install](https://img.shields.io/badge/install-none-brightgreen)
![Single file](https://img.shields.io/badge/size-1%20HTML%20file-blue)
![Dependencies](https://img.shields.io/badge/dependencies-0-blue)

![MC//Cost Risk GIF](docs/mc_cost_risk_demo.gif)

You list the cost items of a project and describe how uncertain each one is, for example "min 900, most likely 950, max 1 150". The tool then simulates thousands of random versions of the project (trials), adds the items up in each one and shows how the total is spread. From that you read off:

- **P80**: a budget that is enough in 80 % of trials (any confidence level works, P80 is an industry standard),
- **contingency**: how much you need on top of the base estimate to reach it,
- **the probability** that a given budget holds,
- **which items drive the risk**, so you know where a better estimate pays off most.

Everything runs in your browser. There's nothing to install, no server and no account, and your data never leaves your computer.

Y2K Futurism inspired design just for the love of it.

## Quick start

**Try it online:** open the [live demo](https://frantis3k.github.io/monte-carlo-price-risk-modeling/mc_cost_risk.html), nothing to download.

**Or just open it locally**

1. Download [`mc_cost_risk.html`](mc_cost_risk.html) (or clone the repository).
2. Double-click it to open it in any modern browser (Chrome, Edge, Firefox or Safari).
3. Click **Load Example** to see a complete model, or **Start from Scratch**.


---

## Contents

- [Motivation](#motivation)
- [Features](#features)
- [How to use](#how-to-use)
- [Reading the results](#reading-the-results)
- [Data & privacy](#data--privacy)
- [Customising](#customising)
- [Contributing](#contributing)
- [License](#license)

---

## Motivation

A single-number cost estimate is almost always wrong. The real question is *how wrong it can be, and how likely that is*. Quantitative risk analysis with Monte Carlo simulation answers that.

This tool was developed for a course *Nuclear Power Plant Project development and Contract* at *KINGS* - KEPCO International Nuclear Graduate School. The students investigated why cost of building a Nuclear Power Plant can rise significantly due to cost uncertainties (not considering time delays adding whole another level of budget overruns).

The established tools are good, but they come with barriers: learning curve, usually under a licence and must be installed (often Windows only). That's hard to justify for simple occasional use and just getting the point across to students.


MC//Cost Risk aims to be the opposite:
- **Free and open source** (MIT).
- **Extremely easy to use**: no learning curve, just read the '?' explanations if needed.
- **Zero install**: one HTML file you open in any browser, even offline.
- **Private by design**: no server and no uploads. Data is kept in your own browser only.
- **Good enough for simple real work**: the methods match those in commercial tools (Latin Hypercube sampling, BetaPERT, Iman–Conover correlation, sensitivity analysis, scenario comparison).

## Features

- **7 input distributions**: Triangular, BetaPERT, Normal, Lognormal, Uniform, *Yes-No × Impact* (discrete risk events) and Fixed value.
- **Sampling**: Latin Hypercube (default) or plain Monte Carlo, 100 to 1 000 000 trials.
- **Reproducible runs**: a seeded random number generator gives the same seed and the same results.
- **Correlations** between items (rank / Spearman), applied with the Iman–Conover method. Each item keeps its exact distribution. Inconsistent correlation sets are repaired automatically with a warning, and an *Achieved* column shows the correlation actually reached.
- **Results**: probability of meeting a budget, P-value at any confidence level, contingency, mean, standard deviation, coefficient of variation, skewness, a percentile table (P5 to P95) and a histogram with Base / P50 / Pxx markers.
- **Sensitivity analysis**: each item's contribution to the variance of the total, as a ranked bar chart.
- **Scenarios**: save runs under a name and compare them side by side in a table and in an overlay chart (frequency or cumulative S-curve).
- **CSV export** of every trial (each item and the total) for further analysis in Excel, Python or R.
- **Auto-save** to browser storage, built-in **"?" help** on every panel.
- **Example model** (a nuclear power plant) that uses every feature, loaded with one click.


## How to use

The page is organised in six numbered steps. The tabs in the toolbar jump to them, and their status lights show what is done (green), out of date (orange) or optional (hollow).

### 1 · Model & Assumptions

Add one row per cost item, pick a distribution and fill in its parameters. *Base* (the value used in a deterministic estimate) and *Mean* are computed live, together with their totals. Set the **Units** (e.g. `mil. €`) for labels.

| Distribution | Parameters | Use it when… |
|---|---|---|
| **Triangular** | Min, Likeliest, Max | You have a three-point estimate. Puts relatively much weight on the extremes. |
| **BetaPERT** | Min, Likeliest, Max | Same three points, but with a smoother curve concentrated around the likeliest value - the extremes are rarely expected. A common default. |
| **Normal** | Mean, Std. Dev. | The uncertainty is symmetric. (beware of negative values) |
| **Lognormal** | Mean, Std. Dev. | The cost can't be negative but can blow up (long right tail). |
| **Uniform** | Min, Max | You only know the range. |
| **Yes-No × Impact** | Probability (0–1), Impact | A discrete risk event that either happens (and costs *Impact*) or doesn't. Its Base is 0. |
| **Fixed Value** | Value | There's no uncertainty, e.g. a signed contract price. |

When you switch an item's distribution, its parameters are converted sensibly instead of being wiped.

### 2 · Correlations (optional)

Items that tend to overrun together (same labour market, same schedule, same supplier) should be correlated. Otherwise the spread of the total is **underestimated**. Add a pair, choose the two items and enter a rank correlation from −1 to 1 (e.g. 0.6).

### 3 · Simulate

Choose **Latin Hypercube** (it converges faster) or **Monte Carlo**, the number of **trials** (10 000 is usually enough) and a **seed**, then press **▶ Run**. Whenever you change the model later, the results fade out and the Run button pulses until you run it again.

### 4 · Results

Set the **confidence level** (default 80 %) and optionally a **budget** to test with "Probability that total ≤ …" (if you leave it empty, the base estimate is used). Read the key figures, the histogram and the statistics and percentile tables. **Export All Trials (CSV)** downloads the raw data.

### 5 · Sensitivity

This shows which items contribute most to the uncertainty of the total. Narrowing the estimate of the top items reduces the risk the most. Purple bars mark items that move *against* the total.

### 6 · Scenarios

Name and **save** the current run, change the model (e.g. remove correlations or mitigate a risk), run again and save again. The table compares the statistics, with Δ measured against the first scenario. The chart overlays the distributions as frequency curves or cumulative S-curves.

## Reading the results

- **Base estimate**: the sum of the most likely values, i.e. what a deterministic estimate would say.
- **P50 (median)**: half of the trials end below it. Because cost distributions are usually skewed to the right, P50 is often *above* the base.
- **P80**: the total is not exceeded in 80 % of trials. It is a common budgeting level for projects.
- **Contingency = Pxx − Base**: the reserve needed on top of the base estimate to reach the chosen confidence.
- **Probability ≤ Base**: how likely the deterministic estimate is to hold. It is often surprisingly low.

*Example:* base 6 000, P80 6 600 → contingency 600 (10 % of base).

## Data & privacy

- Everything stays **in your browser**, in `localStorage`, under two keys:
  - `mcCostRisk.model.v1`: items, distributions, correlations and settings,
  - `mcCostRisk.scenarios.v1`: saved scenarios (their sorted trial totals).
- Data is saved automatically after every change. **Nothing is uploaded anywhere.**
- Storage is tied to the browser and the file's location. Another browser, another computer or a moved file starts blank. Use **CSV export** to keep the results.
- **New Blank** deletes both keys. Clearing the browser's site data does the same.
- The only network requests are for web fonts from Google Fonts. Offline, the page falls back to system fonts and works the same.

## Customising

The script is organised and commented for editing. The most useful places to change:

- **Add a distribution**: add an entry to the `DISTRIBUTIONS` object (label, parameter names, likeliest value, mean, parameter check and inverse CDF). The menu, table and simulation pick it up automatically.
- **Change the forecast formula**: by default the total is the sum of all items. Edit `FORECAST(v)` (e.g. to add a multiplier or an escalation factor). The base estimate uses the same function, so the two stay consistent.
- **Number format**: numbers are displayed in Czech format (space as thousands separator, decimal comma) via `toLocaleString('cs-CZ')` in the `fmt` helper. Change `'cs-CZ'` to e.g. `'en-US'`.
- **CSV format**: the export uses `;` as separator and a decimal comma (Czech/European Excel). Adjust the `exportCsv` handler for other locales.
- **Example model / defaults**: see `EXAMPLE_MODEL` and `DEFAULT_PREFS` (10 000 trials, LHS, seed 123, 80 % confidence).

## Contributing

Issues and pull requests are welcome, whether bug reports, new distributions, translations or ideas.

Please keep the project's spirit:

- **One self-contained HTML file** with no build step and no frameworks.
- **Plain, commented code** that a non-specialist can follow.
- Test changes by opening the file in a browser and running the example model.

## License

Released under the [MIT License](LICENSE). © 2026 Frantis3k.

You may use, copy, modify and distribute it freely, including commercially, as long as the copyright notice is kept. The software is provided "as is", without warranty of any kind.
