# 📊 Project Title: LL Mountain Seat Assembly - Production Dashboard

Author: DUY TRAN

Date: 2025-03-27

Tools Used: Power BI

# 📑 Table of Contents

📌 Background & Overview

📂 Dataset Description & Data Structure

🧠 Design Thinking Process

📊 Key Insights & Visualizations

🔎 Final Conclusion & Recommendations

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


🧠 Design Thinking Process

![image](https://github.com/user-attachments/assets/4ad7ec13-f7a5-4279-8273-eb25cad91d5d)

![image](https://github.com/user-attachments/assets/31a4ea06-e0b1-402f-a75a-797886abb7b0)

![image](https://github.com/user-attachments/assets/e6e1bda3-762f-4b82-89e8-e55270fe9665)

⚒️ Main Process

**Dashboard Features**

* Interactive visualizations for easy data exploration
* Key performance indicators (KPIs) for quick insights
* Filters and slicers for customized data views (e.g., by date, scrap reason).
* Trend analysis to identify patterns and anomalies.

**Choosing the Right Charts**

* **Overall Performance:**
    * Cards: Displaying Total Work Orders, Average Production Time, Delay Percentage, Scrapped Quantity.
    * A new column  of Production Time was created by applying DAX and the given StarDate/EndDate columns

      ```DAX
      Production Time = Production_WorkOrder[EndDate].[Date] - Production_WorkOrder[StartDate].[Date]
      ```
    * the column of Delay was also deployed to see if each Work Order was finished after or before Due Date and a column calculted the number days of delay
 
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

![image](https://github.com/user-attachments/assets/11f5dfe2-ef23-4507-ad84-0b24654ed5ee)

![image](https://github.com/user-attachments/assets/d137e9a2-b2a9-4a6e-a774-b03ac21abf50)


**Insights and Recommendations**

* **High Delay Percentage (51.5%):** Investigate the root causes of delays and implement measures to reduce them.
* **Significant Scrap Quantity (17 units):** Focus on quality control, particularly addressing the Thermoform temperature issue. Further investigate the large number of unknown scrapped reasons.
* **Production Time Trends:** Analyze the fluctuations in production time to identify potential bottlenecks or seasonal variations.
* **Scheduled vs. Actual Times:** Address the differences between scheduled and actual times to improve planning and execution accuracy.
* **Cost Matrix:** Review the cost matrix for possible cost reductions.
* **Sub assembly vs final assembly:** Research why there is such a difference in the time spent between sub assembly and final assembly.

**Future Enhancements**

* Incorporate more detailed data on work orders and production processes.
* Develop predictive models to forecast production times and potential delays.
* Integrate with other systems for a holistic view of the manufacturing operations.
