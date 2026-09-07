# Coupon Acceptance Analysis

## Problem Statement

When a driver receives a coupon on their cell phone for a nearby venue, what factors determine whether they accept it? This project investigates the conditions under which drivers are likely to accept driving coupons — specifically bar coupons and coffee house coupons — using a self-reported survey dataset.

The goal is to distinguish between customers who accepted a coupon versus those who did not, using exploratory data analysis, visualizations, and probability distributions.

---

## Data Acquisition

The dataset was sourced from the **UCI Machine Learning Repository** and collected via a survey hosted on **Amazon Mechanical Turk**. Respondents were presented with driving scenarios and asked whether they would accept a coupon given the circumstances.

- **Records**: 12,684 survey responses
- **Features**: 26 attributes covering user demographics, contextual factors, and coupon details
- **Target variable**: `Y` — 1 if the coupon was accepted, 0 if it was not

**Feature categories:**

| Category | Examples |
|----------|----------|
| User attributes | Age, gender, marital status, income, education, occupation, children |
| Behavioral | Frequency of visits to bars, coffee houses, restaurants (cheap/mid-range), carry-out |
| Contextual | Destination, weather (Sunny/Rainy/Snowy), temperature (30°F/55°F/80°F), time of day, passenger type |
| Coupon | Coupon type (Bar, Coffee House, Carry Out, Restaurant <$20, Restaurant $20–$50), expiration (2h or 1d) |

---

## Data Processing

**Missing value handling:**
- The `car` column was dropped — it had **99.1% missing values** and was not recoverable.
- Rows with missing values in remaining columns (Bar, CoffeeHouse, CarryAway, RestaurantLessThan20, Restaurant20To50) were dropped. These accounted for approximately 1–1.7% of records per column and the final cleaned dataset retained ~12,079 records.

**Data quality fixes:**
- The `passanger` column was corrected to `passenger`.
- The `age` column contained string values with non-numeric labels: `"50plus"` was converted to `"50"` and `"below21"` was converted to `"21"`, allowing numeric comparisons.

---

## Modeling

This project uses **exploratory data analysis (EDA)** rather than a predictive machine learning model. The analytical approach involved:

1. **Univariate analysis** — Distribution of coupon types and temperature using bar charts and histograms.
2. **Conditional acceptance rate analysis** — Slicing the dataset by key variables (bar visit frequency, age, passenger type, occupation, income, weather, temperature) to calculate group-level acceptance rates.
3. **Subgroup comparisons** — Comparing acceptance rates between targeted groups and all others to identify meaningful behavioral and contextual signals.

Two coupon types were analyzed in depth:
- **Bar coupons** — guided analysis across 7 structured sub-problems
- **Coffee house coupons** — independent investigation focusing on the effect of temperature, holding weather and visit frequency constant

---

## Model Outcomes

**Overall acceptance rate: 56.9%** — just over half of all surveyed drivers accepted a coupon.

**Bar coupons (acceptance rate: 41.2%):**

| Group | Acceptance Rate |
|-------|----------------|
| Bar visits ≤ 3/month | 37.3% |
| Bar visits > 3/month | 76.2% |
| Bar visits > 1/month AND age > 25 | 69.0% |
| All others (infrequent visitors, age ≤ 25) | 38.8% |
| Bar > 1/month, no kid passenger, non-farming occupation | 71.4% |
| Not widowed, bar > 1/month, no kid passenger | 71.4% |
| Bar > 1/month AND age < 30 | 71.4% |
| Cheap restaurant visits ≥ 4/month AND income < $50K | 45.6% |

**Coffee house coupons (frequent visitors, sunny weather):**

| Temperature Condition | Acceptance Rate |
|----------------------|----------------|
| Hot (> 60°F) | 73.2% |
| Mild (40–60°F) | 60.9% |
| Cold (< 40°F) | 80.0% |

---

## Model Evaluation

Since this is an EDA-based analysis rather than a trained predictive model, traditional model evaluation metrics (accuracy, precision, recall, AUC) were not applied. The analysis was evaluated based on:

- **Effect size** — magnitude of acceptance rate differences between subgroups
- **Directionality** — whether observed patterns align with intuitive behavioral explanations
- **Consistency** — whether patterns held across multiple sub-groupings (e.g., age, passenger, occupation all pointed in the same direction for bar coupons)

The findings are observational and descriptive. A future step would be to train a classification model (e.g., logistic regression, decision tree) and evaluate predictive performance to formalize these insights.

---

## Key Findings and Recommendations

**Bar coupons:**
- **Past behavior is the strongest predictor.** Drivers who visit bars more than 3 times a month accept bar coupons at nearly twice the rate of infrequent visitors (76.2% vs. 37.3%).
- **Age amplifies the effect.** Drivers under 30 who visit bars regularly are among the most receptive audiences.
- **Family context matters.** Drivers traveling with kids are significantly less likely to accept bar coupons — likely due to venue unsuitability and family scheduling constraints.
- **Occupation plays a secondary role.** Drivers in farming, fishing, or forestry occupations showed lower acceptance, though the group is small.

**Coffee house coupons:**
- **Temperature drives acceptance in non-obvious ways.** Both hot days (>60°F) and cold days (<40°F) outperform mild days, suggesting drivers seek cold beverages in heat and warm beverages in cold — both served at coffee houses.
- **The mild temperature "valley" (60.9%)** suggests coffee house coupons are least effective when weather is comfortable and there is no strong beverage motivation.

**Recommendations:**
1. **Target bar coupon campaigns** at drivers aged 21–30 who have demonstrated frequent bar attendance and are traveling without children.
2. **Time coffee house coupon delivery** around weather extremes — very hot or very cold days — to maximize acceptance among frequent coffee house visitors.
3. **Suppress bar coupon delivery** when a driver's passenger is a child, as this significantly reduces acceptance likelihood.
4. **Invest in richer behavioral data** — visit frequency was the most predictive variable. Collecting or inferring more behavioral history (e.g., from loyalty programs) would improve targeting further.
