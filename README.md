# A/B Test Data Quality Review

A review of an A/B test setup for a mobile app: data quality checks, funnel analysis, and an A/A/B test to decide whether a new font design can be shipped safely.

TripleTen Data Analyst program project — Mara Torres Blanco.

## What this project does

The company wanted to change the font in its app and worried the new design might feel "intimidating" and hurt user behavior. Before trusting the experiment results, this project:

1. **Checks the data quality** of the event log and defines a clean analysis window.
2. **Builds the purchase funnel** (Main screen → Offers → Cart → Successful payment).
3. **Validates the experiment** with an A/A test on the two control groups, then compares the test group against the controls, with a Bonferroni correction for multiple comparisons.

## Tools

- Python
- Pandas, NumPy
- Matplotlib
- SciPy (statistical tests)

## Key findings

### Data quality

- The log contains **244,126 events** from **7,551 unique users** (July 25 – August 7, 2019).
- Records before August 1 are incomplete (fewer than 2,500 events in 7 days vs. ~35,000 events per day after that), so the analysis uses **August 1–7, 2019** as the window. The cutoff loses only **1.2% of events** and **0.2% of users**.

### Funnel

| Step | Users | % of total | % of previous step |
|---|---|---|---|
| Main screen | 7,419 | 98.5% | — |
| Offers screen | 4,593 | 61.0% | 61.9% |
| Cart screen | 3,734 | 49.6% | 81.3% |
| Successful payment | 3,539 | 47.0% | 94.8% |

- The biggest drop-off is between the Main screen and the Offers screen: **~38% of users leave at this step** — the most critical stage to improve.
- Once a user reaches the cart, the chance of completing payment is very high (**94.8%**).

### A/A/B test

- Group sizes are well balanced: Control A (246): 2,484 users; Control B (247): 2,513 users; Test (248, new font): 2,537 users.
- **A/A validation (246 vs. 247):** no statistically significant differences in any funnel event (all p-values > 0.05), confirming the user split and the experiment setup work correctly.
- **Controls vs. test group (248):** no event showed a statistically significant difference (all p-values > 0.05).
- **12 hypothesis tests** were run in total. With the Bonferroni correction (α = 0.05 / 12 = 0.0042) the conclusions do not change.
- **Conclusion:** the font change does not negatively affect user behavior at any stage of the funnel. Management's concern about the new design is not supported by the data. **Recommendation: the design team can ship the font change.**

## Project structure

```
ab-test-data-quality-review/
├── ab_test_data_quality_review.ipynb   # Full analysis (EDA, funnel, A/A/B test)
├── README.md
└── .gitignore
```

## How to run it

Open `ab_test_data_quality_review.ipynb` in Jupyter Notebook or JupyterLab and run the cells top to bottom.

## Data

The analysis uses `logs_exp_us.csv` (tab-separated event log: `event_name`, `user_id`, `timestamp`, `exp_id`), included in this repository. The notebook originally read it from TripleTen's `/datasets/` path — point that path to the included file to run it.

## Notes

- The notebook keeps the original TripleTen reviewer comments (in Spanish) untouched.
- The notebook has no title/introduction cell — the project overview above covers that gap.
