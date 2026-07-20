# Automated E-Commerce Data Pipeline & Financial Dashboard

A robust, production-style ETL (Extract, Transform, Load) data pipeline and interactive dashboard built using **Excel Power Query** and **Pivot Tables**. This project automates the aggregation, chronological sorting, and harmonization of monthly batch files to calculate true operational net profit.

---

## 📌 Project Overview & Business Case
In modern e-commerce and drop-shipping operations, data is heavily siloed. Store owners typically receive separate transactional exports from multiple platforms:
* **Sales Data:** Top-line revenue metrics (e.g., Shopify CSV exports).
* **Fulfillment Data:** Logistical and shipping costs from 3PL/shipping providers.
* **Marketing Data:** Advertising expenditures (e.g., Meta or Google Ads).

Manually consolidating these files to calculate actual net profit is highly inefficient and error-prone. This project engineers an automated solution where a user simply drops raw monthly batch exports into designated folders, and the underlying data pipeline dynamically ingests, combines, and transforms the data into actionable daily and monthly financial insights.

---

## 🏗️ Data Architecture & Pipeline Design
The project implements a **Folder-Ingestion Logic** rather than connecting to flat files. This ensures that as new monthly files are added, the system automatically aggregates them upon refreshing.

### 📂 Directory Structure
* `📂 Client_Project_Alpha` (Root Master Folder)
  * `📂 Sales_Input` ➔ Ingests monthly sales CSVs
  * `📂 Shipping_Input` ➔ Ingests monthly logistics cost CSVs
  * `📂 Ads_Input` ➔ Ingests monthly marketing spend CSVs
  * `📄 Power Query.xlsx` ➔ The data engine and front-end dashboard

  <img width="1918" height="1018" alt="image" src="https://github.com/user-attachments/assets/6c112435-5ac2-4ce3-ba80-1731757c6690" />
  <img width="1918" height="1018" alt="image" src="https://github.com/user-attachments/assets/3748c101-f653-48c2-b591-3a94e6af385e" />

---

## 🛠️ Step-by-Step ETL Implementation

### 1. Gross Profit Query & Chronological Sorting
* Connected Power Query to the `Sales_Input` and `Shipping_Input` folders to handle dynamic file stacking.
* Merged the sales and shipping queries using relational keys to align fulfillment costs directly with order records.
* **Automated Data Sequencing:** Because files are dumped into folders as unsorted monthly batches, I implemented an explicit date-sorting step directly inside the Power Query engine. This guarantees chronological integrity across the entire data timeline automatically.
* **Business Logic Optimization:** To simulate a realistic e-commerce environment, the pipeline models a **"Free Shipping"** consumer strategy[cite: 1]. Revenue is captured purely from product sales, while internal fulfillment fees are dynamically deducted to calculate **Gross Profit**.
* Extracted date dimensions by injecting a calculated `Month` column into the schema using Power Query date functions.

<img width="1918" height="1020" alt="image" src="https://github.com/user-attachments/assets/5487b538-a474-42ea-bb6e-ddf8c0d5f795" />



### 2. Net Profit & Performance Marketing Query (Granular Aggregation)
* **The Granularity Challenge:** Sales and shipping data occur at the individual transaction level, whereas advertising expense is reported as a daily summary[cite: 1]. 
* To resolve this mismatch, I built a secondary staging query that **grouped sales data by date**, aggregating daily revenue.
* Merged the aggregated sales table with the chronologically sorted daily `Ads_Input` data.
* Calculated **Net Profit** by subtracting daily ad spend from daily gross profit.
* Engineered a custom **ROAS (Return on Ad Spend)** metric using conditional logic (`if Spend = 0 then 0 else Revenue / Spend`) to safeguard the pipeline against zero-division errors.

  <img width="1918" height="1020" alt="image" src="https://github.com/user-attachments/assets/67484ece-e185-409b-b9f0-147fc3a01262" />

### 3. Data Modeling & Analytics Layer
Loaded the optimized data tables into Excel's analytical layer to construct targeted Pivot Tables:
* **Product Performance:** Product-wise breakdown of gross sales volumes.
* **Marketing Efficiency:** Average ROAS tracked against daily and monthly revenue.
* **Operational Profitability:** Net profit tracked per day alongside monthly aggregate KPI metrics.

---

## 📊 Interactive Financial Dashboard
Using the underlying data models, I engineered an interactive executive dashboard utilizing custom charts, Key Performance Indicators (KPIs), and interactive **Slicers/Filters** for seamless temporal and categorical cross-filtering.

<img width="1862" height="797" alt="Screenshot 2026-05-10 132320" src="https://github.com/user-attachments/assets/8b7a41ec-d8d3-4a12-8c1b-107157a05a08" />



---

## ⚡ Key Takeaways & Technical Hurdles Overcome
* **Dynamic File Handling:** Bypassed rigid file path restrictions by shifting to a folder-ingestion pipeline, ensuring seamless updates when dropping in new monthly logs.
* **Granularity Matching:** Solved a classic data engineering challenge by grouping transactional data to match the daily grain of marketing spends without losing financial accuracy.
* **Business Acumen:** Successfully translated top-line platform revenue into standard corporate financial metrics (Net Sales ➔ Gross Profit ➔ Operating Margin/Net Profit).
  * **Data Scale & Horizon:** The pipeline is initialized using a 2-month baseline of historical data (April - May 2026). Because the architecture uses dynamic folder ingestion rather than static file links, the system is fully production-ready to scale across infinite historical periods seamlessly upon folder refresh.
