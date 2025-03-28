# 📊 Project Title: LL Mountain Seat Assembly - Production Dashboard

Author: DUY TRAN

Date: 2025-03-27

Tools Used: Power BI

# 📑 Table of Contents

📌 [Background & Overview](#-background--overview)

📂 [Dataset Description & Data Structure](#-dataset-description--data-structure)

🧠 [Design Thinking Process](#-design-thinking-process)

⚒️ [Main Process](#-Main-Process)

📊 [Key Insights & Visualizations](#-key-insights--visualizations)

🔎 [Final Conclusion and Future Enhancements](#-final-conclusion-and-future-enhancements)

# 📌 Background & Overview

 ## 📖 What is this project about?

 ✔️ This project focuses on developing an interactive and insightful Power BI dashboard for the Production Director of LL Mountain Seat Assembly. 
 
 ✔️ The primary goal is to provide a clear, visual overview of the production operations, enabling data-driven decision-making and process improvement.

 ## 👤 Who is this project for?

 ✔️ Production and planning managers:  use the dashboard to optimize production schedules, manage inventory effectively, allocate resources efficiently, and improve overall profitability
 
 ✔️ Business/Data Analyst: Understanding Business Performance, Developing Reporting and Dashboards and Improving Data-Driven Decision Making
 
 ## ❓ Key Objectives/Business Questions:

* **Operational Visibility:** Create a comprehensive dashboard that offers a snapshot of the current production status.
* **Performance Analysis:** Analyze key metrics such as production time, delays, and scrap rates to identify bottlenecks and areas for improvement.
* **Actionable Insights:** Provide data-backed recommendations to optimize the production process and enhance efficiency.

 ## 🗝️ Methodology

* **Data Collection:** Gather relevant data on work orders, production times, delays, and scrap reasons.
* **Power BI Development:** Design and develop an intuitive dashboard with interactive charts, graphs, and tables to visualize the data.
* **Analysis and Insights:** Analyze the data to derive meaningful insights and identify trends, patterns, and areas for improvement.
* **Recommendations:** Based on the analysis, provide actionable recommendations to the Production Director.

# 📂 Dataset Description & Data Structure

 ## 📌 Data Source
 
**Source:** **AdventureWorks** - Microsoft SQL Server sample database, a fictitious bicycle manufacturer (Adventure Works Cycles).

**Size:** This project uses 5 tables recorded the production information of product 'LL Mountain Seat Assembly' 

**Format:** .csv

## 📊 Data Structure & Relationships

1️⃣ Table Schema

<details>
<summary>👉🏻 Fact Table 1: Production_WorkOrder</summary>
<br>

| Name           | Data Type |
|----------------|-----------|
| WorkOrderID    | int       |
| ProductID      | int       |
| OrderQty       | int       |
| StockedQty     | int       |
| ScrappedQty    | smallint  |
| StartDate      | datetime  |
| EndDate        | datetime  |
| DueDate        | datetime  |
| ScrapReasonID  | smallint  |
| ModifiedDate   | datetime  |
</details>

<details>
<summary>👉🏻 Fact Table 2: Production_WorkOrderRouting</summary>
<br>
 
| Name              | Data Type   |
|-------------------|-------------|
| WorkOrderID       | int         |
| ProductID         | int         |
| OperationSequence | smallint    |
| LocationID        | smallint    |
| ScheduledStartDate| datetime    |
| ScheduledEndDate  | datetime    |
| ActualStartDate   | datetime    |
| ActualEndDate     | datetime    |
| ActualResourceHrs | decimal(9,4)|
| PlannedCost       | money       |
| ActualCost        | money       |
| ModifiedDate      | datetime    |
</details>

<details>
<summary>👉🏻 Dim Table 1: Production_ScrapReason</summary>
<br>
 
| Name           | Data Type  |
|----------------|------------|
| ScrapReasonID  | smallint   |
| Name           | nvarchar(50)|
| ModifiedDate   | datetime   |

</details>

<details>
<summary>👉🏻 Dim Table 2: Production_Location</summary>
<br>
 
| Name          | Data Type   |
|---------------|-------------|
| LocationID    | smallint    |
| Name          | nvarchar(50)|
| CostRate      | smallmoney  |
| Availability  | decimal(8,2)|
| ModifiedDate  | datetime    |

</details>

<details>
<summary>👉🏻 Dim Table 3: Production_Inventory</summary>
<br>
 
 | Name                                                                 | Data Type      |
|----------------------------------------------------------------------|----------------|
| **ProductID**<br>Product identification number.<br>Foreign key to Product.ProductID | int            |
| **LocationID**<br>Inventory location identification number.<br>Foreign key to Location.LocationID | smallint       |
| **Shelf**<br>Storage compartment within an inventory location        | nvarchar(10)   |
| **Bin**<br>Storage container on a shelf in an inventory location    | tinyint        |
| **Quantity**<br>Quantity of products in the inventory location.<br>Default: 0 | smallint       |
| **rowguid**<br>ROWGUIDCOL number uniquely identifying the record.<br>Used to support a merge replication sample.<br>Default: newid() | uniqueidentifier |
| **ModifiedDate**<br>Date and time the record was last updated.<br>Default: getdate() | datetime       |

</details>

2️⃣ Data Relationships:

![image](https://github.com/user-attachments/assets/5bce1abf-ad4d-4051-9abf-dffbb9fc52c1)


# 🧠 Design Thinking Process

![image](https://github.com/user-attachments/assets/4ad7ec13-f7a5-4279-8273-eb25cad91d5d)

![image](https://github.com/user-attachments/assets/31a4ea06-e0b1-402f-a75a-797886abb7b0)

![image](https://github.com/user-attachments/assets/e6e1bda3-762f-4b82-89e8-e55270fe9665)

# ⚒️ Main Process

**Dashboard Features**

* Interactive visualizations for easy data exploration
* Key performance indicators (KPIs) for quick insights
* Filters and slicers for customized data views (e.g., by date, scrap reason).
* Trend analysis to identify patterns and anomalies.

**Choosing the Right Charts**

* **Overall Performance:**
    * Cards: Displaying Total Work Orders, Average Production Time, Delay Percentage, Scrapped Quantity.
    * A new column  of `Production Time` was created by applying DAX and the given StarDate/EndDate columns

      ```DAX
      Production Time = Production_WorkOrder[EndDate].[Date] - Production_WorkOrder[StartDate].[Date]
      ```
    * the column of `Delay` was also deployed to see if each Work Order was finished after or before Due Date and a column of `delay date num` was calculted the number days of delay
 
      ```DAX
      Delay = IF(Production_WorkOrder[EndDate].[Date] > Production_WorkOrder[DueDate].[Date], 1,
        if (Production_WorkOrder[EndDate].[Date] > Production_WorkOrder[DueDate].[Date], 2,0))
      ```

      ```DAX
      delay date num = Production_WorkOrder[EndDate].[Date] - Production_WorkOrder[DueDate].[Date]
      ```
      
* **Trends and Patterns:**
    * Line Charts: Visualizing Production Time Trends over time (monthly).
    * Column Charts: Comparing scheduled vs actual times for sub and final assembly.
    * Column chart: Average of start and end date diff by year and quarter.
* **Categorical Data:**
    * Pie Chart: Showing the distribution of scrap reasons.
    * Table: Displaying the cost matrix.
* **Time Based data**
    * Line chart: Displaying the Average number of delay dates.

# 📊 Key Insights & Visualizations

## 📌 Page 1 - Manufacturing Overview

![image](https://github.com/user-attachments/assets/1bddd3c7-1bd2-4f68-bd8a-6aa7a92f5bbd)

**Observation:**

* **Overall Production Time and Work Orders:** The average production time is 15.6 days, with a total of 326 work orders.
* **Production Time Trend:** Production time fluctuates over the months shown, with some variability. It's important to note the specific values for each month to understand the extent of these fluctuations.
* **Order Quantity by Month:** Order quantity varies by month, with peaks and troughs throughout the year. Notably, some months show higher order quantities than others. 
* **Average Delay:** The average number of delay days is 2.6.
* **Delay Percentage:** The delay percentage is 51.5%. 
* **Scrapped Quantity:** The scrapped quantity is 11. 
* **Scrapped Quantity Reasons:** "Wheel misaligned" is one reason for scrapping, with a quantity of 6. 
* **Production Time Trend Over Quarters:** Production time varies across quarters, with specific values for each quarter in 2013 and 2014. 

**Recommendation:**

* **Investigate Delay Causes:** A high delay percentage (51.5%) warrants further investigation into the root causes of these delays. Implement process improvements or resource adjustments to reduce delays and improve on-time delivery.
* **Analyze Production Time Variability:** Analyze the factors contributing to the fluctuations in production time. Implement strategies to stabilize production times, potentially through better resource allocation or process optimization.
* **Address Scrapped Quantity:** Focus on quality control measures to reduce the scrapped quantity, particularly addressing the "Wheel misaligned" issue. Determine the root cause of this misalignment and implement corrective actions.
* **Optimize Order Quantity Management:** Analyze the monthly order quantity data to identify patterns or seasonality. Improve forecasting and capacity planning to efficiently handle fluctuations in demand.

## 📌 Page 2 - Manufacturing Analysis

![image](https://github.com/user-attachments/assets/659a7e1c-edf7-4fdf-9a11-8547199e5160)

**Observation:**

* **Planned vs. Actual Costs:** The dashboard compares planned and actual costs, showing variations across quarters. For example, in one quarter, the sum of planned cost is $7.3K, while the sum of actual cost is $7.4K. 
* **Number of Work Orders:** The number of work orders varies by quarter. 
* **Delay Cases:** The number of delay cases is shown. 
* **Planned and Actual Production Time:** The dashboard compares average actual and scheduled production times, showing differences across quarters. 
* **Difference of Start/End Date:** The average difference between start and end dates, as well as scheduled dates, is displayed, showing variations over time. 
* **Location Stocked:** The dashboard provides a breakdown of where items are stocked, including Subassembly (39.9%), Miscellaneous Storage (33.5%), and Final Assembly (26.6%).

**Recommendation:**

* **Cost Variance Analysis:** Conduct a detailed cost variance analysis to understand the discrepancies between planned and actual costs. Identify areas where costs are exceeding estimates and implement cost control measures.
* **Production Scheduling Optimization:** Analyze the differences between actual and scheduled production times to improve production scheduling accuracy. Identify bottlenecks or inefficiencies that contribute to these discrepancies.
* **Start/End Date Management:** Investigate the differences in start and end dates to identify potential delays or scheduling issues. Implement better project management practices to improve adherence to schedules.
* **Inventory Optimization:** Analyze the location stocked data to optimize inventory management. Ensure efficient flow of materials between Subassembly, Miscellaneous Storage, and Final Assembly to minimize delays and reduce storage costs.

# 🔎 Final Conclusion and Future Enhancements

👉🏻 Based on the insights and findings above, The analysis of the manufacturing overview and analysis dashboards of product 'LL Mountain Seat Assembly'  reveals several critical areas that require attention to improve overall production efficiency and effectiveness. High delay percentages (51.5%), variability in production time, discrepancies between planned and actual costs, and issues with scrapped quantities  indicate significant challenges within the manufacturing process. To address these issues, it is crucial to implement process improvements 

### 📌 Future Enhancements
* Incorporate more detailed data on work orders and production processes.
* Develop predictive models to forecast production times and potential delays.
* Integrate with other systems for a holistic view of the manufacturing operations.
