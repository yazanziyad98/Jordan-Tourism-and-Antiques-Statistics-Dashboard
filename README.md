# Jordan-Tourism-and-Antiques-Statistics-Dashboard

A Power BI dashboard that visualizes Jordan's tourism sector, covering international visitor arrivals, expenditure, accommodation, employment, tourist sites, and domestic tourism. Data comes from the Jordanian **Ministry of Tourism & Antiquities**.

🔗 **Live Dashboard:** [View on Power BI](https://app.powerbi.com/view?r=eyJrIjoiYmUxMDQwZjUtNmYwNC00MDZkLWE1YjUtMjYxMjQwMTdhNjUzIiwidCI6IjI0Nzc2OGNkLWQyY2EtNDQyMy1iYWEwLTI2ZjgwODQ2OWRlNCIsImMiOjl9)

📄 **Full Preview (PDF):** [dashboard-preview.pdf](./dashboard-preview.pdf)

## Overview

The dashboard gives a full view of Jordan's tourism industry across several dimensions, useful for policymakers, researchers, and anyone interested in tourism trends.

### Key Metrics

| Indicator | Value |
|---|---|
| International Visitor Arrivals (2026 YTD) | 1,094,670 |
| Total Visitor Expenditure | 880.7M JD (~$1.24B USD) |
| Average Expenditure per Visitor | 805 JD |
| Average Length of Stay | 4.8 nights |
| Total Accommodation Establishments | 908 |
| Total Tourism Employees | 60,068 |
| Domestic Visitors (2025) | 161,214 |

## Dashboard Sections

1. **Overview**: KPIs for visitor arrivals, tourism receipts, and employment from 2020 to 2026.
2. **International Visitor Arrivals**: Breakdown by nationality, border entry type (land, air, sea), entry point, and country group.
3. **International Visitor Expenditure**: Spending by Arabs, foreigners, and Jordanians residing abroad, plus trends and average length of stay.
4. **Accommodation Establishments**: Hotels, rooms, beds, and employees by classification (1 to 5 stars, suites, camps, etc.) and by governorate.
5. **Tourist Sites**: Visits to archaeological and cultural sites such as Petra, Wadi Rum, and Jerash, broken down by nationality.
6. **Tourism Employment**: Workforce data by segment (restaurants, agencies, guides) and by governorate.
7. **Domestic Tourism / Urdunna Jannah**: Local tourism activity across destinations in Jordan.

## Highlights

- International visitor arrivals peaked at 7.0M in 2025, then dropped to 1.1M in 2026 YTD.
- International tourism receipts peaked at 5.5 billion JD before declining to 0.9 billion JD.
- Employment in the tourism sector grew from 53K in 2020 to 60K in 2025.

## Tech Stack

- **Microsoft Power BI** for data modeling, DAX measures, and interactive visuals.
- **Data Source:** Jordan Ministry of Tourism & Antiquities.

## Sample DAX Measures

A few measures used in the dashboard:

**Year-over-Year change in international arrivals**
```dax
Int. tourism arrivals = 
VAR PYTouristsNum = CALCULATE([Total Tourists - Arrivals], DATEADD(Dim_Date[FullDate], -1, YEAR))
RETURN 
    IF(
        ISBLANK([Total Tourists - Arrivals]), BLANK(),
        DIVIDE([Total Tourists - Arrivals] - PYTouristsNum, PYTouristsNum) + 0
    )
```

**Income per Tourist (aggregated by year)**
```dax
Income per Tourist = 
SUMMARIZECOLUMNS(
    vwSurveyConstantsTransactionsDashboard[Year],
    "Total Income", [Total Tourism Income],
    "Total Tourists", SUM('vwSurveyConstantsTransactionsDashboard'[Number_Of_Tourists])
)
```

**Summarized trip bookings (Urdunna Jannah program)**
```dax
Summarized Utilization = 
SUMMARIZE(
    'Urdunna Jannah 2021',
    'Urdunna Jannah 2021'[OrderDate],
    'Urdunna Jannah 2021'[StartingPoint1_Ar],
    'Urdunna Jannah 2021'[TripName],
    'Urdunna Jannah 2021'[OfficeName_Ar],
    'Urdunna Jannah 2021'[CityName_En],
    "PeopleBooking", COUNT('Urdunna Jannah 2021'[PeopleId])
)
```
