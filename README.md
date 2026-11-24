# HR Attrition Analytics Dashboard (Power BI)

An interactive 5-page Power BI dashboard that analyzes employee attrition across **1,470 employees** and shows who leaves, where, and why. It ends with a short set of recommendations for HR.

![Overview](screenshots/1_overview.png)

## Key Findings

| Finding | Value |
|---|---|
| Overall attrition | **16.1%** (237 of 1,470 employees) |
| Overtime vs no overtime | **30.5%** vs **10.4%** (about 3x higher) |
| Highest-risk role | Sales Representative, **39.8%** |
| Employees with 0-1 years of tenure | **34.9%** left (vs 8.1% for 10+ years) |
| No stock options vs with options | **24.4%** vs 8-9% |
| Single vs married employees | **25.5%** vs 12.5% |
| Average monthly income | Leavers **$4.8K** vs stayers **$6.8K** |

**By department:** Sales 20.6% · Human Resources 19.0% · R&D 13.8%

### Recommendations
1. **Cap overtime.** It is the strongest single driver of attrition in this data.
2. **Strengthen first-year onboarding and check-ins.** Early tenure is when people leave most.
3. **Review compensation and stock options for high-risk roles**, especially Sales Representatives and Laboratory Technicians.

## Dashboard Pages

| Page | What it shows |
|---|---|
| **Overview** | Company KPIs, employees by department, overtime split, attrition vs 10% target, attrition by age group, income by job level and by business travel |
| **Sales** | Department KPIs, attrition by job role, tenure, education field, income vs tenure scatter |
| **R&D** | Same layout for the largest department (961 employees) |
| **Human Resources** | Same layout for the smallest department (63 employees) |
| **Insights** | Key Influencers and Decomposition Tree for root-cause analysis, with headline insight cards |

**Interactivity:** sidebar page navigation, cross-filtering between visuals, synced Gender and OverTime slicers across all pages, and a Reset button.

## Screenshots

| Overview | Sales |
|---|---|
| ![Overview](screenshots/1_overview.png) | ![Sales](screenshots/2_sales.png) |

| R&D | Human Resources |
|---|---|
| ![R&D](screenshots/3_rnd.png) | ![HR](screenshots/4_hr.png) |

| Insights |
|---|
| ![Insights](screenshots/5_insights.png) |

## Data

- **Source:** IBM HR Analytics Employee Attrition & Performance dataset (public, available on Kaggle). It is a fictional dataset created by IBM data scientists.
- **Size:** 1,470 rows, 35 original columns.
- **Preparation (Python / pandas):**
  - Removed constant columns (`EmployeeCount`, `Over18`, `StandardHours`).
  - Added `AttritionFlag` (0/1) and banded fields: `AgeGroup`, `TenureBand`, `IncomeBand`.
  - Added readable labels for `JobSatisfaction`, `WorkLifeBalance` and `Education`.
  - Added sort-order columns (`AgeSort`, `TenureSort`, `IncomeSort`) so bands display in logical order.

## DAX Measures

```dax
Total Employees = COUNTROWS(HR)

Attrition Count = CALCULATE(COUNTROWS(HR), HR[Attrition] = "Yes")

Attrition Rate = DIVIDE([Attrition Count], [Total Employees], 0)

Company Attrition Rate = CALCULATE([Attrition Rate], ALL(HR[Department]))

Attrition vs Company = [Attrition Rate] - [Company Attrition Rate]

Overtime Rate =
DIVIDE(CALCULATE(COUNTROWS(HR), HR[OverTime] = "Yes"), [Total Employees], 0)

Page Title =
IF(ISFILTERED(HR[Department]),
   SELECTEDVALUE(HR[Department], "Multiple Departments") & " · Workforce Analytics",
   "Company Overview · Workforce Analytics")
```

The full list is in [`DAX_Measures.dax`](DAX_Measures.dax).

## Design

- Dark glassmorphism UI: custom page backgrounds with frosted-glass cards, with transparent visuals placed on top.
- Custom theme file ([`HR_Glass_Theme.json`](HR_Glass_Theme.json)) for colors and fonts.
- Each department page has its own accent color.
- Canvas size: 1280 x 720 (16:9).

## Repository Structure

```
├── HR_Attrition_Dashboard.pbix
├── HR_Attrition_Prepared.csv
├── DAX_Measures.dax
├── HR_Glass_Theme.json
├── backgrounds/        # page background images
└── screenshots/        # dashboard page previews
```

## How to Open

1. Download `HR_Attrition_Dashboard.pbix`.
2. Open it in **Power BI Desktop**.
3. If prompted for the data source, point it to `HR_Attrition_Prepared.csv`.

## Limitations

- The dataset is **fictional and a snapshot**, so findings show patterns in this data and not causal proof.
- The **Human Resources** department has only 63 employees (about 12 leavers). Its percentages are directional, not conclusive.
- Attrition drivers overlap (for example, overtime and job level), so Key Influencers results should be read as associations.

## Tools

Power BI · DAX · Power Query · Python (pandas)

## Author

**[Your Name]**
[LinkedIn](https://www.linkedin.com/in/your-profile) · [GitHub](https://github.com/your-username)
