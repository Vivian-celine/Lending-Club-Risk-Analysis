# 🏦 Lending Club Loan Default Prediction & Risk Analysis

**Author:** Vivian Celine  
**GitHub:** [Vivian-celine](https://github.com/Vivian-celine)  
**Dataset:** Lending Club Loan Dataset 2007–2011 (Kaggle)  
**Tools:** Python | Power BI | Jupyter Notebook | Scikit-learn | XGBoost

---

## 📁 Project Structure

```
lending-club-risk-analysis/
├── README.md
├── data/
│   └── loan.csv                        # Raw dataset from Kaggle
├── cleaned_data/
│   └── loan_featured.csv               # Cleaned and engineered dataset
├── notebook/
│   └── lending_club_analysis.ipynb     # Full Jupyter Notebook
├── dashboard/
│   └── loan_risk_dashboard.pbix        # Power BI Dashboard file
└── images/
    ├── page1_loan_portfolio.png         # Dashboard Page 1 screenshot
    └── page2_borrower_insight.png       # Dashboard Page 2 screenshot
```

---

## 🎯 Objective

To analyze **38,577 loan records worth $426M** from Lending Club (2007–2011), identify the key drivers of loan default, and build a machine learning model that predicts whether a borrower will default — enabling fintech companies to make smarter, data-driven lending decisions.

---

## 📊 Dataset Overview

| Property | Details |
|---|---|
| Source | Kaggle — Lending Club Loan Dataset |
| Period | 2007 – 2011 |
| Original Size | 39,719 rows × 111 columns |
| Final Working Size | 38,577 rows × 23 columns |
| Target Variable | loan_status (Fully Paid = 1, Charged Off = 0) |
| Overall Default Rate | 14.59% |
| Total Loan Portfolio | $426 Million |

**Key Columns Used:**

| Column | Description |
|---|---|
| loan_amnt | Amount requested by borrower |
| int_rate (%) | Interest rate charged |
| grade | Lending Club risk grade (A–G) |
| emp_length (years) | Employment duration |
| annual_inc | Borrower's yearly income |
| dti | Debt-to-income ratio |
| home_ownership | Own / Rent / Mortgage |
| verification_status | Was income verified? |
| purpose | Reason for taking the loan |
| revol_util (%) | Credit card utilization rate |
| delinq_2yrs | Late payments in last 2 years |
| loan_status | ⭐ Target — Did they pay back? |

---

## 🧹 Data Cleaning Process

The raw dataset had 111 columns and required significant cleaning before analysis:

**Steps Taken:**

1. **Column Selection** — Reduced from 111 columns to 28 relevant columns based on business relevance and analytical value

2. **Handled Missing Values**
   - emp_length (1,075 nulls) → Filled with mode: "10+ years"
   - revol_util (50 nulls) → Filled with median value

3. **Cleaned Text Columns**
   - Removed % signs from int_rate and revol_util and converted to float
   - Removed "months" from term column — converted to numeric (36, 60)
   - Mapped emp_length text values to numbers (< 1 year → 0, 10+ years → 10)

4. **Filtered Target Variable**
   - Removed "Current" loans (outcome unknown)
   - Kept only: Fully Paid and Charged Off
   - Mapped to binary: Fully Paid = 1, Charged Off = 0

5. **Removed Data Leakage Columns**
   - Dropped: total_pymnt, total_pymnt_inv, total_rec_prncp, total_rec_int, last_pymnt_amnt
   - These columns only exist after loan completion and would give the model an unfair advantage

6. **Encoded Categorical Columns**
   - Used LabelEncoder on: grade, sub_grade, home_ownership, verification_status, purpose

7. **Feature Engineering** — Created 3 new meaningful columns:
   - funded_ratio = funded_amnt ÷ loan_amnt
   - credit_history_age = issue_d − earliest_cr_line (in years)
   - payment_ratio = installment ÷ funded_amnt

---

## 🔍 Exploratory Data Analysis

Key questions explored and answered through charts and statistics:

- Who defaults more — renters or mortgage holders?
- Which loan grades carry the highest default risk?
- Does higher interest rate lead to more defaults?
- Which loan purposes are riskiest?
- Does income verification actually reduce default?
- How does loan amount size affect default rate?

---

## 🤖 Machine Learning Models

Three models were built, compared, and evaluated:

### Before Fixing Class Imbalance

| Model | Accuracy |
|---|---|
| Logistic Regression | 85.81% |
| Random Forest | 85.81% |
| XGBoost | 84.78% |

⚠️ Despite high accuracy, models were failing to detect defaulters (Class 0 recall < 5%). This is a class imbalance problem — 6,621 paid back vs only 1,095 defaulted.

### After Fixing Class Imbalance (Oversampling)

Used **random oversampling** to balance the dataset — giving the model equal exposure to both defaulters and payers.

| Model | Accuracy | Class 0 Recall |
|---|---|---|
| Logistic Regression | 64.37% | 64% |
| Random Forest | 97.87% ⭐ | 99% |
| XGBoost | 81.97% | 86% |

### 🏆 Final Model: Random Forest — 97.87% Accuracy

Random Forest was selected as the final model because:
- Highest overall accuracy (97.87%)
- 99% recall on defaulters — catches almost every bad borrower
- Robust to outliers and works well with mixed data types

---

## 🔑 Top 5 Features That Predict Default

Based on Random Forest feature importance:

| Rank | Feature | Why It Matters |
|---|---|---|
| 1 | payment_ratio | Monthly payment burden relative to loan size |
| 2 | annual_inc | Higher income = lower default risk |
| 3 | revol_util (%) | High credit card usage signals financial stress |
| 4 | int_rate (%) | Higher rates increase repayment difficulty |
| 5 | dti | Higher debt burden = higher default risk |

---

## 📈 Dashboard Preview

### Page 1: Loan Portfolio & Risk Analysis
![Loan Portfolio & Risk Analysis](images/page1_loan_portfolio.png)

### Page 2: Borrower Profile & Insight
![Borrower Profile & Insight](images/page2_borrower_insight.png)

---

## 💡 Key Insights

1. **14.59% Overall Default Rate** — Out of $426M in loans, approximately $62M is at risk of default

2. **Grade G and F are the riskiest** — Default rates of 34% and 33% respectively, compared to just 6% for Grade A borrowers

3. **Small Business loans default the most** — 27% default rate, the highest of any loan purpose

4. **Renters default more than mortgage holders** — 15.4% vs 13.7%, suggesting property ownership indicates financial stability

5. **Higher interest rates predict default** — Defaulters had an average interest rate of 13.82% vs 11.61% for payers

6. **Higher DTI predicts default** — Defaulters had average DTI of 14.0 vs 13.1 for payers

7. **Larger loans default more** — Loans above $20K had a 20.3% default rate, the highest across all loan size groups

8. **Verified borrowers default more** — 17% vs 13% for unverified. Verification enables access to larger loans which increases repayment pressure

9. **Borrowers with 10+ years employment default more** — Long employment history leads to larger loan approvals which increases default risk when combined with high DTI

---

## ✅ Recommendations

1. **Restrict Grade F and G lending** — Apply stricter approval criteria or reduce loan limits for these high-risk segments

2. **Set DTI hard limit** — Flag any borrower with DTI above 14 as high risk and require additional review

3. **Tighten small business loan criteria** — Require financial statements, business revenue proof and collateral for small business applicants

4. **Don't rely on verification alone** — Combine income verification with loan size caps to prevent verified borrowers from taking on unmanageable debt

5. **Cap loans above $15K for risky profiles** — Borrowers with high DTI and low grade should not qualify for large loan amounts regardless of employment history

6. **Target Grade A and B borrowers for growth** — With default rates of 6% and 12%, these segments represent the healthiest lending opportunity

7. **Use mortgage ownership as positive credit signal** — Mortgage holders default less and should receive preferential rates

---

## 💰 Business Impact

| Finding | Business Impact |
|---|---|
| 14.59% default rate on $426M portfolio | $62M in loan losses identified |
| Restricting Grade F and G lending | Could reduce defaults by up to 30% |
| Applying DTI limit above 14 | Filters highest risk borrowers before approval |
| Capping small business loans | Reduces highest defaulting segment (27%) |
| Targeting Grade A and B borrowers | Healthier portfolio and better investor confidence |
| Random Forest model at 97.87% accuracy | Automates credit decisions saving time and money |
| 99% recall on defaulters | Catches almost every bad loan before it is approved |

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Python | Data cleaning, feature engineering, model building |
| Pandas | Data manipulation and analysis |
| NumPy | Numerical computations |
| Matplotlib & Seaborn | Data visualization and EDA charts |
| Scikit-learn | Machine learning models and preprocessing |
| XGBoost | Gradient boosting model |
| Power BI | Interactive business dashboard |
| Jupyter Notebook | Development and documentation environment |
| Anaconda | Python environment management |

---

## 📬 Contact

**Vivian Celine**  
GitHub: [Vivian-celine](https://github.com/Vivian-celine)

---

*This project was built to demonstrate real-world credit risk analysis and machine learning skills applicable to fintech lending institutions.*
