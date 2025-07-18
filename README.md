# Transaction Payment Status Analysis for eWallet using Python

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

This project focuses on analyzing **payment** and **transaction data** in the context of an **e-wallet (digital wallet)** company. The goal is to understand **user behavior**, **product performance**, and **operational issues** related to **digital payments** and **transfers**.

This analysis helps the company answer the key business question:  
**"How can we optimize eWallet operations by identifying underperformance, rule violations, and key transaction patterns across products and teams?"**


### 👤 Who is this project for?  
This project is designed for:

✔️ Data Analysts & Data Scientists 

✔️ Product & Business Teams

✔️ Finance & Operations Teams 

✔️ Leadership & Strategy Teams 

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
#### 2️⃣ Table Schema

<details>
  <summary>📊 <strong>Table 1: Payment</strong></summary>

| Column Name | Data Type | Description |  
|-------------|----------|-------------|  
| report_month  | object    | Report month |  
| payment_group        | object   | Payment group |  
| product_id    | int64     | Product identification |  
| source_id       | int64    | Source identfication |  
| volume      | int64    | Volume |  
</details>

<details>
  <summary>📊 <strong>Table 2: Product</strong></summary>

| Column Name | Data Type | Description |  
|-------------|----------|-------------|  
| product_id  | int64    | Product identification |  
| category      | object     | Product category |  
| team_own    | object    | Team own name |  
</details>

<details>
  <summary>📊 <strong>Table 3: Transactions</strong></summary>
  
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
</details>


---

## ⚒️ Main Process

1️⃣ **Gata gathering**: Import dataset & libraries

2️⃣ **Exploratory Data Analysis (EDA)**

Before EDA, Merge payment_report with product => payment_enriched

<details>
  <summary> <strong>2.1. EDA payment_enriched</strong></summary>

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

*Before & After*

<img width="151" alt="{181F41F6-5183-4EA7-9D40-B9401E631AD9}" src="https://github.com/user-attachments/assets/33c435f3-f1de-4ddd-add9-9ba90819f865" />
<img width="180" alt="{4890C34C-230F-4619-A761-8A98FBA06447}" src="https://github.com/user-attachments/assets/c3c44140-e14e-4385-9049-265800ee20db" />


***(2) Missing Value***

Checking missing values helps identify data quality issues and incompleteness. Handling them prevents analysis errors, ensures accurate results, and improves model performance by providing complete data.

- 'Category' missing data -> add "Unknown"
- 'Team_own' missing data -> add "Uncategorized"

*Before & After*

<img width="135" alt="{5E03B555-2393-4F4F-8B4A-26B21D8C0F28}" src="https://github.com/user-attachments/assets/14e8b5ee-a57b-4e36-858d-8314a9b93edc" />
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
</details>

<details>
  <summary> <strong>2.2. EDA transactions</strong></summary>
  
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

*Before & After*

<img width="132" alt="{5F368968-5D21-4229-BB83-33B41C877AAC}" src="https://github.com/user-attachments/assets/c7b72dd9-165b-402e-acd8-f1e708413011" />
<img width="176" alt="{11753A2E-1F0E-4CB8-AA75-32AC023DF929}" src="https://github.com/user-attachments/assets/23aa1ceb-2034-4bbf-8930-39717b3458d8" />


***(2) Missing Value***

- sender_id, receiver_id are missing data -> fill -1 to red flag
- extra_info has no data -> may be no additional information -> fill "No"

*Before & After*

<img width="140" alt="{FD737E9F-09A5-411D-8A3C-CDB264409625}" src="https://github.com/user-attachments/assets/26b0f1c0-0765-4174-bdf3-19de362eb9f1" />
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
  </details>


----
3️⃣ **Data Wrangling & Analysis**

<details>
  <summary> <strong>❓Identify top-performing products by volume</strong></summary>

*Purpose*: To recognize which e-wallet products are driving the most transaction activity (by volume or value), helping prioritize investment, marketing, and feature development efforts.

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
| product_id | volume |
| --- | --- |
| 1976 | 61797583647 |
| 429 | 14667676567 |
| 372 | 13713658515 |

*Findings*
- Product 1976 accounts for the vast majority of payment volume, highlighting revenue concentration risk.
- Products 429 and 372 show substantial but significantly lower volumes, indicating room for growth.
- The steep volume disparity suggests prioritizing both risk management for top product and growth strategies for others.
</details>

<details>
  <summary> <strong>❓Detect ownership rule violations (1 product → 1 team rule)</strong></summary>

*Purpose*: To ensure operational clarity and accountability by identifying any products managed by multiple teams-violating the "one product, one team" policy-which may cause inefficiencies or conflicting responsibilities.

