# ⚡ Week 4 SUMMARY — Data Analytics with Python
## Lectures 16–20 | Hypothesis Testing

---

### 📍 Hypothesis Setup
| | H₀ (Null) | Hₐ (Alternative) |
|-|-----------|-----------------|
| **Contains** | Always **=** | Never = |
| **Represents** | Status quo | Researcher's claim |
| **Say** | "Do not reject H₀" | (never "accept") |

- Tail direction from **Hₐ sign**: < = left, > = right, ≠ = two-tailed

---

### 📍 Error Types
| | H₀ True | H₀ False |
|--|---------|---------|
| **Reject H₀** | ❌ Type I (α) | ✅ Correct |
| **Don't Reject** | ✅ Correct | ❌ Type II (β) |

- α = Producer's Risk | β = Consumer's Risk
- **α + β ≠ 1** (independent!)
- **Increasing n = only way to reduce BOTH α and β**
- Power = 1 − β

---

### 📍 Test Statistics
| Scenario | Stat | Formula |
|----------|------|---------|
| σ known | **Z** | (X̄−µ₀)/(σ/√n) |
| σ unknown | **t** (df=n−1) | (X̄−µ₀)/(s/√n) |
| Proportion | **Z** | (p̂−P₀)/√(P₀Q₀/n) ← use **P₀** |
| Two-sample (σ known) | **Z** | (X̄₁−X̄₂)/√(σ₁²/n₁+σ₂²/n₂) |
| Two-sample (σ eq.) | **t (pooled)** | df=n₁+n₂−2; Sp²=[(n₁−1)S₁²+(n₂−1)S₂²]/(n₁+n₂−2) |
| Paired samples | **t** | t=d̄/(Sd/√n); df=n−1 |

---

### 📍 Decision Rules
- **p-value ≤ α → Reject H₀** | p-value > α → Do Not Reject H₀
- Critical value method: compare Z_calc to Z_critical

### 📍 Critical Z Values
| α | Left | Right | Two-tail |
|---|------|-------|---------|
| 0.10 | −1.28 | +1.28 | ±1.645 |
| 0.05 | −1.645 | +1.645 | **±1.96** |
| 0.01 | −2.326 | +2.326 | ±2.576 |

---

### 📍 Power & β Factors
| Factor increases → | β | Power |
|---------------------|---|-------|
| |µ_true−µ₀| | ↓ | ↑ |
| n | ↓ | ↑ |
| σ | ↑ | ↓ |
| α | ↓ | ↑ |

---

### 📍 Python Quick Ref
```python
from scipy import stats
from statsmodels.stats.proportion import proportions_ztest

# One-sample t-test
t_stat, p_value = stats.ttest_1samp(X, popmean=15)
# → divide p by 2 for one-tailed test!

# Two-sample t-test
stats.ttest_ind(a, b, equal_var=True)   # pooled
stats.ttest_ind(a, b, equal_var=False)  # Welch's

# Paired t-test
stats.ttest_rel(before, after)

# Proportion test
proportions_ztest(count=67, nobs=120, value=0.5)

# Critical values
stats.norm.ppf(0.975)   # Z for 95% two-tailed = 1.96
stats.norm.ppf(0.95)    # Z for 95% one-tailed = 1.645
```

### 📍 Var of Difference (MCQ TRAP!)
Var(X̄₁ − X̄₂) = σ₁²/n₁ **+** σ₂²/n₂ ← **ADD variances even for difference!**
