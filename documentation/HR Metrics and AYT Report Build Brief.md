# HR Metrics and AYT Report Build Brief

Build a new single-page Power BI Desktop report that brings workforce and Ask Your Team (AYT) metrics together. The page has three visible slicers for Reporting Month, Business Unit and Platform. Viewers select one reporting month at a time. Reuse the published measures behind the existing visuals. Select the final metrics and layout in the new project. AYT Overall Score is out of scope.

## Source models and period behaviour

Connect to `HR_Metrics` and `AYT_Metrics` in the **People & Capability** workspace. Start with `HR_Metrics`, select **Make changes to this model → Add a local model**, and then add `AYT_Metrics` through **Get data → Power BI semantic models**. Use a composite model in the new report. The author needs Build permission on both models. [Microsoft composite model setup and permissions](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-composite-models)

HR Metrics uses the selected month for its workforce measures. AYT uses the selected month to resolve the applicable survey wave through its existing month-to-survey mapping. Several months can show the same survey result. AYT returns blank when no survey applies. Show the survey name or wave on the AYT section where needed to make the observation period clear.

## Desktop setup

1. Add the three visible slicers using the HR Metrics fields below. Add one AYT counterpart for each field on the same page. Give each pair its own group under **View → Sync slicers → Advanced options**. Enable **Sync filter changes to other slicers** on both members of each pair, keep each bound to its own source field, and hide the AYT counterparts after testing. Check that each hidden slicer still filters its intended visuals. [Microsoft guidance on synced slicers in composite models](https://learn.microsoft.com/en-us/power-bi/guidance/composite-model-guidance#use-sync-slicers) · [Sync separate slicers](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-slicers#sync-separate-slicers)

   | Visible slicer | Field in each model | Configuration |
   |---|---|---|
   | Reporting Month | `dim_Calendar[CalMonthYear]` | On the visible List slicer, enable **Single select** and **Force selection**. Start with a known reporting month. Apply HR `HasActuals = TRUE` to the visible slicer if the page follows the BU report's actuals-only month list. |
   | Business Unit | `dim_BusinessUnit[BusinessUnitName]` | Keep the selection clearable for Full Business. Verify the BU choices narrow when Platform is selected. |
   | Platform | `dim_BusinessUnit[Platform]` | Keep the selection clearable. Verify what happens when a BU is already selected and the viewer changes Platform. |

   [Microsoft List slicer settings](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-list-slicer)

2. Start with two cards, such as HR **Headcount** and AYT **Participation Rate**. Bind them to their published measures. Carry over the metric-specific visual filters from the source reports, including AYT `IncludeAYT = TRUE` on result visuals where used. Keep that filter scoped to the applicable AYT visuals. For further AYT visuals, preserve any required question, category or response-option filter from the original visual.

3. Compare the prototype with the two source reports before adding the remaining visuals. Synced slicers pass selections by field value. Check the available labels and their meaning in both models, including whether one label refers to different units or whether the same BU has different names. The saved models align on integer `MonthCode` (`YYYYMM`) and `BusinessUnitCode`. Platform is an attribute of Business Unit in both models.

## Desktop acceptance checks

| Scenario | Expected result |
|---|---|
| One month and a BU present in both models | Both cards match their source reports under the equivalent month and BU context. Changing the month updates both models' context. An AYT result can repeat when the same wave applies. |
| Platform only, then Platform with a BU | Both cards match their source reports at the applicable scope. Platform narrows the visible BU choices. Test clearing the BU and switching Platform. |
| Both organisational slicers cleared | Each measure returns its source model's Full Business result, including AYT's existing treatment of unattributed responses where applicable. |
| HR-only BU, or a BU without AYT data in the selected wave | The HR result follows the selected BU. The AYT card shows blank. Test this immediately after selecting a BU with AYT data to catch a stale or unrestricted result. |
| HR reporting month without an applicable survey | HR shows the selected month's result where available. AYT shows blank. |
| Attempt to clear Month or select multiple months | Check that the configured controls and any reset action leave one month selected and both sections use it. |

If a synced pair fails because of unmatched labels or because a selection silently clears or persists in AYT, use key-based DAX for the affected AYT measures. Keep the HR slicers and measures. In a report-local wrapper, apply the selected HR `MonthCode` to the AYT calendar with `TREATAS`. When a BU or Platform is selected, pass the effective HR `BusinessUnitCode` values to the AYT BU dimension. With both organisation slicers cleared, omit that BU filter so the AYT measure retains its Full Business scope. Disable the corresponding hidden slicer interactions on wrapped visuals so stale label filters cannot intersect the key filters. Repeat the acceptance checks for any wrapped measures. [Microsoft DAX TREATAS reference](https://learn.microsoft.com/en-us/dax/treatas-function-dax)

After Desktop acceptance, follow the new project's release process. Before distribution, check the published report using an intended viewer's access to the composite model and both source models.

## Project references

Paths below are relative to the named source repositories so the brief can travel independently.

- `RPT_HR_BU_Report/documentation/BU Report Page Plans.md` records the HR slicer fields, interactions and existing visual bindings.
- `SM_AYT_Model/documentation/AYT Model Design Document.md` §2.6 records the cross-model key alignment.
- `SM_AYT_Model/documentation/AYT Measure Library.md` §3 records the reporting-month and survey behaviour and describes the published measure families in later sections.
- `RPT_AYT_Report/documentation/AYT Report Page Plans.md` records the AYT visual bindings and filters to preserve when reusing a visual.

This brief is based on saved definitions, project documentation and Microsoft guidance. The combined report and synced slicers still require Desktop acceptance and published viewer checks.
