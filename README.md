# Marketing A/B Test

A portfolio exercise using the public [Marketing A/B Testing dataset on Kaggle](https://www.kaggle.com/datasets/faviovaz/marketing-ab-testing), shared by faviovaz. This is a learning dataset; the author did not conduct the experiment. The original CSV is kept in the ignored `data/` directory and is not tracked by Git.

## Question and hypotheses

Does the conversion rate differ between users in the advertising (`ad`) and public service announcement (`psa`) groups? Conversion rate is the number of converted users divided by the number of users in the group.

- **Null hypothesis:** the population conversion rates are equal.
- **Alternative hypothesis:** the population conversion rates differ.
- The analysis uses a two-sided test at a 5% significance level for this exercise. The CSV does not establish that this was a prespecified analysis plan.

The absolute difference is ad minus PSA conversion in percentage points; relative lift divides that difference by the PSA conversion rate. We use a pooled two-proportion z-test and an unpooled normal 95% confidence interval for the difference.

## Data and design checks

The notebook checks required columns, missing analysis values, valid group and outcome labels, and unique user IDs. All 588,101 rows have unique IDs. There are 564,577 users in `ad` (96.00%) and 23,524 in `psa` (4.00%), a roughly 24:1 ratio. Unequal allocation can be intentional; without the planned ratio, we cannot interpret a sample-ratio mismatch test or conclude that allocation failed.

The CSV does not establish the assignment procedure, independence, eligibility, experiment dates, exposure rules or conversion window. The number of conversions and non-conversions in both groups is large enough for the normal approximation, conditional on independent observations. Exposure variables may reflect post-assignment behaviour and are not used for causal subgroup claims.

## Results and interpretation

| Group | Users | Conversions | Conversion rate |
| --- | ---: | ---: | ---: |
| PSA | 23,524 | 420 | 1.785% |
| Ad | 564,577 | 14,423 | 2.555% |

The observed difference is **0.769 percentage points**, or **43.09% relative lift** over PSA. These are two ways to describe the same comparison: relative lift is not a 43-point increase in conversion rate.

The 95% confidence interval for the absolute difference is **0.595 to 0.943 percentage points**. Under the method's assumptions, this is a range compatible with the data; over repeated comparable samples, the interval procedure would cover the true difference 95% of the time. The two-sided test gives z = 7.370 and p approximately 1.71 × 10⁻¹³, below the 5% threshold. This is evidence against equal rates under the test assumptions, not a measure of business value or proof of causation. One overall comparison is reported; no multiple-testing correction is applied.

## Conclusion

The higher observed conversion in the ad group merits further investigation. The data alone do not justify a rollout or a profit claim. Before deciding, verify randomization, planned allocation, eligibility, timing and conversion definitions; then compare advertising costs with the value of conversions. The confidence interval does not capture uncertainty about the experiment's design or data collection.

## Reproduce

Install Python 3.10 or later. Place the downloaded `marketing_AB.csv` in `data/`, then from the repository root run:

```text
python -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
.venv\Scripts\python.exe -m jupyter lab notebooks/marketing_ab_test.ipynb
```

Select the virtual environment as the notebook kernel and run all cells. Alternatively, open the notebook in VS Code and select that kernel. The notebook reads the CSV without modifying it. Local CSV files, `data/`, and `results/` are ignored by Git.

## Status

- [x] Validate analysis fields and calculate the overall comparison.
- [x] Run the Python notebook with Anaconda Python 3.13.
- [ ] Verify assignment and experiment design against the original study documentation.
