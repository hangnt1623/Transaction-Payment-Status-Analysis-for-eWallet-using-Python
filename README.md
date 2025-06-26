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

Before EDA, Merge payment_report with product

**2.1. EDA payment_enriched**

**Step 1: Understand dataset**
(1) Use "df.shape" to immediately know the number of transactions (rows) and types of information (columns) you're dealing with.
```python
'Size payment_enriched: ', payment_enriched.shape
```
*Result*: 919 rows, 7 columns

(2) Use "df.info()" to provides a quick summary of the structure and quality of payment_enriched
```python
payment_enriched.info()
```
*Result*

<img width="261" alt="{81964A1C-94F8-49EB-A89A-1CE1C8EAE79C}" src="https://github.com/user-attachments/assets/269cfa32-8638-4dcf-940d-df0309e3ffb6" />


(3) Use "df.head(10)" to visually inspect the actual data for the first 10 transactions
```python
payment_enriched.head(10)
```

(4) Use "df.describe()" to get a quick statistical summary of numerical columns
```python
payment_enriched.describe()
```


**Step 2: Inspect & Valid dataset**

- First of all, use ProfileReport to generate automated Exploratory Data Analysis (EDA) reports for payment_enriched, this provide a quick overview of data quality, missing values, and variable distribution, helping you understand your payment_enriched data immediately
```python
profile = ProfileReport(payment_enriched, title="EDA payment_enriched Report", explorative=True)
profile.to_notebook_iframe()
```

***(1) Data type***

Checking data types identifies data's nature and initial quality issues, while changing them ensures correct operations, optimized performance, and compatibility for analysis.

- Change datatype of report_month to DATETIME
- Change datatype of payment_group, category, team_own to CATEGORY -> can be classified

*Before*

<img width="151" alt="{181F41F6-5183-4EA7-9D40-B9401E631AD9}" src="https://github.com/user-attachments/assets/33c435f3-f1de-4ddd-add9-9ba90819f865" />

*After*

<img width="180" alt="{4890C34C-230F-4619-A761-8A98FBA06447}" src="https://github.com/user-attachments/assets/c3c44140-e14e-4385-9049-265800ee20db" />


***(2) Missing Value***

Checking missing values helps identify data quality issues and incompleteness. Handling them prevents analysis errors, ensures accurate results, and improves model performance by providing complete data.

- 'Category' missing data -> add "Unknown"
- 'Team_own' missing data -> add "Uncategorized"

*Before*

<img width="135" alt="{5E03B555-2393-4F4F-8B4A-26B21D8C0F28}" src="https://github.com/user-attachments/assets/14e8b5ee-a57b-4e36-858d-8314a9b93edc" />

*After*

<img width="121" alt="{56188132-8461-40B0-92BE-D6468870FC41}" src="https://github.com/user-attachments/assets/121a392e-7810-445b-a384-18e22ba7429a" />


***(3) Unique Value***

- Understand Categorical Variables: See distinct categories and their counts for analysis
- Verify Identifier Columns: Confirm uniqueness of IDs and spot duplicates
- Aid Data Cleaning & Preprocessing: Identify inconsistencies or values needing standardization

<img width="136" alt="{F90DF29A-2558-4D7F-ACA4-BEF94A5DB896}" src="https://github.com/user-attachments/assets/78a823a2-e76d-4875-a6fd-1aeed795caf8" />


***(4) Duplicate Value***

Checking duplicates is to find redundant or erroneous entries, while removing them is to ensure accurate analysis, prevent bias, and maintain data integrity.

Duplicates: 0 row -> Next step: No action

***(5) Outliers***

Check outliers is to find unusual or extreme data points that can skew results, while dealing with them is to prevent data distortion, ensure accurate analysis, and improve model performance.

*Result*: 142 rows x 7 columns -> Next step: no action

***(6) Distribution***

Checking distribution is to understand how data values are spread and concentrated, detect outliers, guide data transformations, and inform model selection for better analysis.

