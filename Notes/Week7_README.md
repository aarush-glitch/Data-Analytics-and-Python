# 📊 Week 7 — Data Analytics with Python (NPTEL)
### Lectures 31–35 | Topics: Regression (CI/PI, Residuals, Multiple, Dummy Variables)

---

## 📌 LECTURE 31 — Confidence Interval vs Prediction Interval

### Point Estimate vs Interval Estimate for ŷ

When x = xp is given, **point estimate** = `ŷp = b₀ + b₁xp` (same for both CI and PI)

But we need an **interval** because the point estimate is not perfectly reliable.

---

### Confidence Interval (CI) — Estimating the MEAN of Y

When you want an estimate of the **mean Y for ALL observations** at x = xp:

**Variance of ŷp**:
`S²_ŷp = S² × [1/n + (xp − x̄)² / Σ(xi − x̄)²]`

**CI formula**:
`ŷp ± t_(α/2, n−2) × S_ŷp`

> 🔑 CI is **narrower** — estimates mean of all Y values at xp

---

### Prediction Interval (PI) — Estimating ONE Individual Y

When you want an estimate for **one specific observation** at x = xp:

**Variance of individual prediction** (adds one extra σ² term):
`S²_ind = S² + S²_ŷp = S² × [1 + 1/n + (xp − x̄)² / Σ(xi − x̄)²]`

**PI formula**:
`ŷp ± t_(α/2, n−2) × S_ind`

> 🔑 PI is **wider** — accounts for extra individual-level variation

| | CI | PI |
|--|----|----|
| **Goal** | Estimate mean of Y for ALL units at xp | Estimate Y for ONE specific unit at xp |
| **Width** | Narrower | Wider |
| **Extra variance** | No | Yes (+S²) |
| **Shape** | Curved (not straight line) | Curved (also not straight) |

**Special property**: Both CI and PI are **narrowest at x = x̄**
- At x = x̄: the (xp − x̄)² term = 0 → minimum variance → narrowest interval

---

### Worked Example (Ice Cream Sales)

ŷ = 60 + 5x; n=10; x̄=14; MSE=191; **xp = 10** → ŷp = 60 + 5(10) = **110**

- CI: 110 ± 11.415
- PI: 110 ± **33.875** (much wider margin)

```python
from statsmodels.stats.outliers_influence import summary_table

data1, ss2 = summary_table(result1, alpha=0.05)
fitted_values   = data1[:, 2]         # column 3
predict_mean_ci_low  = data1[:, 4]    # column 5 → CI lower
predict_mean_ci_upp  = data1[:, 5]    # column 6 → CI upper
predict_ci_low  = data1[:, 6]         # column 7 → PI lower
predict_ci_upp  = data1[:, 7]         # column 8 → PI upper
```

> 🔑 **Narrow intervals = high precision model; wide intervals = poor model**

---

## 📌 LECTURE 32 — Residual Analysis (Validating Model Assumptions)

### What is Residual?

`eᵢ = yᵢ − ŷᵢ` (actual − predicted)

**4 Assumptions to Validate** (all checked via residual plots):

| # | Assumption | Check Via |
|---|-----------|-----------|
| 1 | E(ε) = 0 (zero mean) | Residual plot (no pattern) |
| 2 | Constant variance (homoscedasticity) | Residual vs X → horizontal band |
| 3 | Independence (no autocorrelation) | Residual vs X → random scatter |
| 4 | Normality: ε ~ N(0, σ²) | Normal probability (QQ) plot |

> ⚠️ If assumptions violated → **t-tests, F-tests, CI, PI all become INVALID**

---

### Residual Plot Types

| Plot | X-axis | Y-axis | Good Pattern |
|------|--------|--------|--------------|
| **Residual vs X** | X (independent var) | Residual | Random scatter (horizontal band) |
| **Residual vs ŷ** | ŷ (predicted Y) | Residual | Random scatter (preferred for multiple regression) |
| **Standardized Residual** | X or ŷ | (eᵢ)/S | 95% points lie between **−2 and +2** |
| **Normal Probability (QQ)** | Normal scores | Standardized residuals | Points cluster on **45° diagonal line** |

> 🔑 For **simple regression**: residual vs X and residual vs ŷ give same pattern
> 🔑 For **multiple regression**: always use **residual vs ŷ** (only one plot needed)

