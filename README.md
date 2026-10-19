# R-Marketing-Metrics-Attribution
#  Core Marketing Analytics, CLV & Attribution Modeling in R

# Project Overview
This repository contains a comprehensive suite of marketing analytics projects developed in **R**. It covers foundational performance tracking, behavioral dynamic segmentation, Customer Lifetime Value ($CLV$) formulation, retention/churn mechanics, and multi-touch attribution modeling.

The suite bridges financial metrics with strategic decision-making to optimize customer acquisition costs ($CAC$) and budget allocation across digital channels.

---

# Projects Included & Methods Applied

# 1. Customer Satisfaction & Budget Efficiency (CPA & ROAS)
- **Concepts:** Campaign cost efficiency and Return on Ad Spend ($ROAS$).
- **Functions:** Vector arithmetic, dataframe summaries, ratio computations ($CPA = \text{Spend} \div \text{Conversions}$).

# 2. Behavioral Customer Segmentation & Retail Analytics
- **Concepts:** Median-based dynamic thresholding and transaction co-occurrence (Market Basket Analysis).
- **Functions:** `median()`, `ifelse()`, `factor()`, `cor()` correlation matrices.

# 3. Multi-Touch Attribution Modeling
- **Concepts:** Comparative revenue credit distribution across customer touchpoints (**First-Touch** vs. **Last-Touch**).
- **Functions:** `aggregate()`, `merge()`, `is.na()` for handling multi-channel journey pathways.

# 4. Customer Lifetime Value (CLV) & Churn Rate Modeling
- **Concepts:** Annual customer value, monthly churn rate ($1 / \text{Churn Rate}$), segmented $CLV$, and $CAC$ ceiling calculations.
- **Formulas:**
  - $\text{Annual Value} = \text{Avg Order Value} \times \text{Purchase Frequency}$
  - $\text{CLV} = \text{Annual Value} \times \text{Expected Lifespan (Years)}$

---

#💻 R Code Examples

# Multi-Touch Attribution Comparison Code
```R
# Customer Journey Data
journeys_df <- data.frame(
  Journey_ID = 1:4,
  First_Touch = c("Facebook", "Instagram", "Facebook", "TikTok"),
  Middle_Touch = c("Instagram", "Google Ads", "TikTok", "Instagram"),
  Last_Touch = c("Google Ads", "Google Ads", "Google Ads", "Google Ads"),
  Revenue = c(120, 200, 150, 180)
)

# First-Touch Model
first_touch_rev <- aggregate(Revenue ~ First_Touch, data = journeys_df, sum)
colnames(first_touch_rev) <- c("Channel", "First_Touch_Revenue")

# Last-Touch Model
last_touch_rev <- aggregate(Revenue ~ Last_Touch, data = journeys_df, sum)
colnames(last_touch_rev) <- c("Channel", "Last_Touch_Revenue")

# Merge Models
attribution_comparison <- merge(first_touch_rev, last_touch_rev, by = "Channel", all = TRUE)
attribution_comparison[is.na(attribution_comparison)] <- 0
print(attribution_comparison)
segments_data <- data.frame(
  Segment = c("VIP_Customers", "Standard_Customers"),
  Avg_Order_Value = c(300, 100),
  Purchase_Frequency = c(6, 3),
  Churn_Rate = c(0.10, 0.25)
)

segments_data$Annual_Value <- segments_data$Avg_Order_Value * segments_data$Purchase_Frequency
segments_data$Expected_Lifespan_Years <- 1 / segments_data$Churn_Rate
segments_data$CLV <- segments_data$Annual_Value * segments_data$Expected_Lifespan_Years
print(segments_data)
