# Leading Goals Target Extraction Review

## Purpose

Review exceptions for goal years 2023 onward from the one-off extraction of the PowerPoint files in `external_documentation/Extracted Leading Goals Files`. Source file names identify the business unit and goal year. `Product Development NZ + China` is mapped to `Product Development` by user decision. The filename year is used when a target header names an earlier year.

The seed contains supported numeric values. A blank target is not zero. The agreed `HC_F` and `HC_M` values are Target 0.40 and Leading Goal 0.50 for every extracted business-unit/year file, including files requiring manual inspection.

The Global decks supply Full Business targets and Leading Goals for 2023–2025; 2026 uses the 2025 figures by user decision. After those values were added, 380 existing rows with blank Targets received the corresponding Global Target for the same year and metric. Existing nonblank Targets and Leading Goals were preserved. Resolved missing-target flags were removed below; the seed does not identify fallback values at row level.

## File and measure exceptions

No file-level extraction flags remain. The user supplied screenshots for Production Engineering NZ 2024 and Information Technology 2025, confirmed the Production Engineering NZ assignment, and approved entering their values. The agreed headcount values were retained.

## Validation and limitations

The extracted CSV was checked for duplicate MetricKey, GoalYear, ScopeType and ScopeKey combinations and rates outside 0–1. The Customer Service AU 2023 rows were checked against the reviewed source table. Power BI Desktop, Parquet generation and published results have not been validated in this extraction.

