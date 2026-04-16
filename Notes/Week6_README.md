# 📊 Week 6 — Data Analytics with Python (NPTEL)
### Lectures 26–30 | Topics: RBD, Two-Way ANOVA, Simple Linear Regression (I–III)

---

## 📌 LECTURE 26 — Randomized Block Design (RBD)

### Why RBD? The Problem with CRD

In **CRD (One-Way ANOVA)**, MSE contains:
> `MSE = True Error + Error due to Nuisance Factor`

If a nuisance (external) factor (e.g., individual differences, batch variation) inflates the MSE:
- F-value = MSTR/MSE → becomes **smaller** (treatment effect hidden)
- You may **accept H₀ incorrectly** — conclude no treatment effect when there IS one!

**Solution: RBD** — isolate and remove nuisance variation from MSE

| Design | When to Use |
|--------|------------|
| **CRD** | Experimental units are **homogeneous** |
| **RBD** | Experimental units are **heterogeneous** (blocking removes nuisance effect) |

> 🔑 Blocking in RBD is analogous to **stratification in sampling** — creates homogeneous groups

---

### RBD ANOVA Table

| Source | SS | df | MS | F |
|--------|----|----|-----|---|
| Treatment | SSTR | k−1 | MSTR | MSTR/MSE |
| **Block** | SSBL | b−1 | MSBL | (not tested) |
| Error | SSE | (k−1)(b−1) | MSE | — |
| Total | SST | bk−1 | — | — |

where **k** = number of treatments, **b** = number of blocks

**Key formula**:
`SSE = SST − SSTR − SSBL`

> 🔑 Blocking only reduces MSE — we don't test significance of blocks (not the study objective)

---

### RBD Computations

`SST = ΣΣ(Xᵢⱼ − X̄..)²`

`SSTR = b × Σ(X̄.ⱼ − X̄..)²` (treatment column means)

`SSBL = k × Σ(X̄ᵢ. − X̄..)²` (block row means)

`SSE = SST − SSTR − SSBL`

**Worked Example (Air Traffic Controllers)**:
- k=3 workstations, b=6 controllers; X̄=14
- SST=70; SSTR=21 (df=2); SSBL=30 (df=5)
- SSE = 70−21−30 = **19** (df = 2×5 = 10)
- Without blocking: p=0.06 → Accept H₀
- With blocking: F=10.5/1.9=5.59, p=0.024 < 0.05 → **Reject H₀** ← decision reversed!

```python
import pandas as pd
import statsmodels.api as sm
from statsmodels.formula.api import ols

df = pd.read_excel('RBD.xlsx')
data = pd.melt(df.reset_index(), id_vars=['index'],
               value_vars=['system A','system B','system C'])
data.columns = ['block', 'treatment', 'value']

# One-way ANOVA (no blocking) — typ=1
model1 = ols('value ~ C(treatment)', data=data).fit()
anova_table1 = sm.stats.anova_lm(model1, typ=1)

# RBD (with blocking) — add block variable
model2 = ols('value ~ C(block) + C(treatment)', data=data).fit()
anova_table2 = sm.stats.anova_lm(model2, typ=1)
```

> 🔑 **In Python formula**: `'value ~ C(block) + C(treatment)'` — block added goes FIRST
> 🔑 **Type 1** for one-way ANOVA and RBD; **Type 2** for two-way ANOVA

---

## 📌 LECTURE 27 — Two-Way ANOVA (Factorial Design)

### Two-Way ANOVA vs RBD

| Feature | RBD | Two-Way ANOVA |
|---------|-----|--------------|
| Independent variables | 1 treatment + 1 block | **2 proper factors** |
| Block significance | NOT tested | **Both factors tested** |
| Interaction | No interaction | **Interaction tested** |

### Interaction Effect

- **No interaction**: Lines are **parallel** on interaction plot
- **Interaction exists**: Lines **cross** or are not parallel

> 🔑 **Factorial experiments are the ONLY way to discover interactions between variables**

