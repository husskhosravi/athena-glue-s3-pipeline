## 📦 Superstore Sales – End-to-End Data Pipeline on AWS

This project demonstrates my ability to build a **fully serverless data engineering pipeline** on AWS, covering everything from data ingestion to visual analytics using native AWS services. 
It uses the [Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final) sourced from Kaggle, containing retail transactional data across multiple categories such as customer orders, product segments, sales, profits, shipping methods and regions.

---

### 🚀 What I Built

I designed an automated pipeline to process incremental sales data from a retail superstore using the following AWS services:

- **S3** – Cloud storage for raw files
- **IAM** – Role-based access and user provisioning
- **Glue** – Metadata cataloguing via crawlers
- **Athena** – Serverless SQL querying on S3 data
- **QuickSight** – Interactive dashboard for analysis

---

### 🧱 Architecture

```
Raw CSV → S3 (Partitioned by snapshot_day)
           ↓
     AWS Glue Crawler → Data Catalog
           ↓
      Amazon Athena (SQL)
           ↓
    Amazon QuickSight Dashboards
```
![workflow](https://github.com/user-attachments/assets/6299b8f8-a586-40f5-99bf-6e1bc5736330)

---

### 🔧 Tech Stack

| Tool/Service | Purpose |
|--------------|---------|
| **S3** | Raw data storage (partitioned folders by snapshot date) |
| **Glue** | Metadata extraction and partition awareness |
| **Athena** | Ad hoc querying with cost-efficient scanning |
| **QuickSight** | Visual reporting and dashboard sharing |
| **IAM** | Secure access management and service-level roles |

---

### 📁 S3 Folder Structure

```
s3://<my-bucket>/orders/
  ├── snapshot_day=2017-01-01/
  │   └── orders_1.csv
  └── snapshot_day=2017-01-02/
      └── orders_2.csv
```

---

### 🧪 What I Did – Step by Step

#### ✅ IAM Setup
- Created a separate IAM user for development (no root user usage).
- Assigned admin permissions and created a Glue-compatible IAM role.

#### ✅ Data Upload to S3
- Cleaned and filtered daily order data from the Kaggle superstore dataset.
- Saved files in `.csv` format and uploaded to partitioned folders.

#### ✅ AWS Glue
- Created a **Glue database** to store metadata.
- Configured a **crawler** to:
  - Auto-detect schema
  - Recognise partitions using folder names
  - Maintain an up-to-date **data catalog** for Athena

#### ✅ Amazon Athena
- Connected Athena to the Glue data catalog
- Ran queries on raw S3 files *without loading into any database*
 
![query1](https://github.com/user-attachments/assets/5786ec63-6821-4c93-b428-80579a4dd739)

Partitioning by date drastically improves Athena query speed and reduces cost.

Using `WHERE snapshot_day = ...` reduced scanned data by **~54%**.
- Example query to show the improvement caused by partitioning:
```sql
SELECT * FROM "db_hkhosravi"."orders"
WHERE snapshot_day = '2017-01-01'
```
![query2](https://github.com/user-attachments/assets/0236a096-c66a-493a-9a30-77325ff6ee36)

#### ✅ Amazon QuickSight
- Connected QuickSight to Athena as the data source
- Imported data into SPICE for faster performance
- Built:
  - Bar chart: Sales by Category
  - Pie chart: Profit by City

![quicksight](https://github.com/user-attachments/assets/2f65b3e2-8ee6-4a15-a1a3-f627f909302a)

---

### 💡 Key Skills Demonstrated

- AWS IAM + security best practices  
- Efficient cloud data partitioning strategies  
- Glue crawler configuration for schema detection  
- Serverless querying with Athena  
- Real-time dashboards with QuickSight (SPICE vs live mode)

---

### 🏁 Next Steps

- Add trigger-based automation using AWS Lambda  
- Integrate with a real-time stream via Kinesis or EventBridge  
- Add notification layer for insights using SNS  
