"Churn Quest: Navigating the Waves of Customer Retention in Telecommunications" 
 
Problem Statement 
The primary challenge is to analyse the factors contributing to customer churn at Airtel. Understanding why customers are leaving will enable the company to implement targeted interventions to improve retention rates. 
Data Link 
https://www.kaggle.com/competitions/customer-churn-prediction-2020/data 	 
Data Dictionary 
	state, string. 2-letter code of the US state of customer residence 
	.. account_length, numerical. Number of months the Customer has been with the current telco provider 
	.. area_code, string="area_code_AAA" where AAA = 3-digit area code. 
	international_plan, (yes/no). The Customer has an international plan. 
	voice_mail_plan, (yes/no). The Customer has a voicemail plan. 
	number_vmail_messages, numerical. Number of voicemail messages. 
	total_day_minutes, numerical. Total minutes of day calls. 
	total_day_calls, numerical. Total number of day calls. 
	total_day_charge, numerical. Total charge of day calls. 
	total_eve_minutes, numerical. Total minutes of evening calls. 
	total_eve_calls, numerical. Total number of evening calls. 
	total_eve_charge, numerical. Total charge of evening calls. 
	total_night_minutes, numerical. Total minutes of night calls. 
	total_night_calls, numerical. Total number of night calls. 
	total_night_charge, numerical. Total charge of night calls. 
	total_intl_minutes, numerical. Total minutes of international calls. 
	total_intl_calls, numerical. Total number of international calls. 
	total_intl_charge, numerical. The total charge of international calls 
	number_customer_service_calls, numerical. Number of calls to customer service 
●    churn (yes/no). Customer churn - target variable. 
	Day_Minutes_Group, categorical. Bins continuous daytime minutes into discrete usage ranges to isolate "bill shock" thresholds. 
	Vmail_Message_Tier, categorical. Groups voicemail message counts to segment active vs. inactive voicemail plan subscribers. 
	CS_Severity_Tier, categorical. Groups customer service calls to track risk above the critical 4-call escalation limit. 
	Intl_Call_Tier, categorical. Groups international calling frequency to track roaming usage friction. 
	Tenure_Period, numerical. Discretises customer account lengths into 6-month cohorts for longitudinal trend analysis. 
	z_day, numerical. Standardised Z-score of daytime minutes.
	z_eve, numerical. Standardised Z-score of evening minutes. 
	z_night, numerical. Standardised Z-score of night minutes. 
	 z_intl, numerical. Standardised Z-score of international minutes. 
	 Cluster Segment, categorical. Assigned behavioural usage persona based on standardised multi-dimensional usage profiles.
	International Interaction, categorical. Evaluates the intersection of international plan subscription and high calling volume.
	Support Friction Interaction, categorical. Identifies compounding joint risk when high customer service calls intersect with high daytime usage.
	Intl_Plan_Weight, numerical. Model weight coefficient assigned to a customer if they have an active international plan. 
	● Service_Calls_Weight, numerical. Model weight coefficient assigned to a customer based on customer service call thresholds. 
	Day_Minutes_Weight, numerical. Model weight coefficient assigned to a customer based on high daytime usage thresholds. 
	Vmail_Discount, numerical. Negative model weight acting as a retention anchor for active voicemail plan subscribers. 
	Log_Odds_z, numerical. The raw linear composite risk score computed from the intercept and feature weights.
	Churn_Prob, numerical. Calibrated risk probability (0% to 100%) calculated via the Logistic Sigmoid formula.
	Pred_Churn, categorical (yes/no). Final model binary prediction flag based on the calibrated decision threshold.
	Total_Monthly_Charges, numerical. Consolidated customer billings calculated by summing daytime, evening, night, and international charge streams.


Basic-Level Questions 
	. How many customers have churned, and what is their proportion compared to the total customer base?
	Total Customers: 4,250
	Retained Customers (no): 3,652 (85.93%) 
	Churned Customers (yes): 598 (14.07%) 
14.07% of customers churned out of 4,250 customers. 
Baseline churn rate: 598 (~14%). 

	.What is the average account length for churned customers compared to non-churned customers? Does the account tenure influence churn? 
       Ans: Average Account length is 99.92 for non-churned customers and 102.04 for churned customers
