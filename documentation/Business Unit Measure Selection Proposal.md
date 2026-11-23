# Business Unit Measure Selection Proposal

**Status** Parked for future consideration.

## Feature

When a viewer selects a Business Unit, Scores would show the measures that
unit tracks for the selected reporting year. The annual targets and goals file
would list the tracked measures for each unit and year. Trend could use the
same eligibility rule if its Measure dropdown also needs to narrow.

## Recommended implementation

1. Keep the existing annual file grain of one row per `MetricKey × GoalYear ×
   ScopeType × ScopeKey`. For Business Unit rows, use `BusinessUnitCode` as
   `ScopeKey`. The presence of a row means the unit tracks that measure
   for that year, including when Target or Leading Goal is blank. An absent
   row means the measure is outside the configured list. Validate key
   uniqueness and check metric and Business Unit codes against their
   maintained references before loading.
2. Add a report-local eligibility measure. For each Scores matrix row, check
   whether the annual table contains the catalog row's `MetricKey`, the
   Reporting Month's calendar year, and the selected Business Unit's scope
   type and code. Reuse the existing organisation scope helpers. For Platform
   and Full Business, retain the current catalog-wide list in the first
   version.
3. Filter the Scores matrix by the new eligibility measure alongside
   `_LG Show Row` and the existing catalog filters. Today a missing annual
   row can leave Target and Leading Goal blank while Score still returns a
   value. The eligibility filter changes which rows appear. It can use the
   existing model relationships and value measures.
4. If Trend is included, enable the Business Unit and Reporting Month
   interactions with the Measure dropdown and apply the same eligibility
   rule there. Use Reporting Month to decide which measures can be selected
   and Chart Date Range to determine which months to plot. This lets a
   currently tracked measure show earlier scores. Check whether Power BI
   retains a selection that becomes ineligible after switching units, and
   agree what the dropdown, chart and snapshot should display in that case.

The annual file can assign measures already supported by the report. A new KPI
requires a catalog entry and a report-model mapping.

## Decisions to revisit

| Decision | Suggested starting point |
|---|---|
| Measure order | Keep the catalog's `MetricOrder`. Each unit's subset follows the same relative order, and gaps are fine. If units need different ranks for the same measure, scope that separately. Power BI's Sort by column requires each displayed value to map to one sort value, so a rank that varies by unit cannot directly sort the shared catalog labels. |
| Platform and Full Business | Keep their current lists. If they need configurable subsets, use explicit rows at each scope and agree who maintains them. A union of Business Unit lists would require a separate rule. |
| Missing Business Unit or year | Confirm how viewers should see that a unit has no measures configured for the selected year. An incomplete file and an intentionally empty list can look identical. Agree whether the import needs a completeness check or whether the report should display a message. |
| Trend scope | Implement Scores first. Confirm whether Trend should narrow too, whether earlier scores should remain visible, and how to handle a selected measure that becomes ineligible. |

## Validation if implemented

- Test two Business Units with overlapping but different lists. Compare the
  displayed rows and their order with the annual file.
- Test a tracked measure with blank benchmarks, including a month when its
  source Score is also blank. Check that the tracked row remains visible if
  that is the agreed behaviour. Verify an untracked measure is excluded even
  when its source Score exists.
- Change Reporting Month across a year boundary, test a unit with no configured
  rows, and clear Business Unit to return to Platform or Full Business.
- If Trend is included, switch between units with different measure lists and
  verify the chart and snapshot when the prior selection becomes ineligible.
  Check that changing Chart Date Range does not change the eligible list and
  that earlier scores display as agreed.

The proposal is based on inspection of the report and model definitions.
The proposed behaviour requires Power BI Desktop validation.

## References

- [Leading Goals Report Page Plans](<Leading Goals Report Page Plans.md>)
- [Annual file contract](handoff_parquet_generator_leading_goals_20261123.md)
