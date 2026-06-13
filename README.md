# Session-Level Ecommerce Funnel Analytics

This project analyzes ecommerce clickstream behavior at the session level using Python. Raw event-level data is converted into a session-level funnel table to understand broad session behavior across view, add-to-cart, and purchase activity.

The project includes data preparation, funnel validation, exploratory analysis, diagnostic analysis, inferential statistics, and a baseline Logistic Regression model to classify whether a completed session contains a purchase.

## Business Problem

Ecommerce businesses often receive many product views, but only a small share of sessions move to add-to-cart or purchase activity.

The main business question is:

> Which session-level behaviors are associated with purchase sessions, and where are users dropping off in the funnel?

This project focuses on:

- identifying major funnel drop-offs
- comparing purchase vs non-purchase sessions
- understanding session engagement patterns
- testing whether engagement differences are statistically significant
- building an interpretable baseline model for purchase-session classification

## Project Scope

This is a session-level funnel analysis.

Each row in the final funnel dataset represents one visitor session. The funnel columns indicate whether that session included:

- view activity
- add-to-cart activity
- purchase activity

Important limitation:

This analysis does not prove that the same product was viewed, added to cart, and purchased. A session can include a view of Product A, cart activity for Product B, and purchase of Product C. Therefore, the project should be interpreted as session-level conversion behavior, not exact product-level journey analysis.

## Repository Structure

```text
.
├── Dataset/
│   ├── Raw Dataset/
│   │   └── events.csv
│   ├── Processed Dataset/
│   │   ├── funnel_data.csv
│   │   └── tableau_session_funnel_data.csv
│   └── README.md
├── Notebooks/
│   ├── Funnel Analytics.ipynb
│   └── Tableau Data Preparation.ipynb
├── ML models/
│   └── Baseline Logistic Regression.ipynb
├── .gitignore
└── README.md
```

## Notebooks

## Recommended Run Order

Run the notebooks in this order:

```text
1. Notebooks/Funnel Analytics.ipynb
   Input:  Dataset/Raw Dataset/events.csv
   Output: Dataset/Processed Dataset/funnel_data.csv

2. Notebooks/Tableau Data Preparation.ipynb
   Input:  Dataset/Processed Dataset/funnel_data.csv
   Output: Dataset/Processed Dataset/tableau_session_funnel_data.csv

3. ML models/Baseline Logistic Regression.ipynb
   Input:  Dataset/Processed Dataset/tableau_session_funnel_data.csv
   Output: baseline and enhanced Logistic Regression model evaluation
```

The Tableau Data Preparation and Logistic Regression notebooks include file checks. If the required input file is missing, the notebook raises a clear message explaining which previous notebook should be run first.

### 1. Funnel Analytics

Location:

```text
Notebooks/Funnel Analytics.ipynb
```

This notebook covers:

- raw event data inspection
- timestamp conversion
- session key generation using a 30-minute inactivity rule
- duplicate-event validation
- session-level funnel table creation
- funnel-stage validation
- descriptive analytics
- diagnostic analytics
- inferential statistics
- business insights and recommendations

Output:

```text
Dataset/Processed Dataset/funnel_data.csv
```

### 2. Tableau Data Preparation

Location:

```text
Notebooks/Tableau Data Preparation.ipynb
```

This notebook takes the validated session-level funnel dataset and creates a dashboard-ready dataset with additional reporting features.

Input:

```text
Dataset/Processed Dataset/funnel_data.csv
```

Output:

```text
Dataset/Processed Dataset/tableau_session_funnel_data.csv
```

The Tableau-ready dataset includes features such as:

- `log_session_duration_mins`
- `visitor_session_count`
- `is_repeat_visitor`
- `session_start_hour`
- `session_start_dayofweek`
- `session_start_day_name`
- `is_weekend`

### 3. Baseline Logistic Regression

Location:

```text
ML models/Baseline Logistic Regression.ipynb
```

This notebook covers:

- loading the Tableau-ready session-level dataset
- target variable definition
- baseline Logistic Regression model
- handling class imbalance with `class_weight='balanced'`
- ROC-AUC and Precision-Recall evaluation
- feature coefficient interpretation
- feature ablation
- threshold tuning
- business interpretation of model results

Input:

```text
Dataset/Processed Dataset/tableau_session_funnel_data.csv
```

## Methodology

