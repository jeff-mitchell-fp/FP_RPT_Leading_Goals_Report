
# Leading Goals Parquet generator handoff

- **Handoff ID** `LG-PARQUET-20261123`
- **Prepared** 2026-11-23
- **Origin** `RPT_Leading_Goals_Report`
- **Recipient** Parquet generator author
- **State** CSV examples available. Generator implementation, Parquet validation, and Power BI integration are pending.

Generate two Parquet files with the column names and types below. Use `MetricKey` to match the datasets. The report uses the metric list for grouping and display and the annual file for numeric targets and goals.

## 1. leading_goals_metrics.parquet

One row per metric or survey question. Use the [Leading Goals Metrics CSV](../external_documentation/leading_goals_metrics.csv) as the starter input.

| Column | Parquet type | Required | Meaning |
|---|---|---|---|
| `MetricKey` | `string` | Yes | Unique, stable identifier. Preserve existing keys when labels or groups change. |
| `DisplayName` | `string` | Yes | KPI label or survey question shown in the matrix. |
| `ImperativeKey` | `string` | Yes | Stable Shared Imperative identifier. |
| `SharedImperative` | `string` | Yes | Shared Imperative display label. |
| `ImperativeOrder` | `int32` | Yes | Positive display order across imperatives. |
| `GoalStatement` | `string` | Yes | Written goal statement for the imperative. |
| `MetricOrder` | `int32` | Yes | Positive display order within the imperative. |
| `Status` | `string` | Yes | `Active` or `Inactive`. Keep inactive rows so their mappings can be restored. |
| `ScoreType` | `string` | Yes | `KPI`, `Agreement`, or `Affirmative`. |
| `AYTItemCode` | `string` | Conditional | Required for `Agreement` and `Affirmative`. Null for `KPI`. |

For a given `ImperativeKey`, the label, order, and goal statement must be consistent across its rows. Each metric must have a distinct order within its imperative. Question codes must match the maintained AYT item reference at `AYT_Metrics_Generate_Staged_Files/src/references/item_reference.csv`. Display also depends on the AYT source's `IncludeLeadingGoals` flag. A new KPI that calls a different measure requires a corresponding report-model mapping.

## 2. annual_targets_and_goals.parquet

One row per **MetricKey × GoalYear × ScopeType × ScopeKey**. The [annual CSV example](../external_documentation/annual_targets_and_goals_example.csv) illustrates the output shape and contains sample values. This dataset supports `RPT_Leading_Goals_Report` backlog items [BL-016 and BL-017](Backlog.md).

| Column | Parquet type | Required | Meaning |
|---|---|---|---|
| `MetricKey` | `string` | Yes | Key present in the metric list. |
| `GoalYear` | `int32` | Yes | Four-digit calendar year. The selected Reporting Month determines this year. |
| `ScopeType` | `string` | Yes | `BusinessUnit`, `Platform`, or `FullBusiness`. |
| `ScopeKey` | `string` | Yes | `BusinessUnitCode` for `BusinessUnit` rows. Platform and Full Business use their respective identifiers when supplied. |
| `Target` | `double` | No | Approved target as a decimal proportion. Use null when unavailable. |
| `LeadingGoal` | `double` | No | Approved numeric Leading Goal as a decimal proportion. Use null when unavailable. |

### Business Unit names in the source CSV

The annual source CSV includes a `BusinessUnitName` column for data entry and review. It is required on `BusinessUnit` rows and can be blank on `Platform` and `FullBusiness` rows. This input-only column is additional to the fields shown in the annual example CSV.

Resolve each source name against the maintained Business Unit reference before exporting. The name must resolve to exactly one `BusinessUnitCode`. If the CSV also supplies a BU code, verify that it matches the name. Reject unmatched names, ambiguous names, and mismatched name/code pairs. Preserve the reference code as text and write it to `ScopeKey`. The Parquet schema carries the code in `ScopeKey` and excludes `BusinessUnitName`. The metric list has no Business Unit fields.

### Annual values

Store 80 percent as `0.80`. Preserve a genuine zero as `0.0`. Platform and Full Business may have their own approved rows. A missing value stays blank in the report. The annual example uses `EXAMPLE_` scope keys and dummy rates. Production output requires approved values and real scope keys.

## Generator acceptance checks

- Read both Parquet files back and verify their exact column names, data types, required fields, and allowed values.
- Check for duplicate metric and annual keys, inconsistent imperative metadata, and annual metric keys missing from the metric list.
- Verify that each BU output row uses the code from the maintained reference and that `BusinessUnitName` is absent from the Parquet schema.
- Reject sample scope keys in production. Check that rates fall between 0 and 1 inclusive and that nulls and genuine zeros survive the round trip.

## Open decisions

Confirm the maintained Business Unit lookup location, production output location, approved annual values, and the Platform and Full Business identifiers before generating production rows for those scopes. The generator and report integration are separate implementation steps.
