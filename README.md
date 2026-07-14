# WHOOP-recovery-analysis
Statistical Modelling of Physiological Recovery Using WHOOP Data

Multiple linear regression analysis identifying the physiological and behavioral drivers of daily recovery score using 306 days of longitudinal WHOOP wearable data.

## Overview

This project investigates which cardiovascular and sleep-related variables are associated with daily recovery score, a composite metric WHOOP uses to reflect autonomic nervous system balance and readiness. Using a multiple linear regression framework, the analysis tests whether heart rate variability (HRV) and resting heart rate (RHR) are significant predictors of recovery, and whether sleep-related variables add explanatory power beyond cardiovascular measures.

## Key Findings

- **HRV** was positively and significantly associated with recovery (β = 1.18, p < 0.001)
- **Resting heart rate** was negatively and significantly associated with recovery (β = −1.87, p < 0.001)
- **Sleep efficiency** was significant but with a smaller effect (β = −0.41, p = 0.027)
- The final model explained **46.7% of the variance** in recovery score (R² = 0.467)
- Cardiovascular markers — not sleep or behavioral metrics — emerged as the dominant drivers of recovery

| Predictor | Estimate | Std. Error | t value | p-value |
|---|---|---|---|---|
| Intercept | 185.76 | 28.44 | 6.53 | < 0.001 |
| HRV (ms) | 1.18 | 0.19 | 6.31 | < 0.001 |
| Resting Heart Rate (bpm) | -1.87 | 0.25 | -7.46 | < 0.001 |
| Sleep Efficiency (%) | -0.41 | 0.18 | -2.22 | 0.027 |

## Data

- **Source:** 308 consecutive days of personal WHOOP wearable data, exported directly from the device platform
- **Final sample:** 306 complete daily observations after preprocessing
- **Variables:** HRV (ms), resting heart rate (bpm), sleep efficiency (%), sleep debt (minutes), strain, lagged prior-day strain, and daily recovery score (%)

### Preprocessing steps
- Cleaning and standardizing column names
- Converting date-time variables to POSIX format
- Sorting observations chronologically
- Constructing a lagged strain variable to capture prior-day physiological load
- Removing observations with missing values in key predictors

## Methods

1. **Exploratory analysis** — descriptive statistics and Pearson correlation to assess bivariate relationships and preliminary collinearity
2. **Full OLS model** — recovery regressed on HRV, RHR, sleep efficiency, sleep debt, and lagged strain
3. **Model reduction** — non-significant predictors dropped; nested models compared via an ANOVA F-test
4. **Diagnostics:**
   - Residuals vs. fitted (linearity, homoscedasticity)
   - Q–Q plot (residual normality)
   - Scale-location plot (variance stability)
   - Leverage / Cook's distance (influential observations)
   - Variance Inflation Factors (multicollinearity)
   - Shapiro–Wilk test (formal normality check)

Full model specification:

```
Recovery_i = β0 + β1(HRV_i) + β2(RHR_i) + β3(SleepEff_i) + β4(SleepDebt_i) + β5(Strain_{i-1}) + ε_i
```

All analysis was conducted in **R**.

## Results Summary

Pearson correlations showed recovery was moderately-to-strongly correlated with HRV (r = 0.60) and resting heart rate (r = −0.62), while sleep efficiency showed a weak, non-significant correlation (r = −0.10). HRV and RHR were themselves correlated (r = −0.63), but VIF values (1.00–1.67) confirmed multicollinearity was not a concern.

Diagnostic checks indicated no severe violations of regression assumptions: residuals were reasonably homoscedastic, no high-leverage/influential points exceeded Cook's distance thresholds, and while the Shapiro–Wilk test detected a statistically significant deviation from normality (W = 0.953, p < 0.001), this was judged minor given the sample size and OLS's robustness to small normality departures.

## Limitations

- Data represents a single individual (n = 1 subject, repeated measures) — findings are not generalizable to a broader population
- Observational design — no causal claims can be made
- Unmeasured confounders (psychological stress, nutrition, illness, training intensity) were not included in the model


## Tools

- **R** (base stats, regression diagnostics, ggplot2/base plotting)