---

### Violation Patterns (IMPORTANT!)

| Pattern in Residual Plot | Problem | Fix |
|--------------------------|---------|-----|
| **Horizontal band** ✓ | None (good!) | — |
| **Funnel / cone shape** | Non-constant variance (**Heteroscedasticity**) | Transform Y (log, sqrt) |
| **Curved / U-shape** | Non-linearity | Try polynomial or multiple regression |
| **Pattern in QQ plot** | Non-normality | Check for outliers / transform |
| **Points outside ±2** | Potential outliers | Investigate observations |

---

### Standardized Residual Formula

`Standardized residual = eᵢ / S_eᵢ`

where `S_eᵢ = S × √(1 − hᵢ)` and `hᵢ = 1/n + (xᵢ − x̄)² / Σ(xᵢ − x̄)²`

```python
import seaborn as sns
from statsmodels.formula.api import OLS

reg1 = OLS('sales ~ student_population', data=df1).fit()

# 1. Residual vs X
sns.residplot(x='student_population', y='sales', data=df1, color='green')

# 2. Standardized residuals
influence = reg1.get_influence()
resid_student = influence.resid_studentized_external
# Plot: student_population vs resid_student (all should be between ±2)

# 3. Normal Probability (QQ) plot
import statsmodels.api as sm
fig, ax = plt.subplots()
sm.qqplot(reg1.resid, line='45', ax=ax)
# Points clustering around the 45° line = normality satisfied
```

---

### ANOVA Table for Regression (df interpretation)

