# Pay, Productivity, and Aging in MLB: Replication and Extension

Replication of Hakes & Turner (2011), *"Pay, productivity and aging in Major League Baseball"* (Journal of Productivity Analysis), built from scratch in R using the Lahman Baseball Database, then extended with regression diagnostics and more recent seasons.

**[Read the full report (PDF)](report/mlb-aging-salary-report.pdf)** · Code: [`analysis.qmd`](analysis.qmd)

## Questions

1. How does a hitter's offensive productivity change with age and experience?
2. Does that aging curve differ by player ability?
3. How do salaries respond to experience and ability in early (years 1–6) vs. late (years 7+) careers?

## Approach

- **Built the dataset** from four Lahman tables (`Batting`, `Salaries`, `People`, `Fielding`): filtered to position players with at least 130 at-bats in a season, merged birth and debut years, and computed OBP, SLG, OPS and **indexed OPS (iOPS)**, which normalizes OPS to each season's league average.
- **Grouped players into ability quintiles** by peak ability (each player's third-best iOPS season).
- **Rebuilt the paper's models** (Tables 2, 3 and 5): quadratic age and experience models for iOPS, and log-salary models split by career stage. Each comes in three forms: pooled, quintile-specific intercepts, and quintile-specific intercepts and slopes.
- **Extended the paper** in two ways:
  - **Regression diagnostics** the original paper doesn't report: linearity (residuals vs. fitted), homoscedasticity (Breusch–Pagan), mean-zero errors, independence (Durbin–Watson), and normality (Q–Q plots, Shapiro–Wilk).
  - **More recent data:** re-ran every model on seasons through 2023.

## Key findings

| Model (log-salary, years 1–6) | R² |
|---|---|
| Pooled experience curve | 0.43 |
| + ability-quintile intercepts | 0.48 |
| + ability-quintile slopes | 0.51 |

- **Aging curve replicated:** productivity rises early, peaks in the late 20s and then declines (a negative quadratic in age). Age alone explains almost nothing (R² ≈ 0.003). Adding ability quintiles raises R² to about 0.33, so **ability drives most of the variation, and age shapes the curve**.
- **Better hitters age better:** top-quintile hitters peak later and decline more gradually, while low-quintile hitters peak earlier.
- **Salary:** pay climbs quickly with experience in years 1–6 and then flattens. Ability remains the main driver of pay late in a career.
- **Diagnostics:** linearity, mean-zero errors and approximate normality hold. Homoscedasticity and independence don't, so the coefficient estimates are reliable, but standard errors should be read with caution.
- **Modern data:** the same patterns hold through 2023, with more separation in pay between ability tiers.

> Note: the Lahman `Salaries` table ends in 2016, so the extended salary models cover 1985–2016. The performance models extend through 2023.

## Tools

R · dplyr · tidyr · readr · ggplot2 · broom · car · Quarto

## Reproduce

```r
install.packages(c("dplyr","readr","tidyr","ggplot2","lubridate","broom","knitr","car","forcats"))
```
```bash
quarto render analysis.qmd   # renders to PDF; data is read from data/
```

## Data & references

- Lahman, S. *Lahman Baseball Database* — https://www.seanlahman.com/baseball-archive/statistics/ (the CSVs in `data/` are included for reproducibility)
- Hakes, J. K., & Turner, C. (2011). Pay, productivity and aging in Major League Baseball. *Journal of Productivity Analysis, 35*(1), 61–74. https://doi.org/10.1007/s11123-009-0152-8

Final project for STAT 3215Q: Applied Linear Regression in Data Science, University of Connecticut (Fall 2025).
