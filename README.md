# 🚀 Automated E-Commerce Financial Intelligence Pipeline

![Power BI](https://img.shields.io/badge/PowerBI-F2C811?style=for-the-badge&logo=Power%20BI&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)

## 📌 Project Overview
In the fast-paced E-commerce sector, companies often confuse high gross revenue with profitability. This project is an **End-to-End Automated Data Pipeline** designed to simulate live order ingestion, clean anomalies, compute strict financial metrics (Net Sales, COGS, Profit Margin), and visually uncover the hidden impact of aggressive discounting strategies on the bottom line.

**Role:** Data Analyst / Analytics Engineer  
**Domain:** E-Commerce & Financial Operations  

---

## 🏗️ Technical Architecture (The ETL Workflow)

This project moves away from static CSV analysis by implementing a live relational database architecture using Python and MySQL.

### 1. Extract (Data Generation & API Simulation)
* Engineered a Python script that simulates a live E-commerce environment, generating daily batches of 500+ orders.
* **Data Engineering Fix:** Solved the cross-batch duplication (Upsert) problem by implementing `uuid4()` algorithms to generate globally unique `order_id` primary keys.

### 2. Transform (Data Cleaning & Financial Logic via Pandas)
* **Data Cleaning:** Handled missing geographic data (Null regions) and filtered out data-entry errors (e.g., negative sales figures).
* **Feature Engineering:** Translated raw data into business reality. Created dynamic columns for:
  * `Discount_Amount` = Gross Sales * Discount %
  * `Net_Sales` = Gross Sales - Discount Amount
  * `COGS` (Cost of Goods Sold) = Simulated base cost of manufacturing.
  * `Profit_Amount` = Net Sales - COGS

### 3. Load (MySQL Data Warehouse)
* Utilized `SQLAlchemy` and `mysql-connector-python` to automate the data push to a local MySQL Database.
* Configured the pipeline with `if_exists='append'`, creating a continuously updating, time-series Data Warehouse ready for live querying.

### 4. Visualize (Power BI & DAX)
* Connected Power BI directly to the live MySQL database for real-time tracking.
* Developed an Executive Dashboard using a Z-pattern layout, semantic color-coding (Red for Loss, Blue for Profit), and complex **DAX measures**.

---

## 📊 Key Business Insights & Findings

*(Note: Add your final dashboard screenshot here)*
`![Live E-Commerce Profitability Dashboard](insert_your_image_link_here)`

By plotting Total Net Revenue against Total Profit on a dual-axis chart, the following critical insights were uncovered:

1. 🚨 **The Discounting Disaster:** A staggering **48.65% of all orders resulted in a net financial loss**. The aggressive discounting strategy (>20% discounts) is directly cannibalizing company margins.
2. ⚠️ **The "High Revenue, High Loss" Trap:** The `Home & Furniture` category drove the highest overall Gross Revenue ($0.65M) but operated at a severe net loss (-$3.8K). Marketing spend here is yielding negative returns.
3. ✅ **The Silent Winner:** The `Beauty` category, despite having much lower sales volume ($0.56M), yielded the highest absolute profit ($18.1K), indicating strong product margins and minimal reliance on discounts.
4. **Overall Health:** The company's profit margin sits at an unsustainable **0.34%**, requiring immediate strategic intervention.

**💡 Actionable Recommendation:** Immediately halt discounts exceeding 15% in the 'Home & Furniture' and 'Sports' categories. Shift marketing budget to scale the highly profitable 'Beauty' segment.

---

## 🧮 Advanced DAX Measures Formulated
To achieve dynamic financial tracking, the following core DAX formulas were created:
```dax
Total Net Revenue = SUM('daily_sales_data'[net_sales])
Total Profit = SUM('daily_sales_data'[profit_amount])
Profit Margin % = DIVIDE([Total Profit], [Total Net Revenue], 0)
Loss Making Orders = CALCULATE(COUNTROWS('daily_sales_data'), 'daily_sales_data'[is_profitable] == "No")
⚙️ How to Run This Project Locally
1. Database Setup:
Create a local MySQL database named ecom_live_project.

SQL
CREATE DATABASE ecom_live_project;
2. Install Dependencies:
Open your terminal and install the required Python libraries:

Bash
pip install pandas numpy sqlalchemy mysql-connector-python
3. Run the ETL Pipeline:
Execute the Python script to fetch, clean, and load today's batch of data into the database. Run it multiple times to simulate a growing data warehouse.

Bash
python live_etl_pipeline.py
4. Connect to Power BI:
Open the .pbix file. Go to Transform Data > Data Source Settings and update the MySQL credentials to match your local host (localhost and root). Refresh to visualize the live data!