| Source | df | SS | MS | F |
|--------|----|----|----|---|
| Regression (X) | **p** (# of X vars) | SSR | SSR/p | MSR/MSE |
| Error (Residual) | **n−p−1** | SSE | SSE/(n−p−1) = MSE | — |
| Total | n−1 | SST | — | — |

For simple regression (p=1): df_error = n−1−1 = **n−2**

---

## 📌 LECTURE 33 — Multiple Linear Regression I (Theory)

### Multiple Regression Model

**Population model**:
`Y = β₀ + β₁X₁ + β₂X₂ + ... + βₚXₚ + ε`

**Sample (estimated) equation**:
`ŷ = b₀ + b₁x₁ + b₂x₂ + ... + bₚxₚ`

| Term | Meaning |
|------|---------|
| p | Number of independent variables |
| b₀...bₚ | Sample estimates of β₀...βₚ |
| nT = n | Total observations |
| R² | Multiple coefficient of determination |

---

### Coefficient Interpretation (Multiple Regression)

**b₁**: "Keeping all other variables constant, a 1-unit increase in X₁ increases ŷ by b₁ units"

> 🔑 "Ceteris paribus" (other things equal) interpretation — critical concept!

**Example (Trucking)**:
ŷ = −0.8687 + 0.0611X₁ + 0.9234X₂
- X₁ = miles traveled; X₂ = number of deliveries; Y = travel time
- b₁ = 0.0611: each extra mile → 0.0611 more hours (holding deliveries constant)
- b₂ = 0.9234: each extra delivery → 0.9234 more hours (holding miles constant)

```python
from statsmodels.formula.api import ols
from statsmodels.stats.anova import anova_lm

# Simple regression (1 variable)
reg1 = ols('travel_time ~ x1', data=df1).fit()   # R²=0.664

# Multiple regression (2 variables)
reg2 = ols('travel_time ~ x1 + n_of_deliveries', data=df1).fit()  # R²=0.90

print(reg2.summary())
anova_table = anova_lm(reg2, typ=1)
print(anova_table)
```

---

### SST Partition in Multiple Regression

`SST = SSR + SSE`

Adding more variables:
- SSR **increases** (explains more)
- SSE **decreases** (less residual error)
- R² **increases** (always!)

---

### Adjusted R² — THE KEY METRIC

**Problem with R²**: Adding ANY variable (even irrelevant) increases R² → overfitting risk!

**Formula**:
`R²_adj = 1 − (1 − R²) × (n−1)/(n−p−1)`

where n = observations, p = number of independent variables

| R² vs Adjusted R² | Meaning |
|-------------------|---------|
| R² ↑ and R²_adj ↑ | New variable is genuinely useful |
| R² ↑ but R²_adj ↓ | New variable is **noise** — remove it! |
| R² ≈ R²_adj | Model is well-specified (no more variables needed) |
| R² large, R²_adj << R² | Too many irrelevant variables (overfitting) |
| Adjusted R² is **negative** | Very poor model with too many useless predictors |

> 🔑 **Adjusted R² is NOT interpreted as % variability explained.** It is only a comparison metric!

**Multiple Regression Response Surface**:
- Simple regression → **line** in 2D
- Multiple regression (2 vars) → **plane** in 3D (also called **Response Surface Model / RSM**)

---

### Multiple Regression Assumptions (same 4 as simple)

1. E(ε) = 0 for all values of X₁, X₂, ..., Xₚ
2. Var(ε) = σ² (constant) for all X — homoscedasticity
3. ε values are **independent**
4. ε ~ N(0, σ²) — normally distributed

---

## 📌 LECTURE 34 — Multiple Regression II (Significance Tests & Dummy Variables)

### F-Test: Overall Model Significance

`H₀: β₁ = β₂ = ... = βₚ = 0` (no predictor is useful)
`H₁: At least one βᵢ ≠ 0`

`F = MSR/MSE` where:
- `MSR = SSR/p`
- `MSE = SSE/(n−p−1)`
- df: numerator = p, denominator = n−p−1

Reject H₀ if **p-value < α** → model as a whole is significant

> 🔑 F-test FIRST; if significant → proceed to t-tests for individual variables

---

### t-Test: Individual Variable Significance

For each βᵢ:
`H₀: βᵢ = 0` (variable i has no effect)
`H₁: βᵢ ≠ 0`

`t = bᵢ / Sbᵢ` with df = n−p−1

Reject H₀ if **p-value < α** → variable i is significant

> 🔑 If p-value > α for a variable → **DROP that variable** from the model (not significant at population level)

**Python output key fields**:
```
R²             → goodness of fit
Adj. R²        → adjusted for number of predictors
F-statistic    → overall model significance
Prob (F-stat)  → p-value for F-test (< 0.05 = model significant)
coef           → b₀, b₁, b₂ (regression coefficients)
P>|t|          → p-values for each individual coefficient
[0.025, 0.975] → 95% CI for each coefficient
```

---

### ANOVA and Regression Are Two Sides of the Same Coin!

ANOVA can be **solved via regression with dummy variables**!

**Example (Assembly Methods A, B, C)**:
- 3 categories → need **3−1 = 2 dummy variables**
- D₁=1,D₂=0 → Method A; D₁=0,D₂=1 → Method B; D₁=0,D₂=0 → Method C (reference)

| Method | D₁ | D₂ | Expected Y |
|--------|----|----|-----------|
| A | 1 | 0 | β₀ + β₁ |
| B | 0 | 1 | β₀ + β₂ |
| C | 0 | 0 | β₀ (baseline) |

- b₀ = 52 (mean for C), b₁ = 10 → mean for A = 52+10 = **62**, b₂ = 14 → mean for B = 52+14 = **66**
- p-value from regression = 0.0038 (same as ANOVA p-value!) ✓

```python
# ANOVA via regression with dummy variables
dummies = pd.get_dummies(data_r['treatment'])  # creates one-hot columns
dummies = dummies.drop('C', axis=1)             # drop reference (k-1 rule!)

step_1 = pd.concat([data_r[['value']], dummies], axis=1)
result = smf.ols('value ~ A + B', data=step_1).fit()
# Same F and p-value as one-way ANOVA!
```

---

## 📌 LECTURE 35 — Categorical Variable (Dummy Variable) Regression

### Why Dummy Variables?

Standard regression requires **continuous** variables. Categorical variables (like gender, repair type) must be coded numerically as **dummy (indicator) variables**.

**Rule**: k categories → **k−1 dummy variables**

| k categories | # Dummy vars |
|-------------|-------------|
| 2 (male/female) | 1 |
| 3 (job grade 1/2/3) | 2 |
| 4 (A/B/C/D) | 3 |

The omitted category becomes the **reference/baseline group**.

---

### Creating Dummy Variables in Python

```python
# Method 1: pd.get_dummies()
just_dummies = pd.get_dummies(tv1['type_of_repair'])
# Creates columns: [Electrical, Mechanical]

# Keep only k-1 columns (drop reference)
tv1 = tv1.drop('type_of_repair', axis=1)
tv1 = pd.concat([tv1, just_dummies[['Electrical']]], axis=1)
# Now: Electrical=1 → electrical repair; 0 → mechanical repair
```

---

### Interpretation of Dummy Variable Coefficient

**Setup**: ŷ = β₀ + β₁X₁ + β₂X₂ (X₂ = 1 if electrical, 0 if mechanical)

| X₂ | Equation | Meaning |
|----|----------|---------|
| **0** (Mechanical) | ŷ = β₀ + β₁X₁ | Baseline |
| **1** (Electrical) | ŷ = (β₀+β₂) + β₁X₁ | Same slope, different intercept |

**Both lines have the SAME slope β₁, but DIFFERENT Y-intercepts!**

| Sign of β₂ | Interpretation |
|-----------|----------------|
| **β₂ > 0** | Category coded 1 has HIGHER mean Y than reference |
| **β₂ < 0** | Category coded 1 has LOWER mean Y than reference |
| **β₂ = 0** | No difference between categories |

> 🔑 **β₂** = difference in mean Y between the two categories (holding other X constant)

---

### Worked Example 1 (Repair Time)

Data: X₁ = months since last service; X₂ = repair type (1=electrical, 0=mechanical)
- Simple regression (X₁ only): ŷ = 2.15 + 0.3041X₁; R²=0.534
- Multiple (X₁ + X₂): ŷ = 0.93 + 0.388X₁ + 1.26X₂; R²=**0.859**
- β₂ = 1.26 > 0 → electrical repairs take **1.26 hours longer** than mechanical
- For electrical: ŷ = (0.93+1.26) + 0.388X₁ = 2.19 + 0.388X₁
- For mechanical: ŷ = 0.93 + 0.388X₁

---

### Worked Example 2 (Gender Discrimination)

Data: Y = salary; X₁ = experience; X₂ = gender (1=female, 0=male)

**Only gender**: ŷ = 9.7 − 1.175X₂; p-value of gender = **0.389 (NOT significant)**
**Gender + experience**: ŷ = 6.25 + 0.227X₁ − 0.789X₂; p-value of gender = **< 0.05 (significant!)**

Key insight: When experience is controlled for, gender discrimination IS detectable.
β₂ = −0.789 → females earn 0.789 units less than males with the same experience.

> 🔑 **Coding reversal doesn't change conclusions!**
> Switching male=0,female=1 to male=1,female=0 only changes the **sign and intercept**, not the actual predicted salaries.

```python
# Full dummy regression workflow
tbl2 = pd.read_excel('dummy2.xlsx')

# Create dummies
just_dummies = pd.get_dummies(tbl2['gender'])   # [female, male]
tbl2 = tbl2.drop('gender', axis=1)
tbl2 = pd.concat([tbl2, just_dummies[['female']]], axis=1)  # keep female col

# Regression: salary ~ experience + female
result = smf.ols('salary ~ experience + female', data=tbl2).fit()
print(result.summary())
# Coefficient of 'female' = difference between female and male average salary
```

---

### k > 2 Categories (Job Grade Example)

Job grade: 3 levels → **2 dummy variables** (D₁, D₂)

| Grade | D₁ | D₂ | Expected Y |
|-------|----|----|-----------|
| Grade 1 | 1 | 0 | β₀ + β₁ |
| Grade 2 | 0 | 1 | β₀ + β₂ |
| Grade 3 (reference) | 0 | 0 | **β₀** |

- β₁ = difference between Grade 1 and Grade 3
- β₂ = difference between Grade 2 and Grade 3
- Both measured relative to the **reference category (all zeros)**

---

## 🎯 Week 7 Assignment — Key Questions & Answers

| Q | Topic | Answer | Working |
|---|-------|--------|---------|
| Q1 | What happens when regression assumptions are violated? | **Hypothesis testing results become unreliable** | p-values, SEs, CIs all become wrong |
| Q2 | Purpose of residual analysis | **Validate regression model assumptions** (not estimate coefficients) | Checks linearity/independence/homoscedasticity/normality |
| Q3 | Funnel/widening shape in residual plot | **Heteroscedasticity** | Non-constant variance = cone/funnel shape |
| Q4 | Curved pattern in residual plot | **Linear model may be inappropriate** | Suggests polynomial or non-linear model needed |
| Q5 | What do standardized residuals help detect? | **Outliers** (values beyond ±2) | 95% should lie within ±2 |
| Q6 | Effect of heteroscedasticity | **Invalid hypothesis tests and CIs** | OLS estimates stay unbiased; standard errors inflate |
| Q7 | What does multicollinearity affect? | **Stability and SEs of coefficient estimates** | Coefficients unstable; SEs inflate; inference affected |
| Q8 | Why use Adjusted R²? | **Penalizes inclusion of irrelevant variables** | R² always increases; Adj R² only increases if variable helps |
| Q9 | Overall F-test rejection means | **At least one independent variable is significant** | Not ALL, just ≥1 |
| Q10 | Dummy variable coefficient in regression means | **Difference in mean Y between coded-1 group and reference** | β₂ = E(Y\|group1) − E(Y\|reference), ceteris paribus |

---

## 💡 Quick-Fire Review Checklist

**CI vs PI:**
- [ ] Point estimate: ŷp = b₀ + b₁xp (same for both CI and PI)
- [ ] CI: estimates mean of ALL Y at xp → narrower; formula uses S²_ŷp = S²[1/n + (xp−x̄)²/Sxx]
- [ ] PI: estimates ONE individual Y at xp → wider; formula adds extra S² term
- [ ] Both intervals are narrowest at **x = x̄**; both are curved (not straight lines)
- [ ] Prediction interval ALWAYS wider than confidence interval

**Residual Analysis:**
- [ ] 4 assumptions: zero mean, constant variance, independence, normality
- [ ] Violation → ALL inference (t, F, CI, PI) becomes invalid
- [ ] Good residual plot = horizontal band (random scatter)
- [ ] Funnel/cone = **heteroscedasticity** (non-constant variance)
- [ ] Curve/U-shape = **non-linearity** (use polynomial/multiple regression)
- [ ] Standardized residuals: 95% should be within **±2**
- [ ] QQ plot: points should cluster on **45° diagonal line** → normality
- [ ] Multiple regression: use residual vs **ŷ** (not vs each X separately)
- [ ] Python: `sns.residplot()`, `influence.resid_studentized_external`, `sm.qqplot(resid, line='45')`

**Multiple Regression:**
- [ ] ŷ = b₀ + b₁x₁ + b₂x₂ + ... + bₚxₚ
- [ ] Interpret bᵢ: "holding all others constant, 1 unit ↑ in xᵢ → bᵢ change in ŷ"
- [ ] SST = SSR + SSE; adding variables → SSR↑, SSE↓, R²↑
- [ ] R² always increases with more variables → use Adj. R² to evaluate new variables
- [ ] Adj R² ↑ → new variable is useful; Adj R² ↓ → new variable is noise
- [ ] Adj R² can be **negative** (poor model with many irrelevant predictors)
- [ ] Python: `ols('y ~ x1 + x2', data).fit()`; `anova_lm(model, typ=1)`
- [ ] df for regression = p; df for error = n−p−1; df total = n−1

**Significance Testing:**
- [ ] F-test: H₀: β₁=β₂=...=βₚ=0 → tests model overall; F=MSR/MSE; df=(p, n−p−1)
- [ ] t-test: H₀: βᵢ=0 → tests each variable; t=bᵢ/Sbᵢ; df=n−p−1
- [ ] If F-test passes and individual t-test fails → drop that variable
- [ ] Durbin-Watson: checks autocorrelation in residuals (mentioned, used in lec34 output)

**Dummy Variable Regression:**
- [ ] k categories → k−1 dummy variables; one category = reference (all zeros)
- [ ] Create: `pd.get_dummies(df['col'])` then drop one column
- [ ] β₂ coefficient = difference in mean Y between group (coded=1) and reference (coded=0)
- [ ] β₂ > 0 → group coded 1 has higher Y; β₂ < 0 → lower Y
- [ ] Both regression lines (one per category) have same slope; only Y-intercept differs
- [ ] Reversing coding (0↔1) only changes sign of coefficient; predicted values stay same
- [ ] ANOVA can be solved via regression with dummy variables → same F and p-values!
