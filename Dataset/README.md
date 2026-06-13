# Dataset Folder

This folder contains the raw, processed, and Tableau-ready datasets used in the session-level ecommerce funnel analytics project.

## Folder Structure

```text
Dataset/
├── Raw Dataset/
│   └── events.csv
└── Processed Dataset/
    ├── funnel_data.csv
    └── tableau_session_funnel_data.csv
```

## Dataset Creation Flow

Run the notebooks in this order to recreate the datasets:

```text
Raw Dataset/events.csv
    ↓ Notebooks/Funnel Analytics.ipynb
Processed Dataset/funnel_data.csv
    ↓ Notebooks/Tableau Data Preparation.ipynb
Processed Dataset/tableau_session_funnel_data.csv
    ↓ ML models/Baseline Logistic Regression.ipynb and Tableau dashboard
```

The Tableau Data Preparation notebook checks whether `funnel_data.csv` exists before running.

The Logistic Regression notebook checks whether `tableau_session_funnel_data.csv` exists before running.

## Raw Dataset

### `Raw Dataset/events.csv`

This is the original event-level clickstream dataset used to build the session-level funnel table.

Each row represents one user event.

Expected columns:

| Column | Description |
| --- | --- |
| `timestamp` | Event timestamp in Unix millisecond format |
| `visitorid` | Unique visitor identifier |
| `event` | Event type such as `view`, `addtocart`, or `transaction` |
| `itemid` | Product/item identifier |
| `transactionid` | Transaction identifier, available for transaction events |

This file is used in `Notebooks/Funnel Analytics.ipynb` for:

- event-level data understanding
- timestamp conversion
- session key generation
- duplicate event validation
- session-level funnel table creation

## Processed Dataset

### `Processed Dataset/funnel_data.csv`

This is the validated session-level funnel dataset created from `events.csv`.

Each row represents one visitor session.

Expected columns:

| Column | Description |
| --- | --- |
| `session_key` | Unique session identifier |
| `visitorid` | Visitor identifier |
| `viewed` | Whether the session had at least one view event |
| `carted` | Whether the session had at least one add-to-cart event |
| `purchased` | Whether the session had at least one transaction event |
| `session_start` | Session start timestamp |
| `session_end` | Session end timestamp |
| `session_duration_mins` | Session duration in minutes |
| `session_type` | Session behavior category |
| `event_count` | Number of events in the session |
| `event_count_segment` | Event-count based segment |
| `duration_segment` | Duration-based segment |

This file is used in:

- descriptive funnel analytics
- diagnostic analytics
- inferential statistics
- Tableau data preparation

### `Processed Dataset/tableau_session_funnel_data.csv`

This is the enhanced session-level dataset created from `funnel_data.csv`.

It is designed for Tableau dashboarding and is also used by the baseline Logistic Regression notebook.

Additional columns include:

| Column | Description |
| --- | --- |
| `log_session_duration_mins` | Log-transformed session duration |
| `visitor_session_count` | Number of sessions associated with the visitor |
| `is_repeat_visitor` | Whether the visitor has more than one session |
| `session_start_hour` | Hour of day when the session started |
| `session_start_dayofweek` | Day of week number when the session started |
| `session_start_day_name` | Day of week name when the session started |
| `is_weekend` | Whether the session started on Saturday or Sunday |

This file is used in:

- Tableau dashboard creation
- baseline Logistic Regression modeling
- time-based purchase behavior analysis
- repeat visitor analysis

## Important Scope Note

This project supports session-level funnel analysis.

The processed funnel table shows whether a session had view, add-to-cart, and purchase activity. It does not prove that the same product moved from view to cart to purchase.

For exact same-product conversion analysis, a future product-level funnel table would need to be created using `visitorid`, `session_key`, and `itemid`.

## File Size Note

The included dataset files are large:

| File | Approx. Size |
| --- | ---: |
| `Raw Dataset/events.csv` | 89.87 MB |
| `Processed Dataset/funnel_data.csv` | 178.99 MB |
| `Processed Dataset/tableau_session_funnel_data.csv` | 222.52 MB |

If GitHub upload fails due to file-size limits, use Git LFS or host the data separately and keep only this README with download instructions.
