# ⚡ Week 6 SUMMARY — Data Analytics with Python
## Lectures 26–30 | RBD, Two-Way ANOVA & Simple Linear Regression

---

### 📍 RBD vs CRD vs Two-Way ANOVA
| Design | Use when | Test interactions? |
|--------|---------|-------------------|
| **CRD** | Units homogeneous | No |
| **RBD** | Units heterogeneous (nuisance factor) | No |
| **Two-Way ANOVA** | Two proper factors of interest | **Yes** |

- RBD: `SSE = SST − SSTR − SSBL` (blocking removes nuisance from error)
- df_error RBD = (k−1)(b−1); b = blocks, k = treatments

---

### 📍 Two-Way ANOVA Table
| Source | df | F |
|--------|----|----|
| Factor A | a−1 | MSA/**MSE** |
| Factor B | b−1 | MSB/**MSE** |
| Interaction AB | (a−1)(b−1) | MSAB/**MSE** |
| Error | ab(r−1) | — |

- Always divide by **MSE** (denominator)
- Parallel lines on interaction plot → **No interaction**
- Crossed/unparallel lines → **Interaction exists**
- Only factorial design can detect interactions!

---

### 📍 Simple Linear Regression
`Y = β₀ + β₁X + ε` (population) | `ŷ = b₀ + b₁x` (sample)

**Formulas:**
| Stat | Formula |
|------|---------|
| Slope | b₁ = Sₓᵧ/Sₓₓ = Cov(X,Y)/Var(X) |
| Intercept | b₀ = ȳ − b₁x̄ |
| Sₓₓ | Σ(x−x̄)² |
| Sₓᵧ | Σ(x−x̄)(y−ȳ) |
| SSE | Syy − Sₓᵧ²/Sₓₓ |

- Best-fit line ALWAYS passes through **(x̄, ȳ)**

---

### 📍 R² and Variance Partition
- SST = SSR + SSE
- **R² = SSR/SST = 1 − SSE/SST** (0 to 1)
- r = sign(b₁) × √R² (correlation; −1 to +1)
- MSE = SSE/(n−2); S = √MSE; Sb₁ = S/√Sₓₓ

---

### 📍 Significance of β₁
- **t-test**: t = b₁/Sb₁, df=n−2
- **F-test**: F = MSR/MSE; for p=1: same result as t-test
- **CI**: b₁ ± t(α/2,n−2)×Sb₁ → if 0 NOT in CI → β₁ significant
- Reject H₀:β₁=0 ≠ cause-and-effect (statistical only)

---

### 📍 Error Assumptions (Residuals ε)
E(ε)=0 | Constant variance | Independence | Normality ~ N(0,σ²)

---

### 📍 Python Quick Ref
```python
import statsmodels.api as sm
from statsmodels.formula.api import ols

# RBD (block first, treatment second)
model = ols('value ~ C(block) + C(treatment)', data=df).fit()
sm.stats.anova_lm(model, typ=1)   # typ=1 for RBD

# Two-Way ANOVA (typ=2!)
model = ols('value ~ C(A) + C(B) + C(A):C(B)', data=df).fit()
sm.stats.anova_lm(model, typ=2)   # typ=2 for two-way

# Simple regression (statsmodels)
t = sm.add_constant(x_col)
model = sm.OLS(y_col, t).fit()
print(model.summary())   # R², b₀, b₁, p-values

# Regression (sklearn)
from sklearn.linear_model import LinearRegression
reg = LinearRegression().fit(X_train, y_train)
reg.intercept_, reg.coef_     # b₀, b₁
reg.score(X_test, y_test)     # R²
```
