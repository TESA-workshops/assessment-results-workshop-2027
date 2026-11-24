# DFO stock assessment output: column dictionary

This page explains the columns in the time series CSV made by the [YAML/CSV generator](https://tesa-workshops.github.io/assessment-results-workshop-2027/yaml-generator/).
The form tells you which columns your CSV needs and can download a blank copy with the headers filled in.
Use this page to check what goes in each one, and how each column is described in `assessment.yaml`.

Most assessments only need one file, `stock_timeseries.csv`, with one row per year.
If the stock is also reported by component (e.g. salmon CUs, herring sections or fishing areas), there is a second file; see [Components](#components).

**Contents**

- [General rules](#general-rules)
- [Catch](#catch)
- [Stock size](#stock-size)
- [Fishing pressure](#fishing-pressure)
- [Recruitment](#recruitment)
- [Intervals (`_lower`, `_upper`)](#intervals-_lower-_upper)
- [Ratios (e.g. B/B_MSY)](#ratios-eg-bb_msy)
- [Components](#components)
- [`assessment.yaml`](#assessmentyaml)
  - [Units](#units)

## General rules

- The first column is `year`. There is one row per year.
- Include only the columns you have. Every metric column is optional, but you need at least one.
- Cells hold plain numbers: no units, text, symbols or thousands separators.
- Leave a cell blank if there's no value for that year. Don't use `NA`, `0` or `-`.
- Units go in `assessment.yaml` (the form asks for them; see [Units](#units)), not in the column name. A column's units also apply to its `_lower` and `_upper` columns.
- Reference points are given every year. If one doesn't change, repeat the same value down the column. If it does, change it in the years it changes.
- Other names in `code` that aren't columns, such as `basis`, `applies_to` or `min_age`, are fields in `assessment.yaml`. The form asks for them. See [`assessment.yaml`](#assessmentyaml) for the full list.

## Catch

| Column | Label | Units | Description |
|---|---|---|---|
| `catch` | Catch | mass, count | Total removals in each year. Give an interval if removals are uncertain (e.g. recreational catch). |
| `total_allowable_catch` | TAC | mass, count | Total allowable catch. Never has an interval. |
| `msy` | MSY | mass, count | Maximum sustainable yield, if estimated. Repeat it each year like a reference point. |

## Stock size

Include the stock-size series the assessment estimates.

| Column | Label | Units | Description |
|---|---|---|---|
| `spawning_stock` | Spawning stock | mass, count | Spawning (mature) part of the stock: SSB in mass units, spawner abundance in count units. Ages can go in `min_age` / `max_age`. |
| `total_stock` | Total stock | mass, count | Whole stock, above any minimum age given in `min_age`. |
| `exploitable_stock` | Exploitable stock | mass, count | Part of the stock vulnerable to the fishery. Say in `notes` what it's vulnerable to, such as which fleet and when in the year. |
| `index` | Index | index | Survey or fishery index of stock size, from the source named in `survey`. Use this only if status is judged from an index rather than a population model. If a model is fit to the surveys, give its estimated stock size instead. One index per assessment. |
| `status_score` | Composite status score | `score` | Use this only if status is judged from a score that combines several indicators (e.g. a snow crab stock health index). Describe the scoring in `notes`. Stock file only. Has no interval or ratios. |
| `limit_reference` | LRP | same as its series | Limit reference point. `basis` and `multiplier` say what it is (e.g. 0.4 B_MSY). |
| `upper_stock_reference` | USR | same as its series | Upper stock reference point. |
| `target_reference` | TRP | same as its series | Target reference point. |

If you give more than one series in this table, `applies_to` (in the YAML file) on each reference point says which one it's compared to.
For `status_score`, the zone boundaries are the reference points. Put the score at each boundary in the `limit_reference` and `upper_stock_reference` columns (e.g. 2 and 5), repeated each year. They take `score` units and no `basis` or `multiplier`; say how the boundaries were set in `notes`.

## Fishing pressure

| Column | Label | Units | Description |
|---|---|---|---|
| `fishing_pressure` | Fishing pressure | `1/yr`, `proportion` | Either instantaneous fishing mortality (F) in `1/yr`, or a harvest rate as a `proportion`. For F, `type` says how it's summarized over ages. For a harvest rate, `applies_to` says which stock-size series catch is divided by (often `exploitable_stock`). |
| `removal_reference` | RR | same as `fishing_pressure` | Removal reference point (e.g. F_MSY, F_0.1, F40%). |
| `natural_mortality` | M | `1/yr` | Instantaneous natural mortality. Ages can go in `min_age` / `max_age`. |

## Recruitment

| Column | Label | Units | Description |
|---|---|---|---|
| `recruitment` | Recruitment | count, mass | Recruits in each year, at the age given in `age` (e.g. millions of age-1 fish). |
| `recruitment_deviation` | Recruitment deviations | none | Recruitment deviations on the log scale, e.g. log(R) − log(expected R), from the stock-recruit curve or mean recruitment. Has no `units`. |

## Intervals (`_lower`, `_upper`)

Any column except `total_allowable_catch` and `status_score` can have an uncertainty interval.
Add two columns named after it, e.g. `spawning_stock_lower` and `spawning_stock_upper`.

- Give both bounds or neither, with lower ≤ value ≤ upper.
- All intervals in one assessment have the same level (e.g. 0.95) and type (`confidence` or `credible`), set once under `interval:` in the YAML.
- Give the interval itself, not a standard error or CV. If you only have a CV, convert it using the error distribution you assumed. For a lognormal 95% interval: `x * exp(±1.96 * sqrt(log(1 + CV^2)))`.

## Ratios (e.g. B/B_MSY)

If the model estimates a ratio such as B/B_MSY, F/F_MSY or B/LRP, you should also give it as its own column.
The name is the series and the denominator separated by `/`:

- `spawning_stock/b_msy`: spawning stock divided by B_MSY (or its proxy)
- `fishing_pressure/f_msy`: fishing pressure divided by F_MSY
- `spawning_stock/limit_reference`: spawning stock divided by the LRP

The denominator is `b_msy` (for stock size), `f_msy` (for fishing pressure), or a reference point or threshold that applies to the series.
Any stock-size series except `status_score`, or `fishing_pressure`, can have a ratio.
If the series has an interval, so does the ratio (`spawning_stock/b_msy_lower`, `spawning_stock/b_msy_upper`).

Take the ratio and its interval from the model rather than dividing one column by another.
The series and reference point usually have correlated errors, so dividing their separate intervals makes status look more uncertain than it is.
That's why we ask for the ratios, with their intervals, as separate columns.

In R, read and write the CSV in one of these ways so the `/` stays in column names and missing values are left blank:

- readr: `read_csv("stock_timeseries.csv")` and `write_csv(df, "stock_timeseries.csv", na = "")`
- base R: `read.csv("stock_timeseries.csv", check.names = FALSE)` and `write.csv(df, "stock_timeseries.csv", row.names = FALSE, na = "")`

Refer to these columns with backticks, e.g. ``df$`spawning_stock/b_msy` ``.

## Components

Skip this section if your stock isn't reported by component.

Components go in a second file, `component_timeseries.csv`, with one row per component per year.
It has a `component` column next to `year`, holding the component's ID.
Each ID is listed under `components:` in `assessment.yaml` with its English and French names:

```yaml
components:
  north:
    name:
      en: North section
      fr: Section nord
```

The component file takes the same columns as the stock file, except the reference points (`limit_reference`, `upper_stock_reference`, `target_reference`, `removal_reference`) and `msy`, which belong to the stock.
Components have thresholds instead of reference points:

| Column | Label | Units | Description |
|---|---|---|---|
| `lower_threshold` | Lower threshold | same as its series | Lower status threshold (e.g. a salmon CU lower benchmark). |
| `upper_threshold` | Upper threshold | same as its series | Upper status threshold (e.g. a salmon CU upper benchmark). |
| `removal_threshold` | Removal threshold | same as `fishing_pressure` | Removal threshold for a component. |

Thresholds take the same fields as reference points (`basis`, `multiplier` and so on).

## `assessment.yaml`

The form writes `assessment.yaml` for you.
If you edit it by hand, every metric column needs an entry under `metrics:`, and every entry needs a column.
`_lower` / `_upper` columns and ratio columns don't get entries of their own.

```yaml
interval:
  level: 0.95
  type: confidence

metrics:
  spawning_stock:
    units: t
    bounds: true
    ratios: [limit_reference, b_msy]
  limit_reference:
    units: t
    basis: b_msy
    multiplier: 0.4
  fishing_pressure:
    units: 1/yr
    type: average
    min_age: 5
    max_age: 8
```

Fields a metric entry can have:

| Field | Used by | Description |
|---|---|---|
| `units` | all except `recruitment_deviation` | Required. A code from [Units](#units). |
| `bounds` | any with an interval | `true` if the column has `_lower` and `_upper` columns. |
| `ratios` | stock-size series, `fishing_pressure` | Denominators of its ratio columns, e.g. `[limit_reference, b_msy]`. |
| `notes` | all | Free text. |
| `survey` | `index` | Required. The survey or fishery the index comes from (e.g. RV summer survey). |
| `min_age`, `max_age` | stock-size series, `fishing_pressure`, `natural_mortality` | Whole numbers. Leave `max_age` blank for a plus group. Required for `fishing_pressure` when `type` is `average`. |
| `age` | `recruitment`, `recruitment_deviation` | Age at recruitment, a whole number. Required for `recruitment`. |
| `type` | `fishing_pressure` | Required when units are `1/yr`: `average` (over ages), `apical` (fully selected) or `aggregate` (no age structure). |
| `applies_to` | reference points, thresholds, `fishing_pressure` | The stock-size series it's compared to (or, for a harvest rate, that catch is divided by). Required if there's more than one stock-size series. |
| `basis` | reference points, thresholds | What it's based on; see the codes below. Required unless `applies_to` is `status_score`. |
| `multiplier` | reference points, thresholds | A number, e.g. `0.4` for 0.4 B_MSY or `1` for F_MSY. Required unless `applies_to` is `status_score`. |
| `period` | basis `mean` or `percentile` | First and last year, e.g. `[1985, 2009]`. |
| `percentile` | basis `percentile` | e.g. `50` for the median. |
| `spr` | basis `f_spr` | e.g. `40` for F40%. |

`basis` codes:

- Stock-size reference points and thresholds: `b_msy`, `b_msy_proxy`, `b0`, `b_recover`, `s_msy`, `s_gen`, `mean`, `percentile`, `other`
- Removal references and thresholds: `f_msy`, `u_msy`, `f_0.1`, `f_spr`, `other`

For `b_msy_proxy` and `other`, describe it in `notes`.

### Units

Each column's units go in its `units:` field, using one of these codes:

| Type | Codes |
|---|---|
| mass | `kg`, `t` (tonnes), `kt` (thousand tonnes) |
| count | `n` (number), `thousands`, `millions`, `billions` |
| rate | `1/yr` (instantaneous annual rate) |
| proportion | `proportion` (0 to 1) |
| index | `kg/tow`, `n/tow`, `kg/km2`, `n/km2`, `kg/trap`, `relative` (scaled to a reference period, e.g. mean = 1) |
| score | `score` (points on a status scoring scale) |

The rest of the file (`stock:`, `method:`, `meeting:`, `publications:` and so on) is explained in the comments of the YAML template the form downloads.
To check a file you've edited, load it back into the form with "Load existing YAML file".