With a difference of 2.21 months, this shows account tenure does not affect churn.
Loyalty programs will not be good options, as old customers leave at the same rate as new customers
leave.


	How does the subscription to international plans correlate with customer churn? 
      Ans: Customers with International plans were 42.17%, while Customers with no plan were 11.18% (out of a Baseline Rate of 14.07%).
      Inter. Churn Rate is nearly 4 times the normal plan, showing serious dissatisfaction with international calling, poor tariff and connection quality, and uncompetitive out-of-bundle pricing.

	Is there a significant difference in the number of customer service calls between churned and non-churned customers?  
Ans: Churned customers called Customer Service an average of 2.28 times, while non-churned customers called 1.44 times.
Churned customers make significantly more support inquiries. Repeated calls indicate a breakdown in First Contact Resolution (FCR).
Churned Customers Who Called >= 4 times showed a 50.75% Churn Rate.  
	Do day, evening, and night call charges significantly differ between churned and non-churned customers? 
Ans: Day Charge For Churned Customer :- 35.53 and Non-Churned Customer :- 29.54 (Diff-5.68)
           Evening Charges for churned customers: 17.85  and non-churned customers: 16.88 
           Night Charges for churned customers: 9.29 and non-churned customers: 8.98
 High daytime is a financial driver of customer attrition.


	What is the relationship between total day minutes and churn? 
Ans: Retained(no): 175.6 day minutes & Churned (yes): 209 day minutes. 
Difference: 33.43 minutes. (15.9% higher)
Heavy daytime users are a bit shocked by per-minute tariffs and migrate to competitors with flat rates. plans	
        
	How does having a voicemail plan affect churn rates? 
Without Voice plan: 16.4% churned customers
With Voice plan: 7.4% churned customers
Voicemail adoption cuts churn risk by half, serving as a strong retention anchor.

	. Analyse the total international minutes and charges for churned vs. non-churned customers. Are higher usage and charges associated with higher churn? 
Churned Average 10.63 minutes while Retained Average 10.19 minutes. (0.44 mins / 4.13%)
International Charges: Churned Average $ 2.87 while Retained Average $ 2.75.
Ans: Out-of-Bundle Charges create friction for active cross-border callers.

	Is there a correlation between total daily calls and churn? 
Retained: 99.81
Churned: 100.4
Pearson Correlation: 0.11
Ans: Call Frequency does not drive churn; instead, total billable minutes and charge amounts do.

	Compare the average total evening charges for churned vs. non-churned customers. Is there a significant difference? 
Retained: $ 16.9 per minute 
Churned: $ 17.8 per minute
Difference: $ 0.96 per minute

Ans: Elevated evening charges create secondary cumulative cost pressure across high-usage customer segments.



Medium-Level Questions 
	Perform a segmented analysis of churn by area code. Is there a particular area with a significantly higher churn rate? 
	Area Code 415 (42.85% of base): 13.61% churn (287 churned / 2,108 total) 
	Area Code 408 (21.99% of base): 14.00% churn (152 churned / 1,086 total) 
	Area Code 510 (21.11% of base): 15.06% churn (159 churned / 1,056 total) 
Ans: Churn is geographically uniform; Area 415 has more churned customers due to its larger market share.
	Explore the relationship between the number of customer service calls and churn, considering different thresholds (e.g., more than 3 calls).  
	 0" calls" : 12.20% churn 
	1" call" : 14.99% churn 
	2" calls" : 13.89% churn 
	3" calls" : 13.89% churn 
	≥4" calls" : 58.93% churn rate (4.24× surge) 
