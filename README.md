# Credit Default Summary – Azure Data Pipeline + Power BI

End-to-end analytics project showing how to move raw credit data into Azure,
transform it, and visualise **credit default risk** with **Power BI**.

---

## 🎯 Business Goal

Lenders want to understand:

- Which clients are most likely to **default**
- How defaults vary by **education** and **gender**
- Overall **default rate** and number of **defaulted clients**

This project builds a small, realistic pipeline + dashboard to answer those questions.

---

## 🏗 Architecture (Azure)

Resources used in my Azure subscription:

- **Resource Group:** `data-pipeline-rg`
- **Azure Data Factory (V2):** `nancyadf01`
- **Azure Storage / Data Lake:** `nancyadls01`
- **Reporting:** Power BI Desktop (`Credit_Default_Summary.pbix`)

High-level flow:

```text
Source CSV (client credit data)
        │
        ▼
Azure Data Factory (nancyadf01)
  - Copy data into Azure Storage
        │
        ▼
Azure Storage / Data Lake (nancyadls01)
  - Stores cleaned/analysis-ready data
        │
        ▼
Power BI – Credit Default Summary
  - Data model + measures
  - Interactive visuals for stakeholders