```python
## Step 1: Count unique team_own per product_id
product_team_counts = payment_enriched.groupby('product_id')['team_own'].nunique()

## Step 2: Filter product_ids with more than 1 owning team
abnormal_products = product_team_counts[product_team_counts > 1].index

## Step 3: Show full records of abnormal products
payment_enriched[payment_enriched['product_id'].isin(abnormal_products)]
```
*Result*: no abnormal products against this rule

*Findings*:
- No ownership conflicts were detected, confirming compliance with the one-product-one-team rule.
- This clear ownership structure helps maintain accountability and operational clarity.
</details>

<details>
  <summary> <strong>❓Determine lowest-performing team since Q2 2023 and their weakest category</strong></summary>

*Purpose*: To pinpoint underperformance at both the team and category level, enabling targeted interventions, retraining, or resource reallocation to improve outcomes.

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

*Findings*:
- The APS team has significantly underperformed since Q2 2023, recording the lowest transaction volume among all teams.
- Within APS, category PXXXXXB shows no transaction volume, indicating a potential gap or inactive product line.
- This combination suggests an urgent need to investigate APS’s portfolio and address dormant categories to boost overall performance.
</details>

<details>
  <summary> <strong>❓Analyze source_id contributions to refund volume</strong></summary>

*Purpose*: To understand which sources are generating the most refund activity, helping to identify potential issues like product defects, policy misuse, or fraud.

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

*Findings*
- Source_id 38 dominates refund transactions, contributing nearly 60% of the total refund volume.
- This concentration suggests potential systemic issues or risks associated with this source that warrant closer monitoring.
- Addressing refund causes linked to source_id 38 could substantially reduce overall refund costs and improve transaction quality.
</details>

<details>
  <summary> <strong>❓Categorize each transaction into defined transaction_type</strong></summary>

*Purpose*: To organize raw transaction data into meaningful categories (e.g., payment, transfer, refund), enabling more structured analysis and easier performance tracking across transaction types.

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
</details>

<details>
  <summary> <strong>❓For each valid type, compute: Number of transactions, Total transaction volume, Unique senders and receivers</strong></summary>

*Purpose*: To generate key operational metrics that reveal transaction behavior, customer engagement, and scale of usage per transaction type-informing decisions around product scaling, user experience, and fraud detection.

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
|  | transaction_type | num_transactions | total_volume | num_senders | num_receivers |
| --- | --- | --- | --- | --- | --- |
| 0 | Bank Transfer Transaction | 37879 | 50605806190 | 23156 | 9272 |
| 1 | Payment Transaction | 398665 | 71850608441 | 139583 | 113299 |
| 2 | Split Bill Transaction | 1376 | 4901464 | 1323 | 572 |
| 3 | Top Up Money Transaction | 290498 | 108605618829 | 110409 | 110409 |
| 4 | Transfer Money Transaction | 341173 | 37032880492 | 39021 | 34585 |
| 5 | Withdraw Money Transaction | 33725 | 23418181420 | 24814 | 24814 |

*Findings*
- Top Up Money Transactions lead in total volume (~108.6B) despite having fewer transactions than Payment and Transfer types, indicating high-value reload activity.
- Payment Transactions have the highest number of transactions (398,665) and broad sender/receiver participation, reflecting their role as the core transaction type.
- Split Bill Transactions show very low volume and participation, suggesting this feature is either niche or underutilized.
</details>

## 🔎 Final Conclusion & Recommendations  

📌 **Key Takeaways:**

- Top products account for the highest **payment volumes**, allowing the company to focus efforts on optimizing these key products for **revenue** and **user experience**.
- No products violate the rule that each **product_id** is owned by only one **team**, ensuring clear **product ownership** and **management**.
- The team with the **lowest performance** since **Q2 2023** has been identified, along with the **category contributing the least**, enabling **targeted improvement** efforts.
- **Refund transactions** are heavily concentrated in **source_id 38**, which accounts for **59.11%** of refund volume (~**36.5 billion**), indicating a critical area for **operational review** and **risk mitigation**.
- **Transaction types** have been accurately **classified**, supporting detailed analysis of **user behavior** and **transaction flows**.
- Summary statistics by **transaction type** — **number of transactions**, **volume**, **unique senders** and **receivers** — provide a comprehensive overview of **platform activity**.


💡 **Recommendations:**

- **Prioritize** development and support for **top-performing products** to sustain and grow **revenue**.
- Maintain **clear** and **strict product ownership rules** to avoid **management conflicts**.
- **Investigate** the causes of **underperformance** for the identified **team** and **category**, and implement **targeted improvements** or **support**.
- Focus on analyzing and **mitigating** issues related to the main **refund source** (**source_id 38**) to reduce **refund rates** and improve **customer satisfaction**.
- Continue **monitoring** **transaction types** closely to detect **trends** or **anomalies** early, enabling **proactive operational decisions**.