---

### Two-Way ANOVA Table

| Source | SS | df | MS | F |
|--------|----|----|-----|---|
| Factor A | SSA | a−1 | MSA | MSA/MSE |
| Factor B | SSB | b−1 | MSB | MSB/MSE |
| Interaction AB | SSAB | (a−1)(b−1) | MSAB | MSAB/MSE |
| Error | SSE | ab(r−1) | MSE | — |
| Total | SST | abr−1 | — | — |

where **a** = levels of A, **b** = levels of B, **r** = replications per cell, **nT = a×b×r**

> 🔑 **ALWAYS divide by MSE — denominator is always error term!**

**Partition**:
`SST = SSA + SSB + SSAB + SSE`

---

### SSAB Formula (Interaction)

`SSAB = r × ΣΣ(X̄ᵢⱼ − X̄ᵢ. − X̄.ⱼ + X̄..)²`

(cell mean − row mean − column mean + grand mean)

---

### Worked Example (CAT Exam)

Factor A = Preparation Program (3 levels: 3hr, 1-day, 10wk)
Factor B = College Background (3 levels: Business, Engg, Arts)
r = 2 replications, nT = 3×3×2 = 18, Grand mean = 515

| Source | SS | df | MS | F | p |
|--------|----|----|-----|---|---|
| Prep Program (A) | 6100 | 2 | 3050 | 1.38 | >0.05 ← Accept |
| College (B) | 45300 | 2 | 22650 | 10.27 | <0.05 ← **Reject** |
| Interaction AB | 11200 | 4 | 2800 | 1.27 | >0.05 ← Accept |
| Error | 19850 | 9 | 2206 | — | |
| Total | 82450 | 17 | | | |

**Conclusion**: Only **college background** significantly affects CAT scores. Preparation program and interaction are NOT significant.

```python
import pandas as pd
import statsmodels.api as sm
from statsmodels.formula.api import ols

df2 = pd.read_excel('factorial.xlsx')

formula = 'value ~ C(college) + C(prep_program) + C(college):C(prep_program)'
model = ols(formula, df2).fit()
anova_table = sm.stats.anova_lm(model, typ=2)   # ← typ=2 for two-way ANOVA!
print(anova_table)
```

> 🔑 **`typ=1`** → for one-way ANOVA / RBD | **`typ=2`** → for two-way ANOVA (factorial)
> 🔑 **Interaction syntax**: `C(A):C(B)` (colon between factors)

---

## 📌 LECTURE 28 — Linear Regression I (Theory & Least Squares)

### Regression vs Hypothesis Testing

| Hypothesis Testing | Regression |
|-------------------|------------|
| Test one parameter (µ, σ, P) | Test a **model** with multiple parameters |
| Sample → population parameter | Sample model → population model |
| Works on same variable | Works across **two different variables** (X and Y) |

### Simple Linear Regression Model

**Population model** (truth):
`Y = β₀ + β₁X + ε`

**Estimated (sample) equation**:
`ŷ = b₀ + b₁x`

| Symbol | Meaning |
|--------|---------|
| β₀, β₁ | Population parameters (unknown) |
| b₀ | Sample Y-intercept (estimated) |
| b₁ | Sample slope (estimated) |
| ε | Error term (random variable) |
| ŷ | Predicted mean of Y for given X |

> 🔑 Regression predicts **mean of Y** (expected value), NOT actual Y

### Slope Interpretation
- **β₁ > 0**: Positive relationship (X↑ → Y↑)
- **β₁ < 0**: Negative relationship (X↑ → Y↓)
- **β₁ = 0**: No linear relationship (X and Y are independent)

---

### Least Squares Method

**Principle**: Find b₀ and b₁ that **minimize the sum of squared errors (SSE)**:
`Minimize Σ(yᵢ − ŷᵢ)² = Σ(yᵢ − b₀ − b₁xᵢ)²`

Why square? → Positive and negative errors don't cancel; larger deviations penalized more.

