# Airtel ChurnQuest: Telecom Customer Churn Analysis & Excel Financial Model

An executive-grade analytical framework and predictive model built entirely in **Excel** to identify customer attrition patterns, pinpoint critical service/billing tipping points, and translate statistical risk into direct bottom-line revenue impact.

---

## 🎯 Executive Summary
At Airtel, retaining high-value subscribers is a core strategic priority. This project establishes that our customer base has a **baseline churn rate of 14.07%**. By examining demographics, plan subscriptions, call frequency, and customer service records, this workbook moves past basic descriptive statistics to build a functional, non-ML predictive model and a financial valuation scorecard.

We uncover exact thresholds where customer frustration boils over into defection, enabling marketing and support teams to deploy highly-targeted, proactive retention campaigns before churn occurs.

---

## ⚡ Key Analytical Revelations (Operational Tipping Points)

Our data analysis reveals that customer attrition is non-linear and heavily driven by specific billing and service thresholds:

*   **The 4-Call Support Tipping Point 📞**: Customer service friction acts as a step-function. Churn remains low and stable at **10% to 13%** for customers making 0 to 3 support calls, but jumps exponentially to **exceed 50%** at **4 or more calls**. 
*   **The 220-Minute Daytime "Bill Shock" Point ⚡**: Churn risk remains stable until daytime usage hits **220 minutes** (where churn jumps to **>30%**), and balloons to **over 55%** past 260 minutes . This points to acute billing friction in our daytime tariff structures.
*   **The International Plan Risk 🌍**: Subscribing to an international plan is currently our highest standalone risk factor, with plan subscribers exhibiting a **~40%+ churn rate** compared to just **11% to 14%** for non-subscribers.
*   **Voicemail Plan as an Loyalty Anchor 📼**: Subscribing to a voicemail plan cut customer attrition roughly in half, dropping the segment churn rate to **~7% to 8%**.

---

## 🛠️ Excel Model Architecture & Engineered Columns

The master workbook contains the original 20-column baseline dataset supplemented by **20 engineered helper variables** to support advanced behavioral profiling and automated scoring:

### 1. Feature Standardization & Clustering (Advanced)
To compare disjoint variables (such as 400 daytime minutes directly against 4 customer service calls), we implement **Z-score standardization** :
$$z = \frac{\text{Value} - \text{Mean}}{\text{StdDev}}$$ 
*   **`z_day` / `z_eve` / `z_night` / `z_intl`**: Center the mean of usage patterns to 0 and scale the variance to 1 [cite: 100].
*   **`Cluster_Segment`**: Automatically groups customers into key behavioral personas (e.g., `Heavy Day Users` or `Heavy Intl Users`) based on whether their standardized usage scores cross defined risk thresholds.

### 2. Pure Excel Probabilistic Churn Model
We build a dynamic, non-black-box classification model by translating business risk weights into calibrated log-odds and mapping them through the mathematical **Logistic Sigmoid Function**:
$$P(\text{Churn}) = \frac{1}{1 + e^{-z}}$$
*   **`Log_Odds_z`**: Dynamically sums the baseline intercept (calibrated to **-1.82** to match the population's 86:14 active-to-churn skew) and positive/negative behavioral coefficients (e.g., adding `+2.5` for $\ge 4$ customer service calls, while subtracting a `-0.8` loyalty anchor for voicemail subscribers).
*   **`Churn_Prob`**: Binds the linear log-odds score into a clean continuous probability between **0% and 100%** using `=1 / (1 + EXP(-Log_Odds_z))`.
*   **`Pred_Churn`**: Flags customers as `"yes"` or `"no"` churn risks based on a tuned decision boundary of **$\ge 0.55$**, maximizing precision and shielding proactive marketing budgets from false alarms.

---

## 📊 Unit Economics & Bottom-Line Financial Scorecard

To justify retention budgets to executive leadership, the model translates predictive scores into financial parameters:

1. **Total Monthly Revenue (`Total_Monthly_Charges`)**: Consolidates daytime, evening, night, and international call charges per customer.
2. **High-Value Customer Loss 💸**: Churned accounts carry a higher average monthly bill (**\$65.53 ARPU**) than retained accounts (**\$58.46 ARPU**), indicating our premium customers are the most vulnerable to bill shock.
3. **Total Financial Loss (Airtel Cohort)**:
   *   **Monthly Recurring Revenue (MRR) Lost**: **\$39,188.24 per month** 
   *   **Annual Recurring Revenue (ARR) Lost**: **\$470,258.88 per year** 
4. **Customer Lifetime Value (CLV)**: Standard 75% gross margin over our baseline churn rate yields a baseline customer lifetime value of **\$311.60**.
5. **Campaign ROI Cap (CRC Ceiling)**: With an individual CLV of \$311.60, Airtel can economically justify a maximum **Customer Retention Cost (CRC) ceiling of \$75 to \$100** per targeted, high-risk customer  

---

## 📂 Repository Layout
*   📂 `/` — Root Directory
    *   📘 `README.md` — Project overview and business summary.
    *   📊 `Customer_Retention_Analysis.xlsx` — The master Excel spreadsheet containing raw data, engineered helper columns, pivot table dashboards, and financial models.


