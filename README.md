# Hotel-Revenue-Data-Analysis-with-SQL
End-to-end SQL project analyzing hotel revenue data, including database design, data cleaning, and advanced querying. Applied normalization and schema optimization to improve data accuracy and efficiency, then extracted insights on revenue trends, seasonal patterns, and performance drivers to support data-driven decision-making.

Project Methodology
1. Database Creation & Data Import (SSMS)

A new relational database, named Project: Hotel Revenue, was created using SQL Server Management Studio (SSMS). The hotel_revenue.xlsx Excel file was then imported into this database using the SSMS Data Import Wizard. The imported file contained the following five tables:

    dbo.'2018$'

    dbo.'2019$'

    dbo.'2020$'

    dbo.market_segment$

    dbo.meal_cost$

2. SQL Data Preparation & Querying

SQL queries were run directly in SSMS to prepare the data for analysis. The first step was to combine the yearly booking data into a single temporary table for analysis.

SQL Code to Combine Data:

with hotels as (
    select * from dbo.['2018$']
    union
    select * from dbo.['2019$']
    union
    select * from dbo.['2020$']
)

The next step was to perform LEFT JOIN operations to enrich the combined booking data with information on market segments and meal costs.

SQL Code to Join Tables:

with hotels as (
    select * from dbo.['2018$']
    union
    select * from dbo.['2019$']
    union
    select * from dbo.['2020$']
)
select * from hotels 
left join dbo.market_segment$
on hotels.market_segment = market_segment$.market_segment
left join dbo.meal_cost$
on meal_cost$.meal = hotels.meal

This prepared dataset was then used to calculate key metrics, such as revenue, which was derived from the average daily rate (ADR) multiplied by the total nights stayed.

SQL Code to Calculate Revenue:

with hotels as (
    select * from dbo.['2018$']
    union
    select * from dbo.['2019$']
    union
    select * from dbo.['2020$']
)
select 
    arrival_date_year,
    hotel,
    round(sum((stays_in_week_nights+stays_in_weekend_nights)*adr),) as revenue 
from hotels
group by arrival_date_year, hotel

3. Power BI Visualisation & Dashboard

The database was then connected to Power BI to create a comprehensive and interactive dashboard. The dashboard includes a range of visualisations and metrics, such as:

    Total Revenue: Calculated using the formula ([stays_in_week_nights]+[stays_in_weekend_nights])*([adr]*(1-[Discount])).

    Key Performance Indicators: Displays average ADR, total nights spent, and average discount.

    Revenue Visualisations: A donut chart showing the sum of revenue by hotel, and a line graph demonstrating revenue over time. This graph can be filtered by country, time period, and hotel type using a slicer and dropdown menu.

    Parking Analysis: A custom column named Parking Percentage was created with the formula "Parking Percentage = sum(Query1[required_car_parking_spaces])/[Total Nights]". This table was used to analyse whether an increase in car park spaces was a worthwhile business decision.

Project Deliverables

    An interactive Power BI dashboard providing a 360-degree view of hotel revenue and guest behaviour.

    Data-driven insights and a recommendation on car park expansion.

    A clear demonstration of a complete data analysis workflow from database design to final reporting.
