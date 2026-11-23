# Leading Goals Report — Page Plans

Version 0.1 | November 2026

## Purpose and authority

This file owns the current page specifications, visual bindings, report-local
interactions and scoped report validation for the Leading Goals Report.

The [Master Context File](Master%20Context%20File.md) owns report-wide scope,
source behaviour, key decisions and unresolved project questions. The
[Backlog](Backlog.md) owns approved open work and completion evidence. The
[build brief](HR%20Metrics%20and%20AYT%20Report%20Build%20Brief.md) remains the
initial requirements source. This file records the evolved two-page design.

Implementation inspection can confirm bindings, filters, layout and interaction
metadata. It does not establish numerical correctness, Power BI Desktop runtime
behaviour, publication or viewer access unless those are separately attributed.

## Report identity and page inventory

**Report display name:** Leading Goals Report

**Purpose:** Bring workforce and Ask Your Team measures together against the
annual Target and Leading Goal applicable to the selected organisation scope.

**Source models:** Published `HR_Metrics` and `AYT_Metrics` semantic models in
the People & Capability workspace, combined through the report's composite
model.

**Canvas:** 1280 × 720 pixels.

| Order | Page | Purpose |
|---:|---|---|
| 1 | Scores | Current reporting-month Score, Target and Leading Goal for the active metric catalog. |
| 2 | Trend | Monthly Score, Target and Leading Goal for one selected catalog measure. |

## Shared header and organisation controls

Both pages use a 95-pixel black header with:

- `Leading Goals Report` in white 12pt text.
- The page name in white bold 20pt text below the report name.
- Selected Platform and Business Unit header values where present.
- The white Fisher & Paykel logo at the upper-right.

Both pages expose HR-bound Platform, Business Unit and Reporting Month
slicers. Hidden AYT-bound counterparts remain in the corresponding sync groups
so each source model receives the selection through its own dimension.

| Control | Visible field | Hidden counterpart | Intended behaviour |
|---|---|---|---|
| Platform | `dim_HR_BusinessUnit[Platform]` | `dim_AYT_BusinessUnit[Platform]` | Clearable. Filters results in both source contexts. |
| Business Unit | `dim_HR_BusinessUnit[BusinessUnitName]` | `dim_AYT_BusinessUnit[BusinessUnitName]` | Clearable. Excludes the two Technologies Business Units listed below. |
| Reporting Month | `dim_HR_Calendar[CalMonthYear]` | `dim_AYT_Calendar[CalMonthYear]` | Strict single-select and restricted to `HasActuals = TRUE`. |

The visible and hidden Business Unit slicers exclude:

- `Fisher & Paykel Technologies`
- `Fisher & Paykel Technologies - China`

The exclusion is implemented with both `is not` conditions joined by `AND`.
The user confirmed in Power BI Desktop that both entries are absent on Scores
and Trend. This limits the selectable Business Unit values. It does not redefine
the unfiltered Full Business or Platform calculation scope.

Clearing both organisation slicers is intended to return Full Business. Target
and Leading Goal resolve organisation scope in this order: explicitly selected
Business Unit, selected Platform, then `FULL_BUSINESS` when neither is filtered.

## Metric and period behaviour

The report uses the active rows in `Leading Goals Metric Catalog`. Page visuals
use catalog status and `_LG Show Row` rather than a hard-coded visible-row
count. The catalog owns the changing metric inventory, Shared Imperative,
Goal Statement, display order, score type and AYT item mapping.

`Score` dispatches the selected catalog row to its published HR or AYT measure.
AYT question rows retain the catalog item code and approved score type. AYT
Overall Score remains outside report scope.

`Target` and `Leading Goal` use the selected metric, calendar year and resolved
organisation scope to read the annual target table. They display as one-decimal
percentages.

AYT reporting months resolve to the applicable survey wave. Several reporting
months may therefore show the same AYT result. A month without an applicable
survey is expected to return blank.

## Scores page

**Objective:** Show the current reporting-month Score, Target and Leading Goal
for every active catalog row under one organisation context.

### Page specification

The page contains the shared header and organisation controls followed by one
matrix across the main content area.

