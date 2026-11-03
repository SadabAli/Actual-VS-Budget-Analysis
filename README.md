# Actual vs Budget Analysis Dashboard (Power BI)

## Project Overview

This project compares the income and expenses planned by a company with the amounts it actually earned and spent. The company manages projects for different clients. The dashboard helps users review results by year, month and client.

- **Income:** Money earned from each client.
- **Expense:** Money spent to complete or manage each client project.
- **Project_ID:** The client identifier used in the source data.
- **Currency:** US Dollars (USD, $).

## Tools Used

- Microsoft Power BI
- Power Query
- DAX (Data Analysis Expressions)
- Microsoft Excel
- SVG for custom progress visuals and the dashboard background

## Dataset

The main monthly performance table contains **360 records** from **January 2019 to December 2024**. It covers **5 client IDs** (1050–1054), with one record per client per month.

| Column | Description |
|---|---|
| `Month_End` | Month-end date for the record |
| `Project_ID` | Client identifier |
| `Income_Budget` | Planned income |
| `Income_Actual` | Actual income earned |
| `Expense_Budget` | Planned expense |
| `Expense_Actual` | Actual expense |

The main table has no blank values or exact duplicate rows in the supplied source file.

## Data Preparation

1. Imported the Excel workbook into Power BI and selected the monthly performance table.
2. Set the correct data types in Power Query.
3. Converted `Month_End` from text to Date using a locale that reads the source date order correctly (day/month/year).
4. Kept client ID as a whole number and money columns as decimal/fixed decimal values to preserve cents.
5. Loaded the prepared data using **Close & Apply**.

> The source data contains an expense value of $5,817.20. Currency columns should not be changed to Whole Number if cents need to be preserved.

## Calculated Date Columns

```dax
year = YEAR('dataset'[Month_End])

Month Name = FORMAT('dataset'[Month_End], "mmm")

Month Number = MONTH('dataset'[Month_End])
```

`Month Name` should be sorted by `Month Number` in Power BI so months appear in calendar order.

## Main DAX Measures

```dax
Income Actual = SUM('dataset'[Income_Actual])
Income Budget = SUM('dataset'[Income_Budget])

% of Income Budget =
DIVIDE([Income Actual], [Income Budget], 0)

Expense Actual = SUM('dataset'[Expense_Actual])
Expense Budget = SUM('dataset'[Expense_Budget])

% of Expense Budget =
DIVIDE([Expense Actual], [Expense Budget], 0)

Income Variance % =
DIVIDE([Income Actual] - [Income Budget], [Income Budget], 0)

Expense Variance % =
DIVIDE([Expense Actual] - [Expense Budget], [Expense Budget], 0)
```

**Measure meanings:**

- Income budget achievement = actual income / income budget.
- Expense budget usage = actual expense / expense budget.
- Variance = actual minus budget. A negative income variance means income was below plan. A negative expense variance means spending was below budget.

## Dashboard Features

- Year, Month Name and Project_ID slicers. All three filters are intended to affect every chart, KPI card and table.
- Income and Expense KPI cards showing actual amounts, budgets and variances.
- Donut-style SVG visuals showing budget percentages.
- SVG progress bars in the client-level tables.
- Yearly clustered column charts comparing actual and budget values.
- Client-level detail tables for income and expense.
- Information icons to support dashboard guidance.

## Colour Rules

The project uses a 95% threshold for both Income and Expense:

| Actual as % of Budget | Colour |
|---|---|
| Below 95% | Red |
| Exactly 95% | Amber |
| Above 95% | Green |

This is a project-specific rule selected for the dashboard, not a universal finance standard. **Important:** for expenses, a value above 100% means spending is above budget. Under the shared colour rule it would still be green, so this rule should be reviewed with the business before the dashboard is used for real decisions.

## Key Findings from the Full Dataset

| Metric | Budget | Actual | Actual / Budget |
|---|---:|---:|---:|
| Income | $7,102,599 | $6,906,642 | 97.24% |
| Expense | $2,440,798 | $2,253,609.20 | 92.33% |

- Total income was $195,957 below budget (2.76% below plan).
- Total expenses were $187,188.80 below budget (7.67% below plan).
- 2021 had the strongest income achievement at 102.68% of budget.
- 2024 had the weakest income achievement at 87.25%; actual income was $156,962 below the annual budget.
- Client 1052 had the strongest income achievement at 101.57%.
- Client 1054 had the weakest income achievement at 93.24%.
- Total expenses were below budget for all five clients and in all six years.

These findings describe what happened in the supplied data. The dataset does not explain why a variance occurred.


## Project Summary

An interactive Power BI dashboard that compares actual income and expenses with budgets for five clients across 2019–2024, using Power Query for data preparation and DAX for calculations and custom SVG progress visuals.
