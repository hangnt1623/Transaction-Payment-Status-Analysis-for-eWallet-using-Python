# Python_Fintech_eWallet Transaction & Payment Status

---
![image](https://github.com/user-attachments/assets/cb5cd69b-87d2-4ae1-99d3-5ab8504c9099)

- Author: Nguyen Thuy Hang 
- Date: 2025-06-22
- Tools Used: Python  

---

## 📑 Table of Contents  
1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)  
3. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

---

## 📌 Background & Overview  

### Objective:
### 📖 What is this project about? What Business Question will it solve?

This project focuses on analyzing payment and transaction data in the context of an e-wallet (digital wallet) company. The goal is to understand user behavior, product performance, and operational issues related to digital payments and transfers.

This project helps the company answer the following key business questions:

✔️ Top Products by Volume (Which are the top 3 product_ids with the highest total payment volume?)

✔️ Ownership Rule Violation (Are there any product_ids assigned to more than one team, violating the rule that each product should be owned by only one team?)

✔️ Lowest-Performing Team Analysis (Since Q2 2023, which team has the lowest payment volume, and what is the least contributing category under that team?)

✔️ Refund Analysis (Among refund transactions, which source_id contributes the most?)

✔️ Transaction Classification (How can we categorize transactions by type? How many are invalid?)

✔️ Transaction Type Summary (For each valid type, what are the totals for transactions, volume, senders, and receivers?)


### 👤 Who is this project for?  
This project is designed for:

✔️ Data Analysts & Data Scientists (to build insights and monitor performance of payments and transactions in an e-wallet environment)

✔️ Product & Business Teams (to track product performance, detect anomalies, and identify growth opportunities across categories and teams)

✔️ Finance & Operations Teams (to monitor transaction health, refund volumes, and ensure operational integrity)

✔️ Leadership & Strategy Teams (to make informed decisions based on payment volume trends, underperforming areas, and user behaviors)

---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  
- Source: Dataset is obtained from Kaggle (payment_report.csv, product.csv, transactions.csv)
- Size:
  + payment_report.csv (919 rows, 5 columns)
  + product.csv (492 rows, 3 columns)
  + transactions.csv (1323974 rows, 10 columns)
- Format: .csv 

### 📊 Data Structure & Relationships  

#### 1️⃣ Tables Used:  
There are 3 tables are in the dataset: payment, product, transactions
#### 2️⃣ Table Schema & Data Snapshot  

**Table 1: Payment**

*Table schema*

| Column Name | Data Type | Description |  
|-------------|----------|-------------|  
| report_month  | object    | Report month |  
| payment_group        | object   | Payment group |  
| product_id    | int64     | Product identification |  
| source_id       | int64    | Source identfication |  
| volume      | int64    | Volume |  

*Table snapshot*

<img width="403" alt="{95A8520A-7A66-4EE2-8807-9659D4C94ADF}" src="https://github.com/user-attachments/assets/40e5df55-3c83-47d3-9b1f-6172b7031a23" />


**Table 2: Product**

*Table schema*

| Column Name | Data Type | Description |  
|-------------|----------|-------------|  
| product_id  | int64    | Product identification |  
| category      | object     | Product category |  
| team_own    | object    | Team own name |  

*Table snapshot*

<img width="226" alt="{2C4E9521-C852-4811-AEA0-DF54F24481CB}" src="https://github.com/user-attachments/assets/2bedb257-2378-4089-971e-18fe01aa1160" />

**Table 3: Transactions**

*Table schema*

| Column Name | Data Type | Description |  
|-------------|----------|-------------|  
| transaction_id  | int64    | Unique identifier for each product |  
| merchant_id        | int64   |ID of the merchant involved in the transaction |  
| volume    | int64     | Amount of money transferred in the transaction |  
| transType       |int64    | Type of transaction |  
| transStatus  | int64      | Status of the transaction |  
| sender_id        | float64   | ID of the user/account who initiated the transaction |  
| receiver_id    | float64   | PID of the user/account who received the funds |  
| extra_info       | object    | Additional notes related to the transaction |  
| timeStamp   | int64     | Date and time when the transaction occurred |  

*Table snapshot*

<img width="802" alt="{DF946F98-1E64-4A8A-B592-E629D0D0E570}" src="https://github.com/user-attachments/assets/0d2b86d8-6edc-46f6-a831-2a9fe9bbd7c9" />



---

## ⚒️ Main Process

1️⃣ **Define problem & data gathering**

**Define problem**

The e-wallet company lacks clear insights into how users engage with different payment products and transaction types, which limits its ability to identify underperforming teams, detect operational issues such as high refund rates, and optimize overall payment processes. This project aims to analyze transaction and payment data to uncover patterns, evaluate performance, and improve decision-making for better business outcomes.


**Data gathering**: Import dataset & libraries


2️⃣ **Exploratory Data Analysis (EDA)**

- Understand dataset & Merge payment_report with product

- Inspect & Valid dataset

3️⃣ **Data Wrangling & Analysis**

✔️ Identify top-performing products by volume

✔️ Detect ownership rule violations (1 product → 1 team rule)

✔️ Determine lowest-performing team since Q2 2023 and their weakest category

✔️ Analyze source_id contributions to refund volume

✔️ Categorize each transaction into defined transaction_type

✔️ For each valid type, compute: Number of transactions, Total transaction volume, Unique senders and receivers


## 🔎 Final Conclusion & Recommendations  

📌 Key Takeaways:

- Top products account for the highest payment volumes, allowing the company to focus efforts on optimizing these key products for revenue and user experience.

- No products violate the rule that each product_id is owned by only one team, ensuring clear product ownership and management.

- The team with the lowest performance since Q2 2023 has been identified, along with the category contributing the least to that team, enabling targeted improvement efforts.

- Refund transactions are heavily concentrated in source_id 38, which accounts for 59.11% of refund volume (~36.5 billion), indicating a critical area for operational review and risk mitigation.

- Transaction types have been accurately classified, supporting detailed analysis of user behavior and transaction flows.

- Summary statistics by transaction type—number of transactions, volume, unique senders and receivers—provide a comprehensive overview of platform activity

💡 Recommendations:

- Prioritize development and support for top-performing products to sustain and grow revenue.

- Maintain clear and strict product ownership rules to avoid management conflicts.

- Investigate the causes of underperformance for the identified team and category, and implement targeted improvements or support.

- Focus on analyzing and mitigating issues related to the main refund source (source_id 38) to reduce refund rates and improve customer satisfaction.

- Continue monitoring transaction types closely to detect trends or anomalies early, enabling proactive operational decisions.










