# DFO stock assessment output: column dictionary

This page explains the columns in the time series CSV made by the [YAML/CSV generator](https://tesa-workshops.github.io/assessment-results-workshop-2027/yaml-generator/).
The form tells you which columns your CSV needs and can download a blank copy with the headers filled in.
Use this page to check what goes in each one, and how each column is described in `assessment.yaml` if you edit that file by hand.

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
- [Units](#units)
- [Components](#components)
- [Editing `assessment.yaml` by hand](#editing-assessmentyaml-by-hand)

## General rules

- The first column is `year`. There is one row per year.
- Include only the columns you have. Every metric column is optional, but you need at least one.
- Cells hold plain numbers: no units, text, symbols or thousands separators.
- Leave a cell blank if there's no value for that year. Don't use `NA`, `0` or `-`.
- Units go in `assessment.yaml` (the form asks for them), not in the column name. A column's units also apply to its `_lower` and `_upper` columns.
- Reference points are given every year. If one doesn't change, repeat the same value down the column. If it does, change it in the years it changes.

In the tables below, **series** are the main lines in each panel of the FSAR figure and **context** columns are drawn alongside them.

## Catch

| Column | Label | Role | Units | Description |
|---|---|---|---|---|
| `catch` | Catch | series | mass, count | Total removals in each year. Give an interval if removals are uncertain (e.g. recreational catch). |
| `total_allowable_catch` | TAC | context | mass, count | Total allowable catch. Never has an interval. |
| `msy` | MSY | context | mass, count | Maximum sustainable yield, if estimated. Repeat it each year like a reference point. |

## Stock size

Include every stock-size series the assessment estimates.
The column name says which part of the stock it is; the units say whether it's biomass or numbers.

| Column | Label | Role | Units | Description |
|---|---|---|---|---|
| `spawning_stock` | Spawning stock | series | mass, count | Spawning (mature) part of the stock: SSB in mass units, spawner abundance in count units. Ages can go in `min_age` / `max_age`. |
| `total_stock` | Total stock | series | mass, count | Whole stock, above any minimum age given in `min_age`. |
| `exploitable_stock` | Exploitable stock | series | mass, count | Part of the stock vulnerable to the fishery (e.g. snow crab exploitable biomass). Say in `notes` what it's vulnerable to, such as which fleet and when in the year. |
| `index` | Index | series | index | Survey or fishery index of stock size, from the source named in `survey`. Use this only if status is judged from an index rather than a population model. If a model is fit to the surveys, give its estimated stock size instead. One index per assessment. |
| `limit_reference` | LRP | reference point | same as its series | Limit reference point. `basis` and `multiplier` say what it is (e.g. 0.4 B_MSY). |
| `upper_stock_reference` | USR | reference point | same as its series | Upper stock reference point. |
| `target_reference` | TRP | reference point | same as its series | Target reference point. |

If you give more than one stock-size series, `applies_to` on each reference point says which one it's compared to.
That's the series shown in the FSAR figure.

## Fishing pressure

| Column | Label | Role | Units | Description |
|---|---|---|---|---|
| `fishing_pressure` | Fishing pressure | series | `1/yr`, `proportion` | Either instantaneous fishing mortality (F) in `1/yr`, or a harvest rate as a `proportion`. For F, `type` says how it's summarized over ages. For a harvest rate, `applies_to` says which stock-size series catch is divided by (often `exploitable_stock`). |
| `removal_reference` | RR | reference point | same as `fishing_pressure` | Removal reference (e.g. F_MSY, F_0.1, F40%). |
| `natural_mortality` | M | context | `1/yr` | Instantaneous natural mortality. Ages can go in `min_age` / `max_age`. |

## Recruitment

| Column | Label | Role | Units | Description |
|---|---|---|---|---|
| `recruitment` | Recruitment | series | count, mass | Recruits in each year, at the age given in `age` (e.g. millions of age-1 fish). |
| `recruitment_deviation` | Recruitment deviations | context | none | Recruitment deviations on the log scale, e.g. log(R) − log(expected R), from the stock-recruit curve or mean recruitment. Has no `units`. |

## Intervals (`_lower`, `_upper`)

Any column except `total_allowable_catch` can have an uncertainty interval.
Add two columns named after it, e.g. `spawning_stock_lower` and `spawning_stock_upper`.

- Give both bounds or neither, with lower ≤ value ≤ upper.
- All intervals in one assessment have the same level (e.g. 0.95) and type (`confidence` or `credible`), set once under `interval:`.
- Give the interval itself, not a standard error or CV. If you only have a CV, convert it using the error distribution you assumed. For a lognormal 95% interval: `x * exp(±1.96 * sqrt(log(1 + CV^2)))`.

## Ratios (e.g. B/B_MSY)

If the model estimates a ratio such as B/B_MSY, F/F_MSY or B/LRP, you can give it as its own column.
The name is the series and the denominator separated by `/`:

- `spawning_stock/b_msy`: spawning stock divided by B_MSY (or its proxy)
- `fishing_pressure/f_msy`: fishing pressure divided by F_MSY
- `spawning_stock/limit_reference`: spawning stock divided by the LRP

The denominator is `b_msy` (for stock size), `f_msy` (for fishing pressure), or a reference point or threshold that applies to the series.
Any stock-size series or `fishing_pressure` can have a ratio. Catch and recruitment can't.
Ratios have no units. If the series has an interval, so does the ratio (`spawning_stock/b_msy_lower`, `spawning_stock/b_msy_upper`).

Take the ratio and its interval from the model rather than dividing one column by another.
The series and reference point usually have correlated errors, so dividing their separate intervals makes status look more uncertain than it is.

## Units

Each column's units go in its `units:` field, using one of these codes:

| Type | Codes |
|---|---|
| mass | `kg`, `t` (tonnes), `kt` (thousand tonnes) |
| count | `n` (number), `thousands`, `millions`, `billions` |
| rate | `1/yr` (instantaneous annual rate) |
| proportion | `proportion` (0 to 1) |
| index | `kg/tow`, `n/tow`, `kg/km2`, `n/km2`, `kg/trap`, `relative` (scaled to a reference period, e.g. mean = 1) |

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

The component file takes the same series and context columns as the stock file, but not reference points or `msy`, which belong to the stock.
Components have thresholds instead of reference points:

| Column | Label | Units | Description |
|---|---|---|---|
| `lower_threshold` | Lower threshold | same as its series | Lower status threshold (e.g. a salmon CU lower benchmark). |
| `upper_threshold` | Upper threshold | same as its series | Upper status threshold (e.g. a salmon CU upper benchmark). |
| `removal_threshold` | Removal threshold | same as `fishing_pressure` | Removal threshold for a component. |

Thresholds take the same fields as reference points (`basis`, `multiplier` and so on).

## Editing `assessment.yaml` by hand

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
| `basis` | reference points, thresholds | What it's based on; see the codes below. Required. |
| `multiplier` | reference points, thresholds | Required. A number, e.g. `0.4` for 0.4 B_MSY or `1` for F_MSY. |
| `period` | basis `mean` or `percentile` | First and last year, e.g. `[1985, 2009]`. |
| `percentile` | basis `percentile` | e.g. `50` for the median. |
| `spr` | basis `f_spr` | e.g. `40` for F40%. |

`basis` codes:

- Stock-size reference points and thresholds: `b_msy`, `b_msy_proxy`, `b0`, `b_recover`, `s_msy`, `s_gen`, `mean`, `percentile`, `other`
- Removal references and thresholds: `f_msy`, `u_msy`, `f_0.1`, `f_spr`, `other`

For `b_msy_proxy` and `other`, describe it in `notes`.

The rest of the file (`stock:`, `method:`, `meeting:`, `publications:` and so on) is explained in the comments of the YAML template the form downloads.
To check a file you've edited, load it back into the form with "Load existing YAML file".