**Derivation result — slope formula**:
`b₁ = Cov(X,Y) / Var(X) = Σ(xᵢ−x̄)(yᵢ−ȳ) / Σ(xᵢ−x̄)²`

**Intercept formula**:
`b₀ = ȳ − b₁x̄`

> 🔑 The best-fit line ALWAYS passes through **(x̄, ȳ)** — the centroid of the data

---

### Shorthand Notation (Exam Quick Method)

| Notation | Formula |
|----------|---------|
| **Sₓₓ** | Σ(xᵢ−x̄)² |
| **Syy** | Σ(yᵢ−ȳ)² |
| **Sₓᵧ** | Σ(xᵢ−x̄)(yᵢ−ȳ) |
| **b₁** | Sₓᵧ / Sₓₓ |
| **SSE** | Syy − (Sₓᵧ)²/Sₓₓ |

> Know Sₓₓ, Syy, Sₓᵧ → can find everything!

### Relationship Among Key Statistics

`b₁ = Cov(X,Y)/Var(X)`

`r = sign(b₁) × √(R²)` → correlation coefficient from regression

`Covariance → Divided by both SDs → Correlation coefficient`

---

## 📌 LECTURE 29 — Linear Regression II (Worked Examples & Python)

### Complete Example (TV Ads → Car Sales)

| X (ads) | Y (cars) |
|---------|---------|
| 1 | 14 |
| 3 | 24 |
| 2 | 18 |
| 1 | 17 |
| 3 | 27 |

- x̄ = 2, ȳ = 20
- Sₓᵧ = Σ(x−x̄)(y−ȳ) = **20**; Sₓₓ = Σ(x−x̄)² = **4**
- **b₁ = 20/4 = 5** (slope: each additional ad → 5 more cars sold)
- **b₀ = ȳ − b₁x̄ = 20 − 5×2 = 10**
- **Regression equation: ŷ = 10 + 5x**

**Interpretation**: When TV ads increase by 1 → car sales increase by **5 units** (not 15!)

**Prediction**: If X=5 ads → ŷ = 10 + 5(5) = **35 cars**

```python
import pandas as pd
import statsmodels.api as sm
import statsmodels.formula.api as smf

tb1 = pd.read_excel('ads_data.xlsx')

# Method 1: OLS from statsmodels
t = tb1['TV ads']
c = tb1['cars sold']
t = sm.add_constant(t)            # adds b0 (intercept) column
model1 = sm.OLS(c, t).fit()
print(model1.summary())
# Output: b0=10, b1=5, R²=0.877

# Method 2: sklearn with train/test split
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split

X = data['hardness'].values.reshape(-1, 1)
y = data['tensile strength'].values
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

reg = LinearRegression().fit(X_train, y_train)
print(reg.intercept_, reg.coef_)  # b0, b1

y_pred = reg.predict(X_test)
score_test = reg.score(X_test, y_test)   # R² on test set
score_train = reg.score(X_train, y_train)  # R² on train set
```

> 🔑 **`train_test_split`**: test_size=0.20 → 20% test, 80% train. Use `random_state` for reproducibility.
> 🔑 **`reg.score(X, y)`** returns **R²** (coefficient of determination)
> 🔑 Regression = **supervised learning** (labels known in advance)

---

## 📌 LECTURE 30 — Linear Regression III (R², Significance Tests)

### Partitioning Total Variation (SST = SSR + SSE)

| Component | Formula | Meaning |
|-----------|---------|---------|
| **SST** | Σ(yᵢ − ȳ)² | Total variation in Y |
| **SSR** | Σ(ŷᵢ − ȳ)² | Explained by regression |
| **SSE** | Σ(yᵢ − ŷᵢ)² | Unexplained (residual error) |

`SST = SSR + SSE` (analogous to ANOVA: SST = SSTr + SSE)

---

### Coefficient of Determination (R²)

`R² = SSR / SST = 1 − SSE/SST`

