# Leading Goals Report — Master Context File

## Purpose

This file summarises the report-wide scope, source behaviour, key decisions and unresolved project questions for the Leading Goals Report.

Current page specifications, visual bindings, interactions and scoped report validation are owned by [Leading Goals Report Page Plans](Leading%20Goals%20Report%20Page%20Plans.md). The [Backlog](Backlog.md) owns approved open work and completion evidence. The build brief remains the initial requirements source.

## Current objective

Build and validate a two-page Power BI Desktop report that combines workforce metrics from `HR_Metrics` with Ask Your Team metrics from `AYT_Metrics` for the People & Capability workspace.

## Scope

The current report comprises:

- **Scores**, showing the selected reporting month's Score, Target and Leading Goal for the active metric catalog.
- **Trend**, showing monthly Score, Target and Leading Goal for one selected catalog measure over an independent Chart Date Range.

Both pages use visible Reporting Month, Business Unit and Platform slicers with hidden synced AYT counterparts. Reuse published source measures and their applicable visual filters. AYT Overall Score remains out of scope.

## Source and period behaviour

The initial guidance is in [HR Metrics and AYT Report Build Brief](HR%20Metrics%20and%20AYT%20Report%20Build%20Brief.md). It is the primary project brief.

Connect to `HR_Metrics` and `AYT_Metrics` in the **People & Capability** workspace. The author needs Build permission on both models.

HR uses the selected month for workforce measures. AYT uses the selected month to resolve the applicable survey wave through its existing month-to-survey mapping. Several months may show the same survey result. AYT returns blank when no survey applies. The AYT section should show the survey name or wave where needed to make the observation period clear.

The saved models align on integer `MonthCode` (`YYYYMM`) and `BusinessUnitCode`. Platform is an attribute of Business Unit in both models. Label-based synced slicers remain the organisation-filtering implementation. The Trend Score measure uses `MonthCode` with `TREATAS` only to pass each HR chart-axis month to the AYT calendar.

Targets and Leading Goals are annual values selected by metric, calendar year and organisation scope. Organisation scope resolves in this order: explicitly selected Business Unit, selected Platform, then Full Business.

## Page specifications

Page layout, slicer configuration, measure bindings, filter interactions and page-level validation evidence are maintained in [Leading Goals Report Page Plans](Leading%20Goals%20Report%20Page%20Plans.md).

If label-based organisation syncing fails an acceptance scenario, assess key-based wrappers using `BusinessUnitCode`. Keep Full Business unrestricted and disable intersecting stale hidden-slicer filters before validating any wrapper implementation.

## Authoritative references

- [Leading Goals Report Page Plans](Leading%20Goals%20Report%20Page%20Plans.md)
- [HR Metrics and AYT Report Build Brief](HR%20Metrics%20and%20AYT%20Report%20Build%20Brief.md)
- `RPT_HR_BU_Report/documentation/BU Report Page Plans.md`
- `SM_AYT_Model/documentation/AYT Model Design Document.md` section 2.6
- `SM_AYT_Model/documentation/AYT Measure Library.md` section 3 and later measure-family sections
- `RPT_AYT_Report/documentation/AYT Report Page Plans.md`

The cross-repository references are paths in named source repositories. They are not assumed to be available in this repository.

## Decisions and open questions

| Topic | Status | Decision or next evidence |
|---|---|---|
| Report page count | Agreed | Two pages: Scores and Trend. |
| AYT Overall Score | Agreed | Excluded. |
| Metric presentation | Agreed | Scores uses the active catalog matrix. Trend uses one single-select Measure dropdown. |
| Trend comparison | Agreed | Plot Score, Target and Leading Goal. Reporting Month controls the snapshot; Chart Date Range controls the chart. |
| HR actuals-only months | Agreed | Reporting Month and Chart Date Range apply `HasActuals = TRUE`. |
| Technologies scope | Agreed | Exclude Fisher & Paykel Technologies and Fisher & Paykel Technologies - China from the Business Unit slicers on both pages. This does not redefine Full Business calculations. |
| Cross-model slicer propagation | Built, validation open | Hidden synced slicers are present. Complete the remaining acceptance scenarios before deciding whether organisation key wrappers are required. |
| Published viewer access | Open | The user will check intended viewer access to the live report, composite model and both source models after publication and before wider sharing. |
| Progression page | Parked | Reconsider after feedback on the current two-page report; tracked as BL-014. |
| Annual target maintenance | User-managed | The user will separately write a script that generates Parquet from the manually maintained CSV source. |
| 2027 starter targets | Open | Confirm whether the starter targets are approved for production. |

## Validation state

The user confirmed the Target and Leading Goal lookups for Business Unit, Platform and Full Business. All ten Customer Service NZ pairs for October 2026 matched the Parquet source. The user also confirmed that the two Technologies entries are absent from the Business Unit slicers on both pages.

Report-definition inspection confirms the two pages, current bindings, filters, page headers and Trend interactions. Remaining Desktop and published-viewer checks are listed in Page Plans and the backlog. No published-viewer validation has been recorded.

The annual seed and generated Parquet cover 2023–2027; the user reported
refreshing the model. The user confirmed one Finance Purpose, Values & Strategy
Trend annual Target transition against the CSV. Page Plans records the
specific evidence and remaining validation.

## Next session

Perform Desktop acceptance checks for Scores and Trend, including HR and AYT
source comparisons, slicer and blank-result scenarios, Trend interactions, and
representative Target and Leading Goal checks at Business Unit, Platform and
Full Business scope. Confirm whether the 2027 starter targets are approved for
production.