1. Load and inspect raw ecommerce event data.
2. Convert Unix millisecond timestamps into datetime values.
3. Sort events by visitor and timestamp.
4. Generate session keys using a 30-minute inactivity rule.
5. Remove exact duplicate event rows before funnel creation.
6. Aggregate raw events into a session-level funnel table.
7. Validate funnel-stage combinations and session logic.
8. Perform descriptive analytics to answer what happened.
9. Perform diagnostic analytics to understand why purchase and non-purchase sessions differ.
10. Use inferential statistics to support key behavioral findings.
11. Create a Tableau-ready enhanced session dataset.
12. Build a baseline Logistic Regression model to classify purchase sessions.
13. Evaluate the model using imbalance-aware metrics and threshold tuning.

## Key Findings

- Most sessions are view-only sessions.
- Purchase sessions represent a very small share of all sessions.
- The largest funnel weakness is the transition from view activity to cart activity.
- Purchase sessions are longer and more active than non-purchase sessions.
- Multi-event sessions are significantly more likely to purchase than single-event sessions.
- Cart activity, session duration, and repeat-visitor behavior are strong purchase-session signals.
- Threshold tuning is important because the purchase class is highly imbalanced.

## Core Funnel Metrics

| Metric | Result |
| --- | ---: |
| View-to-Cart Conversion | 2.21% |
| View-to-Cart Drop-off | 97.79% |
| Cart-to-Purchase Conversion | 27.17% |
| Cart-to-Purchase Drop-off | 72.83% |
| Average Session Duration | 1.75 mins |
| Median Session Duration | 0.00 mins |

## Inferential Analytics

Statistical tests were used to support the observed patterns:

- two-proportion z-test comparing purchase rates between single-event and multi-event sessions
- Mann-Whitney U test comparing session duration between purchase and non-purchase sessions

The results support that stronger session engagement is significantly associated with purchase behavior.

## Baseline Model Summary

The Logistic Regression model predicts whether a completed session contains a purchase.

Target:

```text
purchased
```

Important modeling decision:

The raw `event_count` feature was removed because it can include purchase events and may introduce target leakage.

Enhanced model features are created in `Notebooks/Tableau Data Preparation.ipynb` and loaded into the Logistic Regression notebook.

Enhanced features include:

- `viewed`
- `carted`
- `log_session_duration_mins`
- `visitor_session_count`
- `is_repeat_visitor`
- `session_start_hour`
- `session_start_dayofweek`
- `is_weekend`

## Threshold Tuning Result

At the default threshold `0.5`, the model captured many purchase sessions but created many false positives.

At threshold `0.9`:

- Precision: 32.51%
- Recall: 86.29%
- False positives: 5,122
- False negatives: 392

This threshold gave a more practical balance for business use by reducing false positives substantially while keeping recall strong.

## Business Recommendations

1. Improve product detail pages and calls-to-action to increase movement from view to cart.
2. Investigate cart abandonment because many cart sessions do not convert to purchase.
3. Target repeat visitors because they show stronger purchase intent.
4. Use purchase-probability scores for retargeting, but apply a higher threshold to control false positives.
5. Treat the current Logistic Regression model as an interpretable baseline and improve it later with product, category, price, traffic-source, and device features if available.

## Limitations

- The funnel is session-level, not product-level.
- Same-product view-to-cart-to-purchase behavior is not confirmed.
- The model predicts purchase status for completed sessions, not live future behavior during an active session.
- Product category, price, traffic source, device, and campaign information are not included in the current model.
- Logistic Regression is used as an interpretable baseline model.

## Future Improvements

- Build a product-level funnel using `visitorid`, `session_key`, and `itemid`.
- Create a real-time purchase-intent model using only features available before purchase.
- Add product metadata, category hierarchy, price, traffic source, and device features.
- Compare Logistic Regression with Random Forest, XGBoost, or LightGBM.
- Add Tableau dashboard visualizations for session-level funnel reporting.
- Use cost-based threshold selection based on campaign cost and expected revenue.

## Resume Summary

Built an end-to-end session-level ecommerce funnel analytics project using Python. Transformed raw event-level clickstream data into a validated session-level funnel dataset, analyzed conversion drop-offs, performed diagnostic and inferential analytics, and developed an interpretable Logistic Regression model to classify purchase sessions under severe class imbalance.