- Range: **0 ≤ R² ≤ 1**
- **R²=1**: Perfect fit, all variation explained
- **R²=0**: No linear relationship
- **R²=0.877**: "87.7% of the variability in Y can be explained by the linear relationship with X"

> 🔑 Correlation coefficient: `r = sign(b₁) × √R²`
> 🔑 Correlation range: **−1 ≤ r ≤ 1** vs R² range: **0 ≤ R² ≤ 1**

**Example (Car Sales)**: SSR=100, SST=114; R²=100/114=**0.877** → b₁>0, so r=+√0.877=**+0.937**

---

### Model Assumptions (Error Term ε)

| Assumption | Meaning |
|-----------|---------|
| **Mean = 0** | E(ε) = 0; errors are random (positive and negative cancel) |
| **Constant variance** | Var(ε) = σ² for all X → **homoscedasticity** |
| **Independence** | Errors are independent (no pattern) |
| **Normality** | ε ~ N(0, σ²) |

> 🔑 Plot residuals to check assumptions — if a pattern appears (trend), model is violated!

---

### Standard Error of Estimate

`S² = MSE = SSE / (n−2)` (degrees of freedom = n−K−1 = n−1−1 = n−2 for simple regression)

`S = √MSE` = standard error of estimate

`Sb₁ = S / √Sₓₓ` = standard error of b₁

---

### Significance Testing for β₁

**H₀: β₁ = 0** (no linear relationship at population level)
**H₁: β₁ ≠ 0** (significant relationship exists)

**Method 1 — t-test** (df = n−2):
`t = b₁ / Sb₁`

Reject H₀ if |t| > t_(α/2, n−2) OR p-value < α

**Method 2 — F-test**:
`F = MSR / MSE` where `MSR = SSR / K` (K = number of independent variables)

For simple regression K=1: F = MSR/MSE, df₁=1, df₂=n−2

> 🔑 **t-test and F-test give identical results for simple linear regression!**
> 🔑 F-test preferred for multiple regression (tests all variables simultaneously)

**Method 3 — Confidence Interval for β₁**:
`b₁ ± t_(α/2, n−2) × Sb₁`

If **0 is NOT in the CI → Reject H₀** (slope is significant)

---

### Worked Example (Cars)

- b₁=5, Sb₁=1.08, n=5, df=3, t_(0.025,3)=3.182
- t_calc = 5/1.08 = **4.63** > 3.182 → **Reject H₀**
- CI: 5 ± 3.182×1.08 = 5 ± 3.44 = **(1.56, 8.44)**; 0 not in interval → **Reject H₀** ✓
- F_calc: SSR=100, SSE=14, MSR=100/1=100, MSE=14/3=4.67, F=100/4.67=**21.43**; p<0.05 → **Reject H₀** ✓

```python
# Python output interpretation:
# From result1.summary():
# - Coef (TV ads) = 5.0          → b₁ (slope)
# - Intercept = 10.0             → b₀
# - R-squared = 0.877            → R²
# - t-value for TV ads = 4.63    → test for β₁
# - P>|t| for TV ads < 0.05      → significant → reject H₀: β₁=0
# - F-statistic = 21.43          → overall model F-test
# - Prob (F-statistic) < 0.05    → model is significant
# - CI [0.025, 0.975] = [1.56, 8.44] → 0 not inside → β₁ significant
```

> ⚠️ **Caution**: Rejecting H₀: β₁=0 proves a **statistical** relationship, NOT necessarily a **cause-and-effect** relationship!

---

## 🎯 Week 6 Assignment — Key Questions & Answers

