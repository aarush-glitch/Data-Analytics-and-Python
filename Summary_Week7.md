# ⚡ Week 7 SUMMARY — Data Analytics with Python
## Lectures 31–35 | Regression: CI/PI, Residuals, Multiple & Dummy Variables

---

### 📍 CI vs PI for Regression
| | Confidence Interval | Prediction Interval |
|-|---------------------|---------------------|
| **Estimates** | Mean of ALL Y at xp | ONE individual Y at xp |
| **Width** | **Narrower** | **Wider** (+extra S²) |
| **Narrowest at** | x = x̄ | x = x̄ |
| **Shape** | Curved | Curved |

PI variance = S²[1 + 1/n + (xp−x̄)²/Sₓₓ] (extra **1** inside)

---

### 📍 Residual Analysis (4 Assumptions)
| Assumption | Check via | Good pattern |
|---------|-----------|-------------|
| E(ε) = 0 | Residual vs X plot | Random scatter |
| Constant var | Residual vs X plot | Horizontal band |
| Independence | Residual vs X | No pattern |
| Normality | QQ plot | Points on 45° diagonal |

**Violations:**
- Funnel/cone shape → **Heteroscedasticity** → transform Y (log)
- Curve/U-shape → **Non-linearity** → add x² term
- Points outside ±2 → **Outliers**

> If violated → ALL inference (t, F, CI, PI) becomes INVALID

---

### 📍 Multiple Regression
`ŷ = b₀ + b₁x₁ + b₂x₂ + ... + bₚxₚ`

| df | Formula |
|----|---------|
| Regression | p (# of X vars) |
| Error | **n−p−1** |
| Total | n−1 |

- bᵢ interpretation: "1-unit ↑ in xᵢ → bᵢ change in ŷ, **holding all others constant**"
- Adding variables: SSR↑, SSE↓, R² always ↑ → **R²_adj** penalizes

**Adjusted R² formula:** `R²_adj = 1 − (1−R²) × (n−1)/(n−p−1)`
- R²↑ and R²_adj↑ → variable useful
- R²↑ but R²_adj↓ → variable is **noise** → remove it!

---

### 📍 Significance Tests
- **F-test** (overall model): H₀: β₁=…=βₚ=0; F=MSR/MSE; reject if p < α
- **t-test** (individual): t=bᵢ/Sbᵢ; df=n−p−1; reject if p < α
- Run F-test FIRST; then individual t-tests

---

### 📍 Dummy Variables
- k categories → **k−1 dummy variables**; omitted = reference group
- β₂ > 0 → coded-1 group has HIGHER Y
- β₂ < 0 → coded-1 group has LOWER Y
- Both groups have **same slope**, different intercept
- ANOVA via dummies → same F and p-values!

```python
# Create dummies
dummies = pd.get_dummies(df['category'])  # one-hot
dummies = dummies.drop('reference', axis=1)  # drop one (k-1 rule)
```

---

### 📍 Python Quick Ref
```python
from statsmodels.formula.api import ols, OLS
import statsmodels.api as sm, seaborn as sns

# Multiple regression
reg = ols('y ~ x1 + x2', data=df).fit()
print(reg.summary())   # R², Adj R², F, t, p-values, CI

# Residual plot
sns.residplot(x='x1', y='y', data=df)

# QQ plot (normality check)
sm.qqplot(reg.resid, line='45')   # points on line = normal

# Standardized residuals (outliers > ±2)
inf = reg.get_influence()
inf.resid_studentized_external
```
