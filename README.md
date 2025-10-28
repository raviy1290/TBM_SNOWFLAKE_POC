# Vibe coding - TBM_SNOWFLAKE_POC
# 💰 Technology Business Management (TBM) on Snowflake — MVP

This repo contains a **minimal but extensible TBM cost transparency pipeline** built on **Snowflake**.  
It ingests raw enterprise cost data (Cloud, SaaS, Labor), normalizes it, maps it to **Cost Pools** and **IT Towers**, and finally allocates costs to **Applications / Business Units (BUs)** to provide **Total Cost of Ownership (TCO)**.

---

## 🚀 What This Project Does

- **Raw Ingestion**: Load costs from different sources (Cloud billing CSVs, SaaS invoices, Payroll/HR data).  
- **Normalization**: Standardize into a unified `COST_EVENT` schema.  
- **Cost Pools**: Categorize into `CLOUD`, `SAAS`, `LABOR`.  
- **IT Towers**: Map into towers like `COMPUTE`, `STORAGE`, `ENG_LABOR`, `BUSINESS_SAAS`, etc.  
- **Allocation**: Distribute tower costs into Apps/BUs using:
  - Direct allocation
  - Percentage rules
  - Usage-based proportional splits
- **Reporting**:
  - Spend by **Cost Pool** (Finance view)
  - Spend by **IT Tower** (CIO view)
  - TCO by **Application / BU** (Business view)

---

## 🏗️ Architecture
[RAW_CLOUD ---> CLOUD_RAW_DAILY]
[RAW_SAAS                      ]  -----> [COST_EVENT] --> [ALLOCATED_COST] --> [Reporting Views]
[RAW_LABOR                     ]

COST_POOL (implicit: Cloud, SaaS, Labor)
COST_TOWER (lookup & rules)
BUSINESS_ENTITY (Apps/BUs)
ALLOCATION_RULE (allocation configs)







---

## 📂 Tables

### Raw Layer
- `RAW_CLOUD` — AWS CUR / Azure / GCP raw billing rows.  
- `RAW_SAAS` — SaaS invoices (Salesforce, Snowflake, Slack, Databricks, etc.).  
- `RAW_LABOR` — Payroll / HR data.

### Staging
- `CLOUD_RAW_DAILY` — Aggregated Cloud costs (daily granularity).

### Normalized
- `COST_EVENT` — Canonical unified fact table.

### Allocation
- `BUSINESS_ENTITY` — Applications or BUs.  
- `ALLOCATION_RULE` — Rules to distribute tower costs.  
- `ALLOCATED_COST` — Final allocated cost fact table.

---

## 🔑 Allocation Methods

| Tower           | Allocation Method | Example Metric                        |
|-----------------|-------------------|---------------------------------------|
| Compute         | Usage             | CPU hours, Snowflake queries           |
| Storage         | Usage             | GB consumed, S3 objects                |
| Database (RDS)  | Direct            | DB tagged to AppB                      |
| Network         | Usage             | Traffic per app                        |
| Security (KMS)  | Percentage        | Equal split across apps                 |
| Eng Labor       | Direct            | Employee → App team                     |
| Ops Labor       | Percentage        | Split across apps                       |
| Shared Labor    | Percentage        | Predefined split (e.g., 50/50 AppA/B)  |
| Business SaaS   | Usage             | Seat counts per BU                      |
| Collab SaaS     | Usage             | Active users per BU                     |

---

## ⚙️ Example Queries

**Spend by Cost Pool**
```sql
select cost_pool, sum(normalized_amount) as spend
from COST_EVENT
group by cost_pool;
