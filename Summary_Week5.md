# ⚡ Week 5 SUMMARY — Data Analytics with Python
## Lectures 21–25 | Advanced Tests, F-Test, Sample Size & ANOVA

---

### 📍 F-Test for Variances
- F = S₁²/S₂² (LARGER variance always in numerator!)
- Distribution: right-skewed, always positive
- df₁ = n₁−1 (numerator); df₂ = n₂−1 (denominator)
- CI for σ₁/σ₂: if interval contains **1** → Do NOT reject H₀

---

### 📍 Z vs t Decision Table
| σ Known? | n | Use |
|----------|---|-----|
| Yes | Any | **Z** |
| No | < 30 | **t** |
| No | ≥ 30 | **Z** (acceptable; t≈Z) |

---

### 📍 Sample Size Formulas
| For | Formula | Note |
|-----|---------|------|
| **Mean µ** | n = Z²σ²/E² | σ unknown → use Range/4 |
| **Proportion P** | n = Z²PQ/E² | P unknown → use **P=0.5** (max n) |

---

### 📍 Why ANOVA (not multiple t-tests)
Family-wise error: `1 − (1−α)^comparisons`  
5 groups, α=0.05, 10 comparisons → 40% overall error!  
→ ANOVA controls error at α for any number of groups.

---

### 📍 ANOVA Table
| Source | SS | df | MS | F |
|--------|----|----|-----|---|
| Treatment | SSTR | k−1 | MSTR | **MSTR/MSE** |
| Error | SSE | nT−k | MSE | — |
| Total | SST | nT−1 | — | — |

- H₀: µ₁=µ₂=…=µₖ | Hₐ: Not all means equal
- Reject H₀ if F > F_table OR p-value ≤ α
- **SST = SSTR + SSE** (additive!)
- MSE always estimates σ²
- Assumptions: normality, equal variances, independence

---

### 📍 Post Hoc Tests (After ANOVA rejects H₀)
**Fisher's LSD** (equal n):
`LSD = t(α/2, nT−k) × √(2MSE/n)`
→ If |ȳᵢ−ȳⱼ| > LSD → means differ

**Tukey-Kramer HSD** (equal n):
`CR = Q(α, c, nT−c) × √(MSE/n)`
→ If |ȳᵢ−ȳⱼ| > CR → means differ

> `reject=False` in Tukey output = means ARE equal

---

### 📍 Worked Numbers (Tensile Strength, k=4, n=6)
- MSE = 6.51; LSD = 3.07; CR = 4.12
- Only 10% vs 15% pair has |diff| < both → µ₂ = µ₃

---

### 📍 ANOVA Shortcut Formulas
- SST = Σyᵢⱼ² − y..²/N
- SSTr = (1/n)Σyᵢ.² − y..²/N
- SSE = SST − SSTr

---

### 📍 Python Quick Ref
```python
from scipy import stats
import statsmodels.api as sm
from statsmodels.formula.api import ols
from statsmodels.stats.multicomp import MultiComparison

# Quick ANOVA
F, p = stats.f_oneway(group1, group2, group3)

# Full ANOVA table
model = ols('value ~ C(treatment)', data=df).fit()
sm.stats.anova_lm(model, typ=1)

# Tukey post hoc
mc = MultiComparison(df['value'], df['treatment'])
print(mc.tukeyhsd(alpha=0.05).summary())
# reject=False → means equal; reject=True → means differ

# Sample size
n = math.ceil(((z_alpha+z_beta)**2 * sigma**2) / delta**2)
```
