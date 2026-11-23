# Leading Goals Report — Master Context File

## Purpose

This file is the working context for AI-assisted development of the Leading Goals Power BI report. It records the agreed scope, authoritative references, current decisions, open questions and validation state. Update it when a decision changes the project direction or when validation produces evidence that future work needs.

## Current objective

Build a new single-page Power BI Desktop report that combines workforce metrics from `HR_Metrics` with Ask Your Team metrics from `AYT_Metrics` for the People & Capability workspace.

## Scope

In scope:

- A composite model using `HR_Metrics` as the starting model and `AYT_Metrics` added through Power BI semantic models.
- Three visible slicers: Reporting Month, Business Unit and Platform.
- Synced AYT slicer counterparts that are tested and then hidden.
- Reuse of published measures and the applicable metric-specific visual filters from the source reports.
- A prototype with at least HR `Headcount` and AYT `Participation Rate`.
- Desktop acceptance checks covering month, Business Unit, Platform, Full Business, missing AYT data and months without an applicable survey.
- A published-viewer access check before distribution.

Out of scope:

- AYT Overall Score.
- Final metric selection and final page layout until the source reports have been compared.
- Key-based DAX wrappers unless synced slicer testing shows that label-based propagation is insufficient.

## Source and period behaviour

The initial guidance is in [HR Metrics and AYT Report Build Brief](HR%20Metrics%20and%20AYT%20Report%20Build%20Brief.md). It is the primary project brief.

Connect to `HR_Metrics` and `AYT_Metrics` in the **People & Capability** workspace. The author needs Build permission on both models.

HR uses the selected month for workforce measures. AYT uses the selected month to resolve the applicable survey wave through its existing month-to-survey mapping. Several months may show the same survey result. AYT returns blank when no survey applies. The AYT section should show the survey name or wave where needed to make the observation period clear.

The saved models align on integer `MonthCode` (`YYYYMM`) and `BusinessUnitCode`. Platform is an attribute of Business Unit in both models.

## Required slicers

| Visible slicer | Field in each model | Initial configuration |
|---|---|---|
| Reporting Month | `dim_Calendar[CalMonthYear]` | HR visible slicer uses List, Single select and Force selection. Start with a known reporting month. Confirm whether HR `HasActuals = TRUE` is required. |
| Business Unit | `dim_BusinessUnit[BusinessUnitName]` | Clearable to support Full Business. Test narrowing after Platform selection. |
| Platform | `dim_BusinessUnit[Platform]` | Clearable. Test interactions with an existing Business Unit selection. |

Each visible slicer has an AYT counterpart. Each pair has its own Sync slicers group, passes filter changes to the other slicer, remains bound to its own model field, and is tested before the AYT counterpart is hidden.

## Initial visual direction

Start with two cards:

- HR `Headcount`
- AYT `Participation Rate`

Carry over source visual filters. Apply AYT `IncludeAYT = TRUE` to applicable result visuals and preserve any required question, category or response-option filters from the original AYT visual.

Compare the prototype with both source reports before adding more visuals. Final metric set, layout and explanatory labels remain open until that comparison is complete.

## Contingency for slicer mismatch

If synced slicers fail because labels do not match or a selection silently clears or persists in AYT, use report-local wrapper measures. Pass the selected HR `MonthCode` to the AYT calendar with `TREATAS`. When Business Unit or Platform is selected, pass the effective HR `BusinessUnitCode` values to the AYT Business Unit dimension. When both organisation slicers are clear, omit the Business Unit filter so the AYT measure keeps its Full Business scope. Disable corresponding hidden-slicer interactions on wrapped visuals so stale label filters do not intersect the key filters. Repeat all acceptance checks for wrapped measures.

## Authoritative references

- [HR Metrics and AYT Report Build Brief](HR%20Metrics%20and%20AYT%20Report%20Build%20Brief.md)
- `RPT_HR_BU_Report/documentation/BU Report Page Plans.md`
- `SM_AYT_Model/documentation/AYT Model Design Document.md` section 2.6
- `SM_AYT_Model/documentation/AYT Measure Library.md` section 3 and later measure-family sections
- `RPT_AYT_Report/documentation/AYT Report Page Plans.md`

The cross-repository references are paths in named source repositories. They are not assumed to be available in this repository.

## Decisions and open questions

| Topic | Status | Decision or next evidence |
|---|---|---|
| Report page count | Agreed initial scope | One page. |
| AYT Overall Score | Agreed | Excluded. |
| Initial prototype | Agreed | Headcount and Participation Rate cards. |
| Final metric set | Open | Compare source reports, then record the selected measures and rationale. |
| Final layout | Open | Select after prototype comparison. |
| HR actuals-only month list | Open | Confirm whether the page follows the BU report's `HasActuals = TRUE` month list. |
| Cross-model slicer propagation | Open | Test label-based synced slicers first. Use key-based wrappers only if required. |
| Known reporting month | Open | Choose and record a month available in both source contexts before Desktop testing. |
| Published viewer access | Open | Check intended viewer access to the composite model and both source models before distribution. |

## Validation state

No Power BI Desktop or published-viewer validation has been performed at initial setup. Acceptance evidence should be recorded against the backlog items and reflected here when it changes scope, design or implementation.