| Q | Topic | Answer | Working |
|---|-------|--------|---------|
| Q1 | SSE in 2-way ANOVA without SST | **(cannot determine)** | SST = SSA+SSB+SSAB+SSE; need SST |
| Q2 | What can factorial design detect? | **Interaction effects** | CRD/RBD can't detect interactions; factorial can |
| Q3 | In RBD, each treatment appears... | **Once in each block** | That's the definition of complete RBD |
| Q4 | F = (SSR/k)/(SSE/(n−k−1)) = (200/5)/(60/10) | **F ≈ 6.67** | MSR=200/5=40, MSE=60/10=6, F=40/6≈6.67 |
| Q5 | What does least squares minimize? | **Sum of squared residuals** | OLS principle: minimize Σ(y−ŷ)² |
| Q6 | Transformed predictors can be | **Any function of original predictors** | e.g., X², log(X), 1/X, etc. |
| Q7 | What does blocking accomplish? | **Removes nuisance variation** | Reduces MSE → increases F → better test |
| Q8 | Interpretation of slope | **Change in Y per one-unit change in X** | b₁ = ΔY/ΔX |
| Q9 | `pd.read_excel()` does what? | **Reads Excel data into pandas DataFrame** | Standard pandas function |
| Q10 | CI for slope excludes 0 → | **Slope is statistically significant** | 0 not in CI → reject H₀: β₁=0 |

---

## 💡 Quick-Fire Review Checklist

**RBD:**
- [ ] Use RBD when experimental units are **heterogeneous** (external nuisance factor present)
- [ ] SSE_RBD = SST − SSTR − SSBL (smaller than CRD's SSE)
- [ ] df_error in RBD = (k−1)(b−1); df_total = bk−1
- [ ] Python: `'value ~ C(block) + C(treatment)'`; `typ=1`
- [ ] Blocking does NOT test block significance — only removes its variance from MSE

**Two-Way ANOVA:**
- [ ] Tests: Factor A effect, Factor B effect, Interaction AB effect
- [ ] df_A = a−1; df_B = b−1; df_AB = (a−1)(b−1); df_E = ab(r−1)
- [ ] SST = SSA + SSB + SSAB + SSE
- [ ] F_A = MSA/MSE; F_B = MSB/MSE; F_AB = MSAB/MSE (ALWAYS MSE in denominator!)
- [ ] Parallel lines on interaction plot → NO interaction
- [ ] Python: `'value ~ C(A) + C(B) + C(A):C(B)'`; **`typ=2`** for two-way

**Simple Linear Regression:**
- [ ] ŷ = b₀ + b₁x; Y = β₀ + β₁X + ε (population)
- [ ] b₁ = Sₓᵧ/Sₓₓ = Cov(X,Y)/Var(X); b₀ = ȳ − b₁x̄
- [ ] Best-fit line passes through (x̄, ȳ)
- [ ] Sₓₓ = Σ(x−x̄)²; Syy = Σ(y−ȳ)²; Sₓᵧ = Σ(x−x̄)(y−ȳ)
- [ ] SSE = Syy − Sₓᵧ²/Sₓₓ
- [ ] SST = SSR + SSE; R² = SSR/SST (0 to 1)
- [ ] r = sign(b₁) × √R² (correlation: −1 to +1)
- [ ] 88% R² means "88% of Y's variability explained by X"
- [ ] MSE = SSE/(n−2); S = √MSE (standard error of estimate)
- [ ] Sb₁ = S/√Sₓₓ (standard error of slope)
- [ ] t-test: t = b₁/Sb₁, df=n−2; Reject H₀ if p<α or |t|>t_critical
- [ ] F-test: F = MSR/MSE, df=(1, n−2); both methods give same conclusion
- [ ] CI for β₁: b₁ ± t_(α/2,n−2) × Sb₁; if 0 not in CI → significant
- [ ] Reject H₀:β₁=0 ≠ cause-and-effect (only statistical!)
- [ ] Error assumptions: mean=0, constant variance, independence, normality
- [ ] R² vs r: R² is 0–1; r is −1 to +1; r = sign(b₁)√R²
- [ ] Python OLS: `sm.OLS(y, sm.add_constant(x)).fit(); result.summary()`
- [ ] sklearn: `LinearRegression().fit(X_train, y_train); reg.score(X_test, y_test)` → R²
- [ ] Train/test split: `train_test_split(X, y, test_size=0.2, random_state=42)`
