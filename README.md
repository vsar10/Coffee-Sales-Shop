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

           Date Table = CALENDAR(MIN(Coffee_Sales[transaction_date]),Max(Coffee_Sales[transaction_date]))

- **Step 3:-** Add a "Month" column to display the month abbreviation.

           Month = FORMAT('Date Table'[Date], "mmm")

- **Step 4:-** Add a "Month Number" column to display the numerical value of the month.

           Month Number = MONTH('Date Table'[Date])

- **Step 5:-** Add a "Month Year" column to display the month and year together.

           Month Year = FORMAT('Date Table'[Date], "mmm yyyy")

- **Step 6:-** Add a "Day Name" column to display the day of the week abbreviation.

           Day Name = FORMAT('Date Table'[Date], "DDD")

- **Step 7:-** Add a "Week Number" column to display the week number (ISO format).

           Week Number = WEEKNUM('Date Table'[Date], 2)

- **Step 8:-** Add a "Day Number" column to display the day of the month.

           Day Number = FORMAT('Date Table'[Date], "D")

- **Step 9:-** Add a "Week Day Number" column to display the numerical value for the day of the week.

           Week Day Number = WEEKDAY('Date Table'[Date], 2)

- **Step 10:-** Add a "Weekday/Weekend" column to distinguish between weekdays and weekends.

           Weekday / Weekend = IF('Date Table'[Day Name] = "Sat" || 'Date Table'[Day Name] = "Sun", "Weekend", "Weekday")

- **Step 11:-** Calculated "Total Sales", "PM Sales", "CM Sales", "Total Orders", "PM Orders", "CM Orders", "Total Quantity", "PM Quantity Sold", "CM Quantity Sold".
<br> **Total Sales**

           Total Sales = SUM(Coffee_Sales[Sales])

<br> **PM Sales**

           PM Sales = CALCULATE([CM Sales], DATEADD('Date Table'[Date], -1, MONTH))

<br> **CM Sales**
          
           CM Sales = VAR selected_month = SELECTEDVALUE('Date Table'[Month])
                      RETURN
                      TOTALMTD(CALCULATE([Total Sales], 'Date Table'[Month] = selected_month),'Date Table'[Date])

<br> **Total Orders**
         
           Total Orders = DISTINCTCOUNT(Coffee_Sales[transaction_id])

<br> **PM Order**
         
           PM Order = CALCULATE([CM Orders], DATEADD('Date Table'[Date], -1, MONTH))

<br> **CM Sales**
         
           CM Sales = VAR selected_month = SELECTEDVALUE('Date Table'[Month])
                     RETURN
                     TOTALMTD(CALCULATE([Total Sales], 'Date Table'[Month] = selected_month),'Date Table'[Date])

<br> **Total Quantity**
         
           Total Quantity = SUM(Coffee_Sales[transaction_qty])

<br> **PM Quantity Sold**
         
           PM Quantity Sold = CALCULATE([CM Quantity Sold], DATEADD('Date Table'[Date], -1, MONTH))

<br> **CM Quantity Sold**
         
           CM Quantity Sold = VAR selected_month = SELECTEDVALUE('Date Table'[Month])
                              RETURN
                              TOTALMTD(CALCULATE([Total Quantity], 'Date Table'[Month] = selected_month),'Date Table'[Date])

- **Step 12**:- Create a condition for "Color of Bars" such as "Above Average" or "Below Average" with respect to "Total Sales"

           Color for Bars = IF([Total Sales]> [Daily Avg Sales], "Above Average", "Below Average")

- **Step 13**:- Calculated "Daily Avg Sales" for Coffee with respect to "transaction date"

           Daily Avg Sales = AVERAGEX(ALLSELECTED(Coffee_Sales[transaction_date]),[Total Sales])

- **Step 14**:- Calculated "Mom Growth & Diff Order" in terms of Orders 

             MOM Growth & Diff Order = 
                   VAR month_diff = [CM Orders]-[PM Order]
                   VAR Mom = ([CM Orders]-[PM Order])/[PM Order]
                   VAR _sign = IF(month_diff > 0, "+","")
                   VAR _sign_trend = IF (month_diff > 0, "▲", "▼")
                   RETURN
                   _sign_trend & " " & _sign & FORMAT(Mom, "#0.0%" & " | " & _sign & FORMAT(month_diff/1000, "0.0K")) & " " & "vs LM"

- **Step 15**:- Calculated "MOM Growth & Diff Quantity Sold" in terms of Quantity Sold

             MOM Growth & Diff Quantity Sold = 
                   VAR month_diff = [CM Quantity Sold]-[PM Quantity Sold]
                   VAR Mom = ([CM Quantity Sold]-[PM Quantity Sold])/[PM Quantity Sold]
                   VAR _sign = IF(month_diff > 0, "+","")
                   VAR _sign_trend = IF (month_diff > 0, "▲", "▼")
                   RETURN
                   _sign_trend & " " & _sign & FORMAT(Mom, "#0.0%" & " | " & _sign & FORMAT(month_diff/1000, "0.0K")) & " " & "vs LM"

- **Step 16**:- Calculated "MOM Growth & Diff Sales" in terms of Sales

             MOM Growth & Diff Sales = 
                   VAR month_diff = [CM Sales]-[PM Sales]
                   VAR Mom = ([CM Sales]-[PM Sales])/[PM Sales]
                   VAR _sign = IF(month_diff > 0, "+","")
                   VAR _sign_trend = IF (month_diff > 0, "▲", "▼")
                   RETURN
                   _sign_trend & " " & _sign & FORMAT(Mom, "#0.0%" & " | " & _sign & FORMAT(month_diff/1000, "0.0K")) & " " & "vs LM"

- **Step 17**:- Calculated " New MoM Label" in terms of Sales

             New MoM Label = 
                   VAR month_diff = [CM Sales]-[PM Sales]
                   VAR Mom = ([CM Sales]-[PM Sales])/[PM Sales]
                   VAR _sign = IF(month_diff > 0, "+","")
                   VAR _sign_trend = IF (month_diff > 0, "▲", "▼")
                   RETURN
                   _sign_trend & " " & _sign & FORMAT(Mom, "#0.0%")