<img width="488" alt="{27B15992-1E59-41D4-A048-DBB9DBB5CB97}" src="https://github.com/user-attachments/assets/d92d46f4-acf8-4025-92f1-e29660a88c0e" />

*Observations*:
- Symmetrical Distribution: The volume data, after log transformation, shows a largely symmetrical, bell-shaped distribution.
- Central Tendency: Most transformed volumes are concentrated around the 16-17 mark (log scale).
- Typical Range: The majority of volumes fall within the 12 to 20 range on the log scale.

**2.2. EDA transactions**

**Step 1: Understand dataset**

*Size* -> 1324002 rows, 9 columns
```python
'Size transactions: ', transactions.shape
```

*General information*
```python
transactions.info()
```

*10 first lines*
```python
transactions.head(10)
```

*Descriptive Statistics*
```python
transactions.describe()
```

**Step 2: Inspect & Valid dataset**

- First of all, use ProfileReport to generate automated Exploratory Data Analysis (EDA) reports for 'transactions'
```python
profile = ProfileReport(transactions, title="EDA Transactions Report", explorative=True)
profile.to_notebook_iframe()
```

***(1) Data type***

- Change datatype of transType & transStatus -> category -> classification
- Change datatype of sender_id & receiver_id -> int64 -> id in integer form
- Change datatype of extra_info -> string -> explain the information to be filled in
- Change datatype of timeStamp -> datetime because timeStamp is time

*Before*

<img width="132" alt="{5F368968-5D21-4229-BB83-33B41C877AAC}" src="https://github.com/user-attachments/assets/c7b72dd9-165b-402e-acd8-f1e708413011" />

*After*

<img width="176" alt="{11753A2E-1F0E-4CB8-AA75-32AC023DF929}" src="https://github.com/user-attachments/assets/23aa1ceb-2034-4bbf-8930-39717b3458d8" />


***(2) Missing Value***

- sender_id, receiver_id are missing data -> fill -1 to red flag
- extra_info has no data -> may be no additional information -> fill "No"

*Before*

<img width="140" alt="{FD737E9F-09A5-411D-8A3C-CDB264409625}" src="https://github.com/user-attachments/assets/26b0f1c0-0765-4174-bdf3-19de362eb9f1" />

*After*

<img width="118" alt="{5613BD02-4468-4B21-AAC1-ADA0708EFDB0}" src="https://github.com/user-attachments/assets/bf058fda-cf6f-4f6b-93a5-a98af89972fd" />


***(3) Unique Value***

<img width="152" alt="{55A1EDA5-9377-43E0-9598-7928D550A455}" src="https://github.com/user-attachments/assets/6590a2d9-e14b-4bb6-8a8f-d1c9af8d5530" />


***(4) Duplicate Value***

There are 28 duplicated rows -> Next step: delete these 28 rows

***(5) Outliers***

Result: 186889 rows x 9 columns -> Next step: No action


***(6) Distribution***

