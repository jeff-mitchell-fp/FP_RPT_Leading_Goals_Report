# Leading Goals Report Backlog

This backlog tracks the work required to build, validate and release the two-page Leading Goals Power BI report. Keep IDs stable when updating items. Move completed work to the completed section with a short evidence note rather than deleting its history.

## Status values

Use `Not started`, `In progress`, `Blocked`, `Ready for validation` or `Complete`.

## Identifier convention

Use local IDs `BL-001` onward. IDs `BL-001` through `BL-018` have been issued;
the next available ID is `BL-019`. Never reuse an issued ID.

## Active backlog

| ID | Priority | Status | Item | Dependencies | Evidence or exit condition |
|---|---|---|---|---|---|
| BL-012 | P1 | Not started | If label-based organisation syncing fails, implement and validate key-based wrappers using `BusinessUnitCode` and `TREATAS`, while retaining the Trend page's month transfer. | BL-007, BL-008, BL-009 | Wrapped measures pass repeated acceptance checks. |
| BL-013 | P0 | Not started | After publication and before wider sharing, the user will check the live report with an intended viewer's access to the composite model and both source models. | BL-006, BL-007, BL-008, BL-009, BL-010 | Viewer can access the report and sees the validated results. |
| BL-014 | P1 | Not started | Parked until feedback on the current report: add a Progression page with independent Base Month and Reporting Month selections. Show both values and the change for each reported figure under the same Business Unit and Platform filters. | BL-006 | Verify each endpoint against its source, including the AYT survey wave applicable to each month. Change equals Reporting Month minus Base Month. Percentage measures show change in percentage points. |
| BL-018 | P1 | Not started | Investigate dynamically excluding Business Units with fewer than five people from report selections. | BL-003, BL-004 | Agree the headcount definition, period and blank handling, Trend behaviour, cross-model sync and any confidentiality requirements; assess implementation and regression checks before replacing fixed exclusions. |

## Completed

| ID | Completion evidence |
|---|---|
| BL-001 | Both published source models were selected in Power BI Desktop and returned results in the prototype. |
| BL-002 | The saved Leading Goals composite model contains DirectQuery connections to both `HR_Metrics` and `AYT_Metrics`. |
| BL-003 | The user confirmed Desktop slicer acceptance, including the January 2023 lower bound and Technologies exclusions. The user subsequently confirmed the Distributor EU, Distributor China, Manufacturing MX and China ME exclusions and matching figures; the updated report was published. |
| BL-004 | The user confirmed remaining sync and blank-result checks; switching to eCommerce cleared the previous AYT result. |
| BL-005 | The user confirmed Scores comparisons and remaining checks. BU-level Average Score is unavailable in the AYT report; the method or disposition for its subsequent confirmation was not specified. |
| BL-006 | The final two-page presentation is recorded in Leading Goals Report Page Plans. Scores uses the active catalog matrix; Trend uses a single Measure dropdown and plots Score, Target and Leading Goal. Report-definition inspection and user screenshots confirm the layout. |
| BL-007 | Customer Service NZ results matched for October and September 2026; the user confirmed remaining shared-BU and month checks. |
| BL-008 | The user confirmed matching results for Australia ME, Customer Service AU and the remaining Platform and Full Business scenarios; BU choices narrowed correctly. |
| BL-009 | The user confirmed remaining blank-result and source checks; switching from a populated AYT BU to eCommerce cleared AYT values. |
| BL-010 | The user confirmed the remaining Reporting Month selection and reset checks. |
| BL-011 | The report uses an actuals-only HR Month list. The saved Month slicer applies `dim_HR_Calendar[HasActuals] = TRUE`. This decision was made independently of final metric selection. |
| BL-015 | The user confirmed Trend interactions and remaining source and annual-line checks. Finance January 2023 Voluntary Turnover Rolling 12 matched the source at 12.6% in the card and chart. |
| BL-016 | The Leading Goals matrix displays Target and Leading Goal values. The user confirmed the Business Unit, Platform and Full Business lookups in Desktop. All ten Target and Leading Goal pairs for Customer Service NZ in October 2026 matched the Parquet source. |
| BL-017 | The starter annual targets and Leading Goals Parquet file was generated, uploaded to SharePoint and refreshed into the model. Read-back checks verified 1,080 unique rows, with 540 each for 2026 and 2027. |

## Working notes

- Keep `IncludeAYT = TRUE` scoped to applicable AYT result visuals.
- Preserve source question, category and response-option filters when reusing AYT visuals.
- If the same survey wave applies to several reporting months, repeated AYT results are expected and should be explained by the survey-wave label where needed.
- Current page specifications and scoped validation evidence are maintained in [Leading Goals Report Page Plans](Leading%20Goals%20Report%20Page%20Plans.md).
- Record blockers with the affected ID, observed behaviour, attempted recovery and required decision.
