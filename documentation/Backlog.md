# Leading Goals Report Backlog

This backlog tracks the work required to build, validate and release the two-page Leading Goals Power BI report. Keep IDs stable when updating items. Move completed work to the completed section with a short evidence note rather than deleting its history.

## Status values

Use `Not started`, `In progress`, `Blocked`, `Ready for validation` or `Complete`.

## Identifier convention

Use local IDs `BL-001` onward. IDs `BL-001` through `BL-017` have been issued;
the next available ID is `BL-018`. Never reuse an issued ID.

## Active backlog

| ID | Priority | Status | Item | Dependencies | Evidence or exit condition |
|---|---|---|---|---|---|
| BL-003 | P0 | Ready for validation | Validate the implemented HR Reporting Month, Business Unit and Platform slicers on Scores and Trend. The Business Unit slicers exclude both Technologies entries. | BL-002 | Visible slicers have the intended fields, actuals restriction, exclusions and selection behaviour in all acceptance scenarios. |
| BL-004 | P0 | Ready for validation | Validate the three hidden AYT counterpart slicers, sync groups and interactions on both pages. | BL-003 | Hidden counterparts filter the intended AYT results and no stale or unrestricted AYT context remains. |
| BL-005 | P0 | Ready for validation | Validate the Scores matrix for the active catalog using `Score`, `Target` and `Leading Goal`, including required source filters. | BL-002, BL-004 | Representative HR and AYT rows match their source reports and the matrix follows the approved catalog. |
| BL-007 | P0 | Not started | Run Desktop acceptance checks for one-month and shared-Business-Unit context, including month changes. | BL-005 | Representative Scores and Trend results match their source reports for the tested context. |
| BL-008 | P0 | Not started | Run acceptance checks for Platform, Platform plus Business Unit, clearing both organisational slicers and Full Business results. | BL-004, BL-005 | Filter choices narrow and clear as intended, and representative page results match source behaviour. |
| BL-009 | P0 | Not started | Run acceptance checks for an HR-only Business Unit or a Business Unit without AYT data, and for a month without an applicable survey. | BL-004, BL-005 | HR remains populated where available and AYT returns blank where expected. |
| BL-010 | P0 | Not started | Verify the Reporting Month slicer cannot be cleared or multi-selected and confirm any reset action leaves one month selected. | BL-003, BL-004 | One reporting month remains selected and both source contexts use it. |
| BL-012 | P1 | Not started | If label-based organisation syncing fails, implement and validate key-based wrappers using `BusinessUnitCode` and `TREATAS`, while retaining the Trend page's month transfer. | BL-007, BL-008, BL-009 | Wrapped measures pass repeated acceptance checks. |
| BL-013 | P0 | Not started | After publication and before wider sharing, the user will check the live report with an intended viewer's access to the composite model and both source models. | BL-006, BL-007, BL-008, BL-009, BL-010 | Viewer can access the report and sees the validated results. |
| BL-014 | P1 | Not started | Parked until feedback on the current report: add a Progression page with independent Base Month and Reporting Month selections. Show both values and the change for each reported figure under the same Business Unit and Platform filters. | BL-006 | Verify each endpoint against its source, including the AYT survey wave applicable to each month. Change equals Reporting Month minus Base Month. Percentage measures show change in percentage points. |
| BL-015 | P1 | Ready for validation | Validate the Trend page with its single-select Measure dropdown, independent Chart Date Range, Reporting Month Score card, and Score, Target and Leading Goal series. | BL-006 | Representative HR and AYT measures match their sources; Measure switching preserves date and organisation selections; Reporting Month and Chart Date Range remain independent; annual lines behave correctly across years. |

## Completed

| ID | Completion evidence |
|---|---|
| BL-001 | Both published source models were selected in Power BI Desktop and returned results in the prototype. |
| BL-002 | The saved Leading Goals composite model contains DirectQuery connections to both `HR_Metrics` and `AYT_Metrics`. |
| BL-006 | The final two-page presentation is recorded in Leading Goals Report Page Plans. Scores uses the active catalog matrix; Trend uses a single Measure dropdown and plots Score, Target and Leading Goal. Report-definition inspection and user screenshots confirm the layout. Numerical and interaction acceptance remains under BL-003–005 and BL-015. |
| BL-011 | The report uses an actuals-only HR Month list. The saved Month slicer applies `dim_HR_Calendar[HasActuals] = TRUE`. This decision was made independently of final metric selection. |
| BL-016 | The Leading Goals matrix displays Target and Leading Goal values. The user confirmed the Business Unit, Platform and Full Business lookups in Desktop. All ten Target and Leading Goal pairs for Customer Service NZ in October 2026 matched the Parquet source. |
| BL-017 | The starter annual targets and Leading Goals Parquet file was generated, uploaded to SharePoint and refreshed into the model. Read-back checks verified 1,080 unique rows, with 540 each for 2026 and 2027. |

## Working notes

- Keep `IncludeAYT = TRUE` scoped to applicable AYT result visuals.
- Preserve source question, category and response-option filters when reusing AYT visuals.
- If the same survey wave applies to several reporting months, repeated AYT results are expected and should be explained by the survey-wave label where needed.
- Current page specifications and scoped validation evidence are maintained in [Leading Goals Report Page Plans](Leading%20Goals%20Report%20Page%20Plans.md).
- Record blockers with the affected ID, observed behaviour, attempted recovery and required decision.
