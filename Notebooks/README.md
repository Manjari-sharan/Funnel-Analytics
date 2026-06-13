# Notebooks

Run these notebooks in order.

## 1. Funnel Analytics

Notebook:

```text
Funnel Analytics.ipynb
```

Purpose:

- Load raw ecommerce event data.
- Generate session keys using the 30-minute inactivity rule.
- Create the session-level funnel table.
- Perform validation, descriptive analytics, diagnostic analytics, inferential statistics, insights, and recommendations.

Input:

```text
../Dataset/Raw Dataset/events.csv
```

Output:

```text
../Dataset/Processed Dataset/funnel_data.csv
```

## 2. Tableau Data Preparation

Notebook:

```text
Tableau Data Preparation.ipynb
```

Purpose:

- Load the validated session-level funnel dataset.
- Add dashboard-ready features such as log duration, repeat visitor flag, visitor session count, hour, day of week, and weekend flag.
- Save the Tableau-ready session-level dataset.

Input:

```text
../Dataset/Processed Dataset/funnel_data.csv
```

Output:

```text
../Dataset/Processed Dataset/tableau_session_funnel_data.csv
```

Note:

If `funnel_data.csv` is missing, run `Funnel Analytics.ipynb` first.
