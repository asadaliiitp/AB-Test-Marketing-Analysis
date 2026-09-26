# A/B Test Analysis — Ad Campaign vs PSA Control

## 1. Project Overview

This project analyzes a marketing A/B test to check whether showing users an actual ad campaign leads to a higher conversion rate than showing a Public Service Announcement (PSA).

The analysis uses a binary conversion metric and covers the main steps I would use for a basic experiment analysis:

- data validation
- sample ratio mismatch (SRM) check
- two-proportion z-test
- conversion-rate lift
- 95% confidence interval
- statistical vs. practical significance
- day/hour segment checks
- exposure check
- final business decision

## 2. Business Question

**Does the ad campaign increase conversion compared with the PSA control?**

## 3. Dataset

The notebook uses `marketing_AB.csv`.

The dataset contains 588,101 users, with:

- `test_group` — `ad` or `psa`
- `converted` — whether the user converted
- `total_ads` — recorded ad exposure
- `most_ads_day` — day with the most recorded ads
- `most_ads_hour` — hour with the most recorded ads

The raw dataset is not included in this repository package. Place it at:

```text
data/marketing_AB.csv
```

before running the notebook.

## 4. Main Results

| Metric | Result |
|---|---:|
| Total users | 588,101 |
| Ad users | 564,577 |
| PSA users | 23,524 |
| Ad conversion rate | 2.55% |
| PSA conversion rate | 1.79% |
| Absolute lift | +0.77 percentage points |
| 95% CI | +0.60 to +0.94 percentage points |
| Relative lift | +43.1% |
| z-statistic | 7.37 |
| p-value | 1.7 × 10⁻¹³ |
| SRM check | No mismatch detected |

## 5. Statistical Approach

### SRM check

The experiment was designed with an approximately 96/4 allocation between the ad and PSA groups. A chi-square goodness-of-fit test was used to check whether the observed group sizes were consistent with that allocation.

### Hypothesis test

The primary metric is conversion rate.

- **H₀:** conversion rates are equal between the ad and PSA groups.
- **H₁:** conversion rates are different between the ad and PSA groups.
- **α:** 0.05

A two-proportion z-test was used because conversion is a binary outcome and the analysis compares two groups.

### Confidence interval

A 95% confidence interval was calculated for the difference in conversion rates. The interval stays above zero, which is consistent with the hypothesis-test result.

## 6. Business Interpretation

The ad group had a higher observed conversion rate than the PSA group.

The estimated difference was about **+0.77 percentage points**, or about **+43.1% relative to the PSA conversion rate**.

The result is statistically significant. From a business perspective, the size of the observed lift is worth considering, but the dataset does not contain campaign cost, revenue per conversion, or downstream customer value. Because of that, ROI cannot be calculated from this analysis alone.

## 7. Segment Analysis

The notebook checks the observed result by:

- day of week
- hour-of-day bucket

The ad group has a higher observed conversion rate across the tested segments.

These checks are descriptive. Users were not separately randomized within each day/hour segment, so the segment results should not be treated as independent causal experiments.

## 8. Exposure Check

The median recorded ad exposure is similar between the groups (13 vs. 12).

This is useful descriptive context, but `total_ads` is a post-treatment variable. It is therefore not treated as an independent causal adjustment for the main treatment effect.

## 9. Decision

The results support using the ad experience over the PSA control for the primary conversion metric tested.

Before making the change permanent, I would also check the economics of the additional conversions and validate the result with a follow-up experiment.

## 10. Project Files

```text
ab-test-marketing-analysis/
│
├── marketing_AB.csv          
│
├── AB_Test_Analysis.ipynb
│
├── Decision_Memo.docx
│
├── README.md
├── requirements.txt
└── .gitignore
```

## 11. How to Run

1. Clone the repository.
2. Put `marketing_AB.csv` inside the `data/` folder.
3. Install the required Python packages.
4. Open `notebooks/AB_Test_Analysis.ipynb`.
5. Run the cells from top to bottom.

The notebook currently reads the dataset as `marketing_AB.csv`, so if the file is stored in `data/`, update the loading path to:

```python
pd.read_csv('../data/marketing_AB.csv', index_col=0)
```

## 12. Tools Used

- Python
- pandas
- NumPy
- SciPy
- statsmodels
- Matplotlib
- Jupyter Notebook

## 13. Portfolio Skills Demonstrated

- A/B testing
- Hypothesis testing
- Two-proportion z-test
- Confidence intervals
- Sample ratio mismatch checking
- Conversion-rate analysis
- Segmentation
- Statistical vs. practical significance
- Business decision-making
