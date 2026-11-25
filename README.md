PhonePy Payment Insights Dashboard (Power BI)

An interactive Power BI dashboard analysing 300,000 UPI-style transactions across ~108K users for the year 2024. It covers transaction volume and value trends, service mix, payment success/failure, user demographics, and top users.

Note: The dataset is synthetic / simulated. It is not real PhonePe data, and the patterns in it should not be read as real-world UPI behaviour.

Dashboard Preview

Table of Contents
Project Overview
Dataset
Dashboard Features# PhonePe Payment Insights Dashboard (Power BI)

An interactive Power BI dashboard analysing **300,000 UPI-style transactions** across **~108K users** for the year 2024. It covers transaction volume and value trends, service mix, payment success/failure, user demographics, and top users.

> **Note:** The dataset is synthetic / simulated. It is not real PhonePe data, and the patterns in it should not be read as real-world UPI behaviour.

![Dashboard Preview](images/dashboard_preview.png)

---

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Dashboard Features](#dashboard-features)
- [Key Metrics](#key-metrics)
- [Findings](#findings)
- [Known Limitations](#known-limitations)
- [Tech Stack](#tech-stack)
- [Repository Structure](#repository-structure)
- [How to Run](#how-to-run)
- [Future Improvements](#future-improvements)

---

## Project Overview

The goal is to turn raw payment records into a single-page dashboard that answers:

- How many transactions are happening, and how much money moves through the platform?
- Which services (Money Transfer, Loans, Insurance, Recharge & Bills) drive volume vs. value?
- How reliable are payments, and why do they fail?
- Which age groups and users contribute most?
- How does activity change month to month?

---

## Dataset

File: `Phonepe-Final-Dataset.xlsx` (2 sheets)

**`All_Transactions`** (300,000 rows)

| Column | Description |
|---|---|
| `Transaction_ID` | Unique transaction identifier |
| `Amount` | Transaction amount (INR), range 20 – 99,999 |
| `User_ID` | Links to `All_Users` |
| `Service` | Money_Transfer, Recharge_Bills, Loans, Insurance |
| `Service Type` | Sub-category (e.g. To UPI ID, DTH, Gold Loan, Health) |
| `Payment_Status` | Successful, Failed, Wrong PIN, Server error, Insufficient amount |
| `Reason` | Outcome reason (adds Wrong Info and Bank Denied) |
| `Date` | Transaction date (1 Jan 2024 – 30 Dec 2024) |

**`All_Users`** (107,658 rows)

| Column | Description |
|---|---|
| `User_ID` | Unique user identifier |
| `Name` | User name |
| `Age` | Age (18 – 60) |
| `Join_Date` | Date the user joined |

**Service breakdown**

| Service | Service Types |
|---|---|
| Money Transfer (150K txns) | To UPI ID, To Self Account, To Mobile Number, To QR Code |
| Loans (50K) | Bike Loan, Credit Score, Gold Loan, Mutual Fund |
| Insurance (50K) | Bike, Car, Health, Term Life |
| Recharge & Bills (50K) | Mobile Recharge, DTH, Cable TV, FASTag Recharge |

---

## Dashboard Features

**Page 1: Overview**
- KPI cards: Total Transactions, Total Value, Unique Users, Successful Rate, with MoM growth
- Transactions Over Time (volume vs. value, by month)
- Age Segment Contribution (Gen Z, Millennials, Gen X, Boomers)
- Service Transaction Value Analysis
- Top 5 Users by transaction value
- Weekday vs. Weekend usage
- Slicers: Month and Payment Status

**Page 2 (drill-through / tooltip):** Total Transactions by Service Type

**Page 3 (drill-through / tooltip):** Total Transaction Value by Service Type

---

## Key Metrics

| Metric | Value |
|---|---|
| Total Transactions | 300,000 |
| Total Transaction Value | ₹3,474.3M |
| Unique Users | ~108K |
| Successful Rate | 96.00% |

**Value by service:** Loans ₹2.5bn · Insurance ₹0.5bn · Money Transfer ₹0.4bn · Recharge & Bills ₹0.1bn

**Volume:** Money Transfer accounts for 50% of transactions (150K), but only about 11% of value. Loans account for roughly 72% of value from 1/6 of transactions.

**Failure breakdown (by transaction count):** Server error (4,053), Wrong PIN (3,315), Insufficient amount (3,304), Wrong Info (700), Bank Denied (635).

---

## Findings

- **Value is concentrated in Loans.** Loan transactions carry far larger tickets than any other service, which is why value and volume rankings differ sharply.
- **Payment success is high (~96%).** Server errors are the single largest failure reason, which is a platform-side issue rather than a user error.
- **Monthly volume is stable.** Monthly transactions stay in a narrow band of roughly 24K–26K. The visible peaks and dips in the line chart are small relative to the baseline (the y-axis is truncated).
- **Weekday vs. weekend share (71.6% / 28.4%) is essentially proportional to the number of days** (5/7 ≈ 71.4%). Daily volume is nearly flat across the week, so there is no real weekday preference in this data.

---

## Known Limitations

Being upfront about these so readers don't over-interpret the dashboard:

1. **Synthetic data.** Service volumes are almost perfectly uniform (e.g. 50,000 rows for each of three services, ~12.5K per sub-type). Real data would not look like this.
2. **Total Value includes failed and pending transactions.** ₹3,474.3M is the sum of all attempted amounts. Only ~₹3,333M is from successful transactions.
3. **"Top 5 Users" is grouped by name, not `User_ID`.** Many different users share a name (e.g. 47 distinct users are named "Michael Smith"), so the chart shows name groups, not individual users. Grouped by `User_ID`, the top user's total is around ₹349K, not ₹1.82M.
4. **Unique Users (108K) counts registered users**, not active ones. Only ~100.8K users appear in the transactions table.
5. **The Payment Status slicer lists "Pending",** but the data has no Pending status. It has Successful, Failed, Wrong PIN, Server error, and Insufficient amount.

---

## Tech Stack

- **Power BI Desktop**: data modelling, DAX measures, visuals
- **Excel**: source data
- **Power Query**: data cleaning and transformation

---

## Repository Structure

```
├── data/
│   └── Phonepe-Final-Dataset.xlsx
├── dashboard/
│   ├── phonepy_dashboard.pbix
│   └── phonepy_dashboard.pdf
├── images/
│   └── dashboard_preview.png
└── README.md
```

> The `.pbix` (~15 MB) and the dataset (~22 MB) are under GitHub's 100 MB limit, but consider [Git LFS](https://git-lfs.com/) if the files grow.

---

## How to Run

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   ```
2. Open `dashboard/phonepy_dashboard.pbix` in **Power BI Desktop**.
3. If prompted, update the data source path: **Home → Transform data → Data source settings**, and point it to `data/Phonepe-Final-Dataset.xlsx`.
4. Click **Refresh**.

---

## Future Improvements

- Group "Top Users" by `User_ID` and show the name as a label only
- Split Total Value into successful vs. failed value
- Replace the unused "Pending" slicer option with the real failure reasons
- Add a failure-reason breakdown by service
- Add user cohorts by `Join_Date` (retention / activation)

---

## Author

**<Your Name>**
[LinkedIn](https://linkedin.com/in/your-profile) · [GitHub](https://github.com/your-username)

Key Metrics
Findings
Known Limitations
Tech Stack
Repository Structure
How to Run
Future Improvements
Project Overview

The goal is to turn raw payment records into a single-page dashboard that answers:

How many transactions are happening, and how much money moves through the platform?
Which services (Money Transfer, Loans, Insurance, Recharge & Bills) drive volume vs. value?
How reliable are payments, and why do they fail?
Which age groups and users contribute most?
How does activity change month to month?
Dataset

File: Phonepe-Final-Dataset.xlsx (2 sheets)

All_Transactions (300,000 rows)

Column	Description
Transaction_ID	Unique transaction identifier
Amount	Transaction amount (INR), range 20 – 99,999
User_ID	Links to All_Users
Service	Money_Transfer, Recharge_Bills, Loans, Insurance
Service Type	Sub-category (e.g. To UPI ID, DTH, Gold Loan, Health)
Payment_Status	Successful, Failed, Wrong PIN, Server error, Insufficient amount
Reason	Outcome reason (adds Wrong Info and Bank Denied)
Date	Transaction date (1 Jan 2024 – 30 Dec 2024)

All_Users (107,658 rows)

Column	Description
User_ID	Unique user identifier
Name	User name
Age	Age (18 – 60)
Join_Date	Date the user joined

Service breakdown

Service	Service Types
Money Transfer (150K txns)	To UPI ID, To Self Account, To Mobile Number, To QR Code
Loans (50K)	Bike Loan, Credit Score, Gold Loan, Mutual Fund
Insurance (50K)	Bike, Car, Health, Term Life
Recharge & Bills (50K)	Mobile Recharge, DTH, Cable TV, FASTag Recharge
Dashboard Features

Page 1: Overview

KPI cards: Total Transactions, Total Value, Unique Users, Successful Rate, with MoM growth
Transactions Over Time (volume vs. value, by month)
Age Segment Contribution (Gen Z, Millennials, Gen X, Boomers)
Service Transaction Value Analysis
Top 5 Users by transaction value
Weekday vs. Weekend usage
Slicers: Month and Payment Status

Page 2 (drill-through / tooltip): Total Transactions by Service Type

Page 3 (drill-through / tooltip): Total Transaction Value by Service Type

Key Metrics
Metric	Value
Total Transactions	300,000
Total Transaction Value	₹3,474.3M
Unique Users	~108K
Successful Rate	96.00%

Value by service: Loans ₹2.5bn · Insurance ₹0.5bn · Money Transfer ₹0.4bn · Recharge & Bills ₹0.1bn

Volume: Money Transfer accounts for 50% of transactions (150K), but only about 11% of value. Loans account for roughly 72% of value from 1/6 of transactions.

Failure breakdown (by transaction count): Server error (4,053), Wrong PIN (3,315), Insufficient amount (3,304), Wrong Info (700), Bank Denied (635).

Findings
Value is concentrated in Loans. Loan transactions carry far larger tickets than any other service, which is why value and volume rankings differ sharply.
Payment success is high (~96%). Server errors are the single largest failure reason, which is a platform-side issue rather than a user error.
Monthly volume is stable. Monthly transactions stay in a narrow band of roughly 24K–26K. The visible peaks and dips in the line chart are small relative to the baseline (the y-axis is truncated).
Weekday vs. weekend share (71.6% / 28.4%) is essentially proportional to the number of days (5/7 ≈ 71.4%). Daily volume is nearly flat across the week, so there is no real weekday preference in this data.
Known Limitations

Being upfront about these so readers don't over-interpret the dashboard:

Synthetic data. Service volumes are almost perfectly uniform (e.g. 50,000 rows for each of three services, ~12.5K per sub-type). Real data would not look like this.
Total Value includes failed and pending transactions. ₹3,474.3M is the sum of all attempted amounts. Only ~₹3,333M is from successful transactions.
"Top 5 Users" is grouped by name, not User_ID. Many different users share a name (e.g. 47 distinct users are named "Michael Smith"), so the chart shows name groups, not individual users. Grouped by User_ID, the top user's total is around ₹349K, not ₹1.82M.
Unique Users (108K) counts registered users, not active ones. Only ~100.8K users appear in the transactions table.
The Payment Status slicer lists "Pending", but the data has no Pending status. It has Successful, Failed, Wrong PIN, Server error, and Insufficient amount.
Tech Stack
Power BI Desktop: data modelling, DAX measures, visuals
Excel: source data
Power Query: data cleaning and transformation
Repository Structure
├── data/
│   └── Phonepe-Final-Dataset.xlsx
├── dashboard/
│   ├── phonepy_dashboard.pbix
│   └── phonepy_dashboard.pdf
├── images/
│   └── dashboard_preview.png
└── README.md

The .pbix (~15 MB) and the dataset (~22 MB) are under GitHub's 100 MB limit, but consider Git LFS if the files grow.

How to Run
Clone the repo:
bash
git clone https://github.com/Harshrathour-04/<PhonePy Analysis>.git
Open dashboard/phonepy_dashboard.pbix in Power BI Desktop.
If prompted, update the data source path: Home → Transform data → Data source settings, and point it to data/Phonepe-Final-Dataset.xlsx.
Click Refresh.
Future Improvements
Group "Top Users" by User_ID and show the name as a label only
Split Total Value into successful vs. failed value
Replace the unused "Pending" slicer option with the real failure reasons
Add a failure-reason breakdown by service
Add user cohorts by Join_Date (retention / activation)
Author

Harsh Kumar 
