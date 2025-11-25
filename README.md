Credit Default Summary – Azure Data Pipeline (Medallion Architecture) + Power BI

A complete end-to-end Azure Data Engineering project demonstrating ingestion, transformation, aggregation, and reporting of credit-risk data using:

Azure Data Factory (ADF)

Azure Data Lake Storage Gen2 (ADLS)

Medallion Architecture (Bronze → Silver → Gold)

Power BI for insights & reporting

This project uses real public data from the UCI Default of Credit Card Clients dataset.

🎯 1. Business Objective

Credit lenders need to understand:

Which clients are most likely to default

How default rates differ across education, gender, and client groups

Key performance indicators (KPIs) such as:
✔ Total Clients
✔ Total Defaults
✔ Average Default Rate

This project builds a production-style Azure data pipeline and a Power BI dashboard to deliver those insights.

🏗 2. End-to-End Architecture
                   ┌───────────────────────────────┐
                   │     UCI Credit Dataset        │
                   └───────────────┬───────────────┘
                                   │
                            Ingestion (ADF)
                                   │
        ┌──────────────┬───────────┴──────────────┬──────────────┐
        │              │                          │              │
     Bronze         Silver                      Gold          Power BI
   Raw data     Cleaned data             Aggregated data     Analytics layer
 Credit_Clients  silver_credit_clients   credit_summary      Dashboard & KPIs

Azure Components Used
Azure Service	Name	Purpose
Resource Group	data-pipeline-rg	Central container for project resources
Azure Data Factory	nancyadf01	Pipelines + Data Flows
ADLS Gen2	nancyadls01	Stores Bronze, Silver and Gold datasets
Power BI Desktop	Credit_Default_Summary.pbix	Visual analytics
🗂 3. Dataset (Bronze Layer)
Source:

Default of Credit Card Clients Dataset – UCI Machine Learning Repository
🔗 https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients

Raw file used (Bronze):
data/bronze/Credit_Clients.csv


30,000 client records

Variables:
SEX, EDUCATION, LIMIT_BAL, PAY_0..PAY_6, BILL_AMT1..6, PAY_AMT1..6, and Y (default flag)

🔄 4. Data Pipelines (Azure Data Factory)

This project uses three pipelines:

4.1 pl_copy_raw_to_bronze

Copies raw data (Credit_Clients.csv) into the Bronze container of ADLS.

4.2 pl_run_df_bronze_to_silver

Executes the Bronze → Silver Data Flow:

Cleans column names

Selects required fields

Removes invalid/null rows

Writes cleaned output to Silver layer as:

data/silver/silver_credit_clients.csv

4.3 pl_run_df_silver_to_gold_credit_summary

Executes Silver → Gold Data Flow:

Groups by education and gender

Calculates:

total_clients

total_defaults

default_rate

Creates readable labels

Writes final aggregated output to:

data/gold/credit_summary.csv

⚙️ 5. Data Transformations (ADF Data Flows)
5.1 Bronze → Silver Data Flow (df_bronze_to_silver)

Steps:

Source: srcBronzeCreditClients
→ Reads Credit_Clients.csv

selectColumns:
→ Renames UCI raw columns & selects relevant attributes

filter1:
→ Removes rows with invalid/blank client_id

sinkSilverCreditClients:
→ Saves output to silver_credit_clients.csv

5.2 Silver → Gold Data Flow (df_silver_to_gold_credit_summary)

Steps:

Source: Cleaned Silver dataset

aggDefaultByEducation:

Groups data by education and sex

Computes totals & default metrics

derivedColumn1:

Assigns readable labels (gender_label, education_label)

sinkGoldCreditSummary:
→ Saves aggregated output to credit_summary.csv

📂 6. Medallion Storage Layout
data/
   ├── bronze/
   │     └── Credit_Clients.csv
   │
   ├── silver/
   │     └── silver_credit_clients.csv
   │
   ├── gold/
   │     └── credit_summary.csv
   │
   └── logs/
         └── nancyadf01_Pipeline runs.csv


This structure ensures traceability, cleanliness, and layered quality improvements.

📊 7. Power BI Dashboard
File
powerbi/Credit_Default_Summary.pbix

Key KPIs

12K total clients

2,873 defaults

15.18% average default rate

Included Visuals

Bar chart: Average Default Rate by Education Level

Stacked bar: Education × Gender Default Comparison

KPI cards: totals and percentages

Slicer for gender-based filtering

📈 8. DAX Measures (Examples)
Total Clients = COUNTROWS('Credit')

Total Defaults =
CALCULATE(COUNTROWS('Credit'), 'Credit'[Y] = 1)

Default Rate =
DIVIDE([Total Defaults], [Total Clients])

🚀 9. How to Reproduce This Project
Clone the repository
git clone https://github.com/NancySushma/credit-summary-dashboard.git
cd credit-summary-dashboard

Upload data to ADLS Bronze folder

Upload:

Credit_Clients.csv

Run pipelines in ADF

pl_copy_raw_to_bronze

pl_run_df_bronze_to_silver

pl_run_df_silver_to_gold_credit_summary

Open Power BI

Load the Gold dataset

Refresh the data

Explore the visuals

📝 10. Professional Summary (Add to CV)

Built a complete Azure Data Engineering pipeline using ADF and ADLS Gen2.

Implemented Medallion Architecture (Bronze, Silver, Gold) for structured data refinement.

Designed Data Flows for cleaning, transforming, aggregating and modelling credit risk data.

Developed an interactive Power BI dashboard analyzing default trends by demographic factors.

Delivered a cloud-ready, reproducible, analytics-driven risk scoring solution.

🏁 11. Status

✔ Completed
✔ Cloud-ready
✔ Portfolio & interview-ready