![image](https://github.com/user-attachments/assets/29ac1fd1-6e03-41ab-a741-ce7229f2fb0d)

*Observations*:
- Clearly Bimodal: The distribution shows two distinct peaks.
- Two Main Clusters: One smaller peak around log(1+Volume) 6-7, and a much larger, dominant peak around 9.5-11.5.

3️⃣ **Data Wrangling & Analysis**

❓Identify top-performing products by volume
```python
#Top 3 product_ids with the highest volume
top_3_products = (
    payment_enriched
    .groupby('product_id')['volume']
    .sum()
    .sort_values(ascending=False)
    .head(3)
)

top_3_products
```
*Result*
<img width="153" alt="{399E159B-EDBE-4294-8C82-BC8A698E07E2}" src="https://github.com/user-attachments/assets/780d4eb2-fbe3-41cf-be99-44a5b2ac2d15" />

❓Detect ownership rule violations (1 product → 1 team rule)
```python
## Step 1: Count unique team_own per product_id
product_team_counts = payment_enriched.groupby('product_id')['team_own'].nunique()

## Step 2: Filter product_ids with more than 1 owning team
abnormal_products = product_team_counts[product_team_counts > 1].index

## Step 3: Show full records of abnormal products
payment_enriched[payment_enriched['product_id'].isin(abnormal_products)]
```
*Result*: no abnormal products against this rule

❓Determine lowest-performing team since Q2 2023 and their weakest category
```python
## Step 1: Filter data Q2/2023
df_q2_2023 = payment_enriched[payment_enriched['report_month'] >= '2023-04']
df_q2_2023
#lowest performance -> team?
lowest_performance_team = payment_enriched[payment_enriched['report_month'] >= '2023-04'].groupby(by = 'team_own')['volume'].sum().sort_values(ascending = True).head(1)
lowest_performance_team
#category contribute the least?
worst_cate = payment_enriched[(payment_enriched['report_month'] >= '2023-04') & (payment_enriched['team_own'] == 'APS')].groupby(by = 'category')['volume'].sum().sort_values(ascending = True).head(1)
worst_cate
```
*Result*

<img width="119" alt="{F1DF481A-2A0D-4033-8906-315E446E2D5A}" src="https://github.com/user-attachments/assets/2bf35dad-6841-42c5-910e-4a325803b473" />

<img width="114" alt="{81142A9A-DD27-47ED-83F3-945FA435A5EB}" src="https://github.com/user-attachments/assets/8480a167-c6f4-4b54-a56e-51ab6250b8f9" />


❓Analyze source_id contributions to refund volume
```python
# Filter refund transactions
df_refund = payment_enriched[payment_enriched['payment_group'] == 'refund']

# Calculate total volume by source_id
refund_contribution = df_refund.groupby('source_id')['volume'].sum()

# Find source_id with largest volume
top_source_id = refund_contribution.idxmax()
top_volume = refund_contribution.max()

# Calculate the contribution % of each source_id
refund_contribution_pct = refund_contribution / refund_contribution.sum() * 100

print(f"Source_id with highest refund contribution: {top_source_id}")
print(f"Volume: {top_volume:,}")
print(f"Contribution: {refund_contribution_pct[top_source_id]:.2f}%")
```
*Result*
- Source_id with highest refund contribution: 38
- Volume: 36,527,454,759
- Contribution: 59.11%

❓Categorize each transaction into defined transaction_type
```python
conditions = [
    (transactions['transType'] == 2) & (transactions['merchant_id'] == 1205),
    (transactions['transType'] == 2) & (transactions['merchant_id'] == 2260),
    (transactions['transType'] == 2) & (transactions['merchant_id'] == 2270),
    (transactions['transType'] == 2),
    (transactions['transType'] == 8) & (transactions['merchant_id'] == 2250),
    (transactions['transType'] == 8)
]

transaction_types = [
    'Bank Transfer Transaction',
    'Withdraw Money Transaction',
    'Top Up Money Transaction',
    'Payment Transaction',
    'Transfer Money Transaction',
    'Split Bill Transaction'
]

transactions['transaction_type'] = np.select(conditions, transaction_types, default='Invalid Transaction')
```

❓For each valid type, compute: Number of transactions, Total transaction volume, Unique senders and receivers
```python
## Filter out invalid transactions
valid_df = transactions[transactions['transaction_type'] != 'Invalid Transaction']

## Group and aggregate
summary = valid_df.groupby('transaction_type').agg(
    num_transactions=('transaction_id', 'count'),
    total_volume=('volume', 'sum'),
    num_senders=('sender_id', pd.Series.nunique),
    num_receivers=('receiver_id', pd.Series.nunique)
).reset_index()

summary
```
*Result*

<img width="516" alt="{4CB13042-F05A-4CFB-887C-E026553321A0}" src="https://github.com/user-attachments/assets/01f13e2a-0644-4e0e-a9a2-d79216f3e8a2" />


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










