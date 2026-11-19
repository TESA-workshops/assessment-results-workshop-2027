# Column dictionary

This file defines the columns of the time series CSVs listed by the [YAML generator](index.html):

- **`stock_timeseries.csv`**: one row per year, for the stock.
- **`component_timeseries.csv`** (optional): one row per component per year, for stocks also reported by component (e.g. salmon CUs, herring sections, fishing areas).

What each column means (units, basis, ages, survey, interval level) goes in `assessment.yaml`, not in the CSV.
See [`fsar-data/README.md`](../fsar-data/README.md) for the full format and [`fsar-data/vocab/`](../fsar-data/vocab/) for the tables these definitions come from.

Definitions are adapted from the CanSTAR template dictionaries ([`CanSTAR-template/metadata/column_dictionary_assess.csv`](../CanSTAR-template/metadata/column_dictionary_assess.csv)).
[What changed from CanSTAR](#what-changed-from-canstar) maps the old columns to the new ones.

**Contents**

- [Rules for every column](#rules-for-every-column)
- [Key columns](#key-columns)
- [Metric columns](#metric-columns)
  - [Catch](#catch)
  - [Stock size](#stock-size)
  - [Fishing pressure](#fishing-pressure)
  - [Recruitment](#recruitment)
- [Interval columns (`_lower`, `_upper`)](#interval-columns-_lower-_upper)
- [Ratio columns (`series/denominator`)](#ratio-columns-seriesdenominator)
- [Units](#units)
- [Column order](#column-order)
- [What changed from CanSTAR](#what-changed-from-canstar)

## Rules for every column

- **Numbers only.** Metric cells are plain numbers: no units, text, symbols or thousands separators.
- **Blank means missing.** Leave a cell empty if there is no value for that year.
  Don't write `NA`, `0` or `-`.
- **No units in column names.** Each metric's units are in its `metrics:` entry in `assessment.yaml`, and apply to its `_lower` and `_upper` columns too.
- **One `metrics:` entry per metric column.** Every metric column in either CSV needs an entry in `assessment.yaml`, and every entry needs a column.
  The form writes these for you.
- **You only need to include the columns you have.** All metric columns are optional, but there must be at least one.

## Key columns

| Column | File | Type | Required | Description |
|---|---|---|---|---|
| `year` | both | integer | yes | Year the estimates correspond to in the time series. One row per year (per component). |
| `component` | `component_timeseries.csv` | string | yes | ID of the component (e.g. a salmon CU or herring section). Must match an ID under `components:` in `assessment.yaml`, where its English and French names are given. |

Each row is identified by `year` in `stock_timeseries.csv` and by `component, year` in `component_timeseries.csv`.
The assessment itself is identified by its folder (e.g. `example-stock-2024`), formed from the major stock name and year assessed in `assessment.yaml`.

## Metric columns

All metric columns are numeric.
**File** says which CSV a column can go in: reference points belong to the stock, thresholds to components, and the rest can go in either.

**Series** are the main time series in each panel of the FSAR figure.
**Reference points** and **thresholds** are given every year: repeat a constant value, or change it in the years it changes.
**Context** columns are shown alongside the series.

### Catch

| Column | Label | Role | File | Units | Description |
|---|---|---|---|---|---|
| `catch` | Catch | series | both | mass, count | Total removals in each year. Give an interval if removals are uncertain (e.g. recreational catch). |
| `total_allowable_catch` | TAC | context | both | mass, count | Total allowable catch in each year. Never has an interval. |
| `msy` | MSY | context | stock | mass, count | Maximum sustainable yield, if estimated. Repeated each year like a reference point. |

### Stock size

Report the stock-size series used for management and comparison with reference points.
Most stocks need only `spawning_stock` and/or `total_stock`.
The column name says *which part of the stock*; the units say *biomass or numbers*.

| Column | Label | Role | File | Units | Description |
|---|---|---|---|---|---|
| `spawning_stock` | Spawning stock | series | both | mass, count | Spawning (mature) part of the stock. In mass units this is spawning stock biomass (SSB); in count units it is spawner abundance. Ages can be given in `min_age` / `max_age`. |
| `total_stock` | Total stock | series | both | mass, count | Whole stock, above any minimum age given in `min_age`. |
| `exploitable_stock` | Exploitable stock | series | both | mass, count | Part of the stock vulnerable to the fishery. Only if it is the main stock-size series (e.g. snow crab exploitable biomass, fishable shrimp biomass). |
| `index` | Index | series | both | index | Survey or fishery index of stock size, from the source named in `survey`. Only for assessments based on an index rather than a population model; a model fit to surveys reports its estimated stock size instead. One index per assessment: the one status is judged from. |
| `limit_reference` | LRP | reference point | stock | same as its series | Limit reference point, as defined in the assessment. `basis` and `multiplier` say what it is (e.g. 0.4 B_MSY). |
| `upper_stock_reference` | USR | reference point | stock | same as its series | Upper stock reference point, as defined in the assessment. |
| `target_reference` | TRP | reference point | stock | same as its series | Target reference point, as defined in the assessment. |
| `lower_threshold` | Lower threshold | threshold | component | same as its series | Lower status threshold for a component (e.g. a salmon CU lower benchmark). |
| `upper_threshold` | Upper threshold | threshold | component | same as its series | Upper status threshold for a component (e.g. a salmon CU upper benchmark). |

A reference point or threshold is in the same units as the series it is compared to.
If more than one stock-size series is reported, `applies_to` in `assessment.yaml` says which.

### Fishing pressure

| Column | Label | Role | File | Units | Description |
|---|---|---|---|---|---|
| `fishing_pressure` | Fishing pressure | series | both | `1/yr`, `proportion` | In `1/yr`, instantaneous fishing mortality (F), averaged over ages, apical, or aggregate (`type`). As a `proportion`, a harvest rate: catch divided by the stock series in `applies_to`. |
| `removal_reference` | RR | reference point | stock | same as `fishing_pressure` | Removal reference, as defined in the assessment (e.g. F_MSY, F_0.1, F40%). |
| `removal_threshold` | Removal threshold | threshold | component | same as `fishing_pressure` | Removal threshold for a component. |
| `natural_mortality` | M | context | both | `1/yr` | Instantaneous natural mortality in each year. Ages can be given in `min_age` / `max_age`. |

### Recruitment

| Column | Label | Role | File | Units | Description |
|---|---|---|---|---|---|
| `recruitment` | Recruitment | series | both | count, mass | Recruits in each year, at the age given in `age` (e.g. millions of age-1 fish). |

## Interval columns (`_lower`, `_upper`)

Any metric column except `total_allowable_catch` can have an uncertainty interval in two more columns, named after it:

| Column | Type | Description |
|---|---|---|
| `<column>_lower` | numeric | Lower bound of the interval for `<column>` in that year. |
| `<column>_upper` | numeric | Upper bound of the interval for `<column>` in that year. |

For example, `spawning_stock`, `spawning_stock_lower`, `spawning_stock_upper`.

- Give both bounds or neither, with `lower <= value <= upper`.
- Bounds are in the same units as the column.
- All intervals in an assessment share one **level** (e.g. `0.95`) and **type** (`confidence` or `credible`), set under `interval:` in `assessment.yaml`.
- Each metric with bounds has `bounds: true` in its `metrics:` entry.
- **Give intervals, not standard errors or CVs.** If you have a CV, convert it yourself, since you know the error distribution.
  For a lognormal 95% interval: `x * exp(±1.96 * sqrt(log(1 + CV^2)))`.

In the form, stock size, fishing pressure, recruitment, ratio and `msy` columns get bounds whenever the assessment has intervals; for `catch`, `natural_mortality` and reference points you tick whether they have one.

## Ratio columns (`series/denominator`)

If the assessment estimates a ratio such as B/B_MSY, F/F_MSY or B/LRP, give it as its own column, with its own interval from the model:

| Column | Description |
|---|---|
| `<series>/b_msy` | A stock-size series divided by B_MSY (or its proxy). |
| `fishing_pressure/f_msy` | Fishing pressure divided by F_MSY (for a harvest rate, the harvest rate at MSY). |
| `<series>/<reference>` | A series divided by a reference point that applies to it, e.g. `spawning_stock/limit_reference` or `fishing_pressure/removal_reference`. |
| `<series>/<denominator>_lower`, `_upper` | The ratio's interval. A ratio has bounds when its series does. |

- `<series>` is a stock-size series (`spawning_stock`, `total_stock`, `exploitable_stock`, `index`) or `fishing_pressure`.
  Catch and recruitment don't have ratios.
- Ratios are unitless.
- **Report the ratio as the model estimates it; don't divide two columns.**
  The series and its reference point usually have correlated errors, so their separate intervals overstate how uncertain status is.
  The ratio's own interval doesn't.
- Ratios need no `metrics:` entry of their own.
  The series lists them, e.g. `ratios: [limit_reference, b_msy]` under `spawning_stock`.
  A reference point used as a denominator needs its usual entry.

In the database a ratio column becomes a row with `metric` = the series and `relative_to` = the denominator.

## Units

Units are set per metric in `assessment.yaml` (`units:`), from these codes.
**Units** in the tables above lists the allowed dimensions.

| Dimension | Codes |
|---|---|
| mass | `kg`, `t` (tonnes), `kt` (thousand tonnes) |
| count | `n` (number), `thousands`, `millions`, `billions` |
| rate | `1/yr` (instantaneous annual rate) |
| proportion | `proportion` (unitless fraction from 0 to 1) |
| index | `kg/tow`, `n/tow`, `kg/km2`, `n/km2`, `kg/trap`, `relative` (scaled to a reference period, e.g. mean = 1) |
