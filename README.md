# Power BI Supply Chain Dashbboard
Excel | Power BI 

Analyzing sales and shipping data across 5 regions (countries) within a 4 month time frame (quarter). The goal is to uncover revenue trends, lead times & shipping costs, and defect rates by supplier.

A Power BI project using data from a supply chain company (kaggle)

Data:
- I got the data from Kaggle. Since it was already cleaned, I used Gemini (AI) to make it alittle messy. I made some transformations using power query to clean it up in Power BI. 

------------------------------------------

Screenshots

Overview Page:
<img width="1146" height="641" alt="image" src="https://github.com/user-attachments/assets/0e3f9a5e-3337-479e-972f-ebeaa0033dcc" />

Efficience Page:
<img width="1118" height="622" alt="image" src="https://github.com/user-attachments/assets/23afa30e-4f76-4bd7-a995-e536925577da" />

Risk Page:
<img width="1139" height="633" alt="image" src="https://github.com/user-attachments/assets/729ed0e6-5da1-4ada-8fba-c6c619e55712" />

Relationship Model:
<img width="883" height="685" alt="image" src="https://github.com/user-attachments/assets/94175652-e1cb-4d53-8f93-8a40ccfb5032" />


-----------------------
Business Problem:
???



What I built:

Data Model
- Built a star schema: A fact table (fact_SupplyChain) and 6 dimention tables (Date, Supplier, Inspection, Location, Demographics, Products, Shipping)
- * Created a custome Data table using DAX for time intelligence
 
Key DAX Measures:
-- dim_Date
dim_Date = 
ADDCOLUMNS (
    CALENDAR(MIN('Fact_SupplyChain'[Transaction Date]), MAX('Fact_SupplyChain'[Transaction Date])),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Month Number", MONTH([Date]),
    "Quarter", "Q" & FORMAT([Date], "Q")
)

-- YoY Growth
YoY Growth = 
VAR CurrentRevenue = SUM(fact_SupplyChain[Revenue generated])
VAR PastRevenue = CALCULATE(SUM(fact_SupplyChain[Revenue generated]), SAMEPERIODLASTYEAR(dim_Date[Date]))
RETURN
DIVIDE(CurrentRevenue - PastRevenue, PastRevenue)

-- Profit
Profit = 
SUM(fact_SupplyChain[Revenue generated]) - SUM(fact_SupplyChain[Costs]) + SUM(fact_SupplyChain[Shipping costs])

-- Total Cost per unit
Total Cost per unit = 
DIVIDE(
    SUM(fact_SupplyChain[Shipping costs]) + SUM(fact_SupplyChain[Manufacturing costs]), 
    [Total Units Sold]
)

Report Features:
- Dynamic slicers
