# Leading Goals Report Backlog

This backlog tracks the work required to build and release the single-page Leading Goals Power BI report. Keep IDs stable when updating items. Move completed work to the completed section with a short evidence note rather than deleting its history.

## Status values

Use `Not started`, `In progress`, `Blocked`, `Ready for validation` or `Complete`.

## Active backlog

| ID | Priority | Status | Item | Dependencies | Evidence or exit condition |
|---|---|---|---|---|---|
| LG-003 | P0 | Not started | Add the HR Reporting Month, Business Unit and Platform slicers with the specified configuration. | LG-002 | Visible slicers have the intended fields and selection behaviour. |
| LG-004 | P0 | Not started | Add and configure the three AYT counterpart slicers, sync each pair, test interactions and hide the AYT counterparts. | LG-003 | Hidden counterparts still filter intended AYT visuals and no stale label filters remain. |
| LG-005 | P0 | Not started | Add HR `Headcount` and AYT `Participation Rate` cards using published measures and source visual filters. | LG-002, LG-004 | Prototype renders and uses the intended measure/filter bindings. |
| LG-006 | P0 | Not started | Compare the prototype against the HR and AYT source reports and select the final metrics and page layout. | LG-005 | Final metric list, layout and rationale are recorded in the Master Context File. |
| LG-007 | P0 | Not started | Run Desktop acceptance checks for one-month and shared-Business-Unit context, including month changes. | LG-005 | Both cards match source reports for the tested context. |
| LG-008 | P0 | Not started | Run acceptance checks for Platform, Platform plus Business Unit, clearing both organisational slicers and Full Business results. | LG-004, LG-005 | Filter choices narrow and clear as intended, and both measures match source behaviour. |
| LG-009 | P0 | Not started | Run acceptance checks for an HR-only Business Unit or a Business Unit without AYT data, and for a month without an applicable survey. | LG-004, LG-005 | HR remains populated where available and AYT returns blank where expected. |
| LG-010 | P0 | Not started | Verify the month slicer cannot be cleared or multi-selected and confirm any reset action leaves one month selected. | LG-003, LG-004 | One month remains selected and both sections use it. |
| LG-012 | P1 | Not started | If label-based syncing fails, implement and validate key-based wrapper measures using `MonthCode`, `BusinessUnitCode` and `TREATAS`. | LG-007, LG-008, LG-009 | Wrapped measures pass repeated acceptance checks. |
| LG-013 | P0 | Not started | Check the published report with an intended viewer's access to the composite model and both source models. | LG-006, LG-007, LG-008, LG-009, LG-010 | Viewer can access the report and sees the validated results. |
| LG-014 | P1 | Not started | Add a Progression page with independent Base Month and Reporting Month selections. Show both values and the change for each reported figure under the same Business Unit and Platform filters. | LG-006 | Verify each endpoint against its source, including the AYT survey wave applicable to each month. Change equals Reporting Month minus Base Month. Percentage measures show change in percentage points. |
| LG-015 | P1 | Not started | Add a Trend page where viewers select a date range and choose a measure to plot. Use bookmarks to show the trend visual for the selected measure. | LG-006 | Verify that the graph follows the selected date range and that switching measures shows the correct visual and preserves the date range. |

## Completed

| ID | Completion evidence |
|---|---|
| LG-001 | Both published source models were selected in Power BI Desktop and returned results in the prototype. |
| LG-002 | The saved Leading Goals composite model contains DirectQuery connections to both `HR_Metrics` and `AYT_Metrics`. |
| LG-011 | The report uses an actuals-only HR Month list. The saved Month slicer applies `dim_HR_Calendar[HasActuals] = TRUE`. This decision was made independently of final metric selection. |
| LG-016 | The Leading Goals matrix displays Target and Leading Goal values. The user confirmed the Business Unit, Platform and Full Business lookups in Desktop. All ten Target and Leading Goal pairs for Customer Service NZ in October 2026 matched the Parquet source. |
| LG-017 | The starter annual targets and Leading Goals Parquet file was generated, uploaded to SharePoint and refreshed into the model. Read-back checks verified 1,080 unique rows, with 540 each for 2026 and 2027. |

## Working notes

- Keep `IncludeAYT = TRUE` scoped to applicable AYT result visuals.
- Preserve source question, category and response-option filters when reusing AYT visuals.
- If the same survey wave applies to several reporting months, repeated AYT results are expected and should be explained by the survey-wave label where needed.
- Record blockers with the affected ID, observed behaviour, attempted recovery and required decision.
