Gather the requirements to do analysis for the Coffee Sales Shop where have to create an interactive dashboard based on client desire requirement
Client Requirement:- 

**(a) KPI Requirement:**
**1. Total Sales Analysis:**
- Calculate the total sales for each respective month.
- Determine the month-on-month increase or decrease in sales.
- Calculate the difference in sales between the selected month and the previous month.

**2. Total Orders Analysis:**
- Calculate the total number of orders for each respective month.
- Determine the month-on-month increase or decrease in the number of orders.
- Calculate the difference in the number of orders between the selected month and the previous month.

**3. Total Quantity Sold Analysis:**
- Calculate the total quantity sold for each respective month.
- Determine the month-on-month increase or decrease in the total quantity sold.
- Calculate the difference in the total quantity sold between the selected month and the previous month.

**(b) Charts Requirement:**
**1. Calendar Heat Map:**
- Implement a calendar heat map that dynamically adjusts based on the selected month from a slicer.
- Each day on the calendar will be color-coded to represent sales volume, with darker shades indicating higher sales.
- Implement tooltips to display detailed metrics (Sales, Orders, Quantity) when hovering over a specific day.

**2. Sales Analysis by Weekdays and Weekends:**
- Segment sales data into weekdays and weekends to analyze performance variations.
- Provide insights into whether sales patterns differ significantly between weekdays and weekends.

**3. Sales Analysis by Store Location:**
- Visualize sales data by different store locations.
- Include month-over-month (MoM) difference metrics based on the selected month in the slicer.
- Highlight MoM sales increase or decrease for each store location to identify trends.

**4. Daily Sales Analysis with Average Line:**
- Display daily sales for the selected month with a line chart.
- Incorporate an average line on the chart to represent the average daily sales.
- Highlight bars exceeding or falling below the average sales to identify exceptional sales days.

**5. Sales Analysis by Product Category:**
- Analyze sales performance across different product categories.
- Provide insights into which product categories contribute the most to overall sales.

**6. Top 10 Products by Sales:**
- Identify and display the top 10 products based on sales volume.
- Allow users to quickly visualize the best-performing products in terms of sales.

**7. Sales Analysis by Days and Hours:**
- Utilize a heat map to visualize sales patterns by days and hours.
- Implement tooltips to display detailed metrics (Sales, Orders, Quantity) when hovering over a specific day-hour.

**Created a date table Using DAX Expressions**
- **Step 1:-** Create a Date Table in Power BI.
- **Step 2:-** Generate the Date column based on the transaction date from the Coffee Sales table.
         <br>**Date Table = CALENDAR(MIN(Coffee_Sales[transaction_date]),Max(Coffee_Sales[transaction_date]))**
- **Step 3:-** Add a "Month" column to display the month abbreviation.
         <br>**Month = FORMAT('Date Table'[Date], "mmm")**
- **Step 4:-** Add a "Month Number" column to display the numerical value of the month.
         <br>**Month Number = MONTH('Date Table'[Date])**
- **Step 5:-** Add a "Month Year" column to display the month and year together.
         <br>**Month Year = FORMAT('Date Table'[Date], "mmm yyyy")**
- **Step 6:-** Add a "Day Name" column to display the day of the week abbreviation.
         <br>**Day Name = FORMAT('Date Table'[Date], "DDD")**
- **Step 7:-** AAdd a "Week Number" column to display the week number (ISO format).
         <br>**Week Number = WEEKNUM('Date Table'[Date], 2)**
- **Step 8:-** Add a "Day Number" column to display the day of the month.
         <br>**Day Number = FORMAT('Date Table'[Date], "D")**
- **Step 9:-** Add a "Week Day Number" column to display the numerical value for the day of the week.
         <br>**Week Day Number = WEEKDAY('Date Table'[Date], 2)**
- **Step 10:-** Add a "Weekday/Weekend" column to distinguish between weekdays and weekends.
         <br>**Weekday / Weekend = IF('Date Table'[Day Name] = "Sat" || 'Date Table'[Day Name] = "Sun", "Weekend", "Weekday")**

  