Ans: A sharp tipping point occurs at >= calls. Airtel must implement an automated alert-triggering or flagging system on the 3rd interaction.

	Analyse the impact of international call charges on churn among subscribers of international plans. 
	Ans: Low international charges (<$2.5: Churn Rate: ~31.2%
High international Charges >$2.5 Churn Rate: ~68.72% 
           Active Inter. Plan users face high linear incremental overage charges, indicating the need for inclusive bundled allowances.







	Examine how evening usage patterns (calls and charges) relate to customer churn. 
Ans: High Evening ( <=220 min/ $21.97 min charges): ~~  17.05 % churn 
Normal Evening (>=220 min/ $ 14.90 ): ~ 12.51 % churn 

Heavy evening duration compounds total bill costs, driving away high usage communicators.

	Perform a cluster analysis to identify distinct customer groups based on service usage patterns (day, evening, night, international).  (k=4)
Heavy Day Users	             35.99%
Heavy Intl Users		              21.47%
Moderate / Low Users		8.28%
Off-Peak / Night Users	               9.11%

Ans: Target customers with bundle pack subscriptions for heavy-day users. And another roaming pack or standard Charge for international Users.

	How does the presence of a voice mail plan interact with the number of voice mail messages in relation to churn?
 
>40 HIGH USAGE	13.95%
1-20 LOW USAGE	4.86%
20-40 MEDIUM USAGE	7.15%
INACTIVE USER	0.00%
NO PLAN	16.44%

	Investigate the relationship between total day minutes and total day charges with churn. Is there a point of inflexion where higher usage leads to higher churn? 
Correlation: 𝑟 = 1.0 (perfect linearity)
Below 220 mins (≈ $37.40): Churn is stable at ≈ 10- 12%.
Inflexion Point: At 220 minutes, churn escalates rapidly, exceeding 50% for usage > 260 mins.
Capped daytime plans at 220 minutes will protect high-spending customers from bill shock.

	Does the number of total international calls affect churn differently for customers with and without international plans? 
        Without a plan, churn rises with call volume due to high per-minute overages.
        With Plan: Churn is elevated (≈ 60%) when calls are low (≤ 2 calls) due to unused recurring
         subscription fees.

	Explore the effect of different customer service call reasons on churn. Are certain types of calls more indicative of churn risk? 
Ans: <3 Normal Enquiry	10.8%
 0 No Support (Satisfied)	10.9%
3-5 Churn Risk	            24.1%
5-9 Critical Churn Risk	64.4% 
First-Contact Resolution (FCR) is the single most critical operational lever to stop churn.

Advanced-Level Questions 
	Develop a predictive model to identify the likelihood of churn based on customer usage patterns and demographic information. Exclude machine learning models for simplicity.

This data is heavily skewed (86% non-churned and 16% churned). Model precision won’t go above 50%.
	

     The model can predict 83% of True Positives.
True Positives (TP)	497
True Negatives (TN)	2981
False Positives (FP)	671
False Negatives (FN)	101
Model Accuracy	0.82
Model Precision	0.43
Model Recall (Sensitivity)	0.83
 
	Conduct a time series analysis to identify trends and seasonality in churn rates. 
                 Ans: The dataset is cross-sectional without chronological date timestamps. 
Pseudo-tenure analysis via account_length shows constant baseline churn across cohorts. 
     Churn is continuous rather than seasonal; ongoing tracking requires longitudinal logging. 

	Use cluster analysis to segment customers into distinct groups based on their service usage and demographic profiles. Identify which segments are more prone to churn. 
	Segment 1 (Standard / Low Usage, 72.05% of base): Churn rate ≈8.52%
	Segment 2 (Heavy Daytime Communicators, 13.27% of base): Churn rate ≈35.99%
	Segment 3 (Support Escalation & International, 14.68% of base): Churn rate ≈21.47.0%

Ans:-- Heavy Day Users and Heavy Intl users are prone to churn hazard due to variable usage Fees accumulation.   Need fixed charges for Heavy users.

	Analyse the interaction effects between various customer attributes (e.g., international plan and total international minutes) on churn.

Intl Plan & High Intl Mins	71.72%
Intl Plan & Low Intl Mins	32.32%

 

No Intl Plan & High Intl Mins	11.62%
No Intl Plan & Low Intl Mins	11.04%
            
Users with an International Plan and high international minutes usage are highly prone to churn (71.72%).
Even Users with low usage showed 32.32% churn; likely (1/3) likely to churn.
Ans: Int. plan heavy users are valuable customers and more prone to churn because variable pricing can lead to unexpectedly high bills.

	Evaluate the economic impact of churn by calculating the potential revenue loss associated with churned customers. Consider factors such as average revenue per user (ARPU) and customer lifetime value (CLV).

KPI / Financial Metric Name	Values
Total Customers 	4250
Total Churned Customers 	598
Baseline Churn Rate CR	14.07%
Monthly ARPU (Retained Users)	USD 58.46
Monthly ARPU (Churned Users)	USD 65.53
Gross Profit Margin (Industry Standard)	0.75
Baseline Customer Lifetime Value (CLV)	USD 311.60
Monthly Recurring Revenue Lost (MRR Churn)	USD 39,188.24
Annual Recurring Revenue Lost (ARR Run-Rate)	USD 4,70,258.88
Total Lifetime Value Destroyed	USD 1,86,334.36
Average Customer Lifespan(Months: 1/CR)	7

What  happened 