| Matrix role | Binding |
|---|---|
| Row 1 | `Leading Goals Metric Catalog[SharedImperative]` |
| Row 2 | `Leading Goals Metric Catalog[GoalStatement]` |
| Row 3 | `Leading Goals Metric Catalog[DisplayName]` |
| Value 1 | `_LG_Measures[Score]`, displayed as Current |
| Value 2 | `_LG_Measures[Target]` |
| Value 3 | `_LG_Measures[Leading Goal]` |

The matrix applies catalog `Status`, `_LG Show Row` and row-field filters. The
header includes `_Selected Platform Name` and `_Selected BU Name` cards.

Two earlier AYT question tables remain hidden in the report definition. They
are not part of the visible Scores layout and should not be treated as current
page requirements.

### Evidence and limits

- The user confirmed the Business Unit, Platform and Full Business Target and
  Leading Goal lookups in Desktop.
- All ten Target and Leading Goal pairs for Customer Service NZ in October 2026
  matched the Parquet source.
- Report-definition matrix bindings and filters were inspected on 24 November 2026.
- Current Score values have not been independently reconciled to both source
  reports across all acceptance scenarios.

## Trend page

**Objective:** Show one selected catalog measure over an independent actuals
date range, with its annual Target and Leading Goal, while retaining the page's
Reporting Month snapshot.

### Page specification

The page contains:

- The shared header and organisation controls.
- A Between slicer on `dim_HR_Calendar[MonthStartDate]`, headed **Chart Date
  Range**, with the slider hidden and `HasActuals = TRUE`.
- One wide, single-select **Measure** dropdown on
  `Leading Goals Metric Catalog[DisplayName]`, filtered by `_LG Show Row = 1`.
- A **Reporting Month Score** card bound to `_LG_Measures[Score]`.
- A monthly line chart using `dim_HR_Calendar[CalMonthYearShort]` on the axis
  and `_LG_Measures[Trend Score]`, `_LG_Measures[Target]` and
  `_LG_Measures[Leading Goal]` as the series.
- A note explaining that AYT results may repeat across months covered by the
  same survey wave.

The page has no Shared Imperative slicer. The Measure dropdown presents the
eligible catalog rows directly.

### Measure and period behaviour

`Trend Score` reads the month on the HR chart axis. HR catalog measures use
that month directly. For AYT catalog measures, `TREATAS` passes the axis
`MonthCode` to `dim_AYT_Calendar[MonthCode]`, allowing the source measure to
resolve the applicable survey wave for each point.

The Reporting Month and Chart Date Range are intentionally independent:

- Reporting Month filters the snapshot card and does not filter the trend chart
  or Chart Date Range.
- Chart Date Range filters the trend chart and does not filter the Reporting
  Month slicers or snapshot card.
- Platform and Business Unit filter both the snapshot card and trend chart.
- Measure filters both the snapshot card and trend chart while leaving the
  organisation and period controls unchanged.
- Chart selections do not alter the slicers or snapshot card.

Target and Leading Goal are evaluated for each axis month's calendar year.
Months outside the available annual target data can display Score without a
Target or Leading Goal.

### Evidence and limits

- The user supplied a Desktop screenshot showing Score, Target and Leading Goal
  rendered together and confirmed the final selector design.
- After the user reported refreshing the model, the user confirmed the Finance
  Purpose, Values & Strategy Trend screenshot: Target was 65% in 2023, 87% in
  2024 and 65% in 2025–2026; Leading Goal remained 90%. This validates that
  selection's annual Target transitions only.
- Report-definition bindings, visual filters and explicit interactions were inspected on
  24 November 2026.
- Switching between representative HR and AYT measures while preserving date
  and organisation selections remains to be confirmed in Desktop.
- Other Trend values, annual line changes and blank-month behaviour remain to
  be reconciled against the source reports and annual target source.

## Validation still required

The following acceptance evidence remains open unless recorded elsewhere:

- One reporting month and one Business Unit present in both source models.
- Platform only, Platform plus Business Unit, clearing Business Unit and
  switching Platform.
- Full Business with both organisation slicers clear.
- A Business Unit or reporting month without an applicable AYT result.
- Reporting Month force-selection and reset behaviour.
- Trend Measure switching across HR and AYT metrics while retaining Chart Date
  Range, Platform and Business Unit selections.
- Trend values and Target/Leading Goal lines at representative months,
  including a calendar-year boundary.
- Published report access using an intended viewer account with access to the
  composite model and both source models.

