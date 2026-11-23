# Leading Goals Power BI Project

## Start here

Read these files before making project decisions:

1. [Master Context File](documentation/Master%20Context%20File.md)
2. [HR Metrics and AYT Report Build Brief](documentation/HR%20Metrics%20and%20AYT%20Report%20Build%20Brief.md)
3. [Backlog](documentation/Backlog.md)

The build brief is the initial requirements source. The Master Context File summarises the current project state and records decisions that must remain visible to future agents. The backlog is the execution record.

## Project objective

Build a new single-page Power BI Desktop report that brings workforce and Ask Your Team (AYT) metrics together for the Leading Goals project. The initial report uses a composite model built from the published `HR_Metrics` and `AYT_Metrics` semantic models in the People & Capability workspace.

## Working rules

- Preserve published measures and source-report visual filters when reusing them.
- Keep AYT Overall Score out of scope unless the user explicitly changes scope.
- Treat the visible Reporting Month, Business Unit and Platform slicers, their synced AYT counterparts, and the Desktop acceptance scenarios as core requirements.
- Record assumptions and unresolved decisions in the Master Context File or backlog before implementing dependent work.
- Do not claim Power BI Desktop or published-viewer validation unless it has actually been performed.
- Coordinate any report-file changes with the user's saved Power BI Desktop state and pending Power Query changes.
- Keep credentials, tokens and sensitive people data out of project files and chat.

## Change boundaries

- Ask before changing Markdown files outside the approved project documentation scope.
- Ask before changing TMDL or `.pbip` files, deleting or moving files, publishing reports, refreshing external resources, or sending communications.
- The user performs Git state changes. Do not commit, stage, push, pull, merge, rebase, reset, restore, checkout, or change branches.

## Validation expectations

Separate code or document inspection from Power BI Desktop validation. For report work, identify the affected measures, relationships, slicer interactions, visual filters and acceptance scenarios. Record what was checked, what was performed in Desktop, and what remains unverified.

## Current status

The project is at initial setup. No report model, report page, metric selection or layout decision has been finalised.
