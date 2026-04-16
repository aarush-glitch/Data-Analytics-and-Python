# ⚡ Week 3 SUMMARY — Data Analytics with Python
## Lectures 11–15 | Sampling, CLT & Confidence Intervals

---

### 📍 Sampling Types
| Type | Key Trait |
|------|----------|
| **Simple Random** | Each unit equally likely |
| **Stratified** | Strata = homogeneous within, heterogeneous between |
| **Systematic** | Every Kth unit; K = N/n |
| **Cluster** | Clusters = heterogeneous within, homogeneous between |

Non-random: Convenience, Judgment, Quota, Snowball — NOT for inferential stats.

---

### 📍 Central Limit Theorem (CLT)
- E(X̄) = µ; **Standard Error = σ/√n** (NOT σ)
- X̄ ~ Normal for n ≥ 25 (regardless of population shape!)
- Z for sampling mean: `Z = (X̄ − µ) / (σ/√n)`

### 📍 Sampling Distribution of Proportion
- p̂ = X/n; E(p̂) = P; σ_p̂ = √(PQ/n)
- Z = (p̂ − P) / √(PQ/n); Valid when nPQ > 5

### 📍 Sampling Distribution of Variance
- χ² = (n−1)s²/σ²; df = n−1
- Chi-square: always **positive**, **right-skewed**

---

### 📍 Confidence Interval Formulas
| Situation | Formula | Key |
|-----------|---------|-----|
| µ, σ known | X̄ ± Zα/2(σ/√n) | Use Z |
| µ, σ unknown | X̄ ± t(n−1,α/2)(s/√n) | Use t; df=n−1 |
| Proportion P | p̂ ± Zα/2√(p̂q̂/n) | nPQ>5 |
| Variance σ² | (n−1)s²/χ²_upper < σ² < (n−1)s²/χ²_lower | Chi-square |

### 📍 Critical Z Values (MEMORIZE!)
| CL | α | Zα/2 |
|----|---|------|
| 80% | 0.20 | **1.28** |
| 90% | 0.10 | **1.645** |
| 95% | 0.05 | **1.96** |
| 99% | 0.01 | **2.576** |

---

### 📍 t-Distribution
- Bell-shaped, symmetric; fatter tails than Z
- t → Z as df → ∞
- t table: rows=df, columns=α/2; **body = critical values** (NOT probabilities!)

| df | t at 95% |
|----|---------|
| 10 | 2.228 |
| 20 | 2.086 |
| ∞ | 1.96 (= Z) |

---

### 📍 Key Rules
- Correct CI interpretation: "95% of such intervals WILL contain µ" — NOT "95% chance µ is in this interval"
- Reduce margin of error: ↑n, ↓σ, or ↓confidence level
- Finite population correction: apply when n > 5% of N AND without replacement
  - Corrected SE: (σ/√n) × √((N−n)/(N−1))

---

### 📍 Python Quick Ref
```python
from scipy.stats import norm, t, chi2
norm.ppf(0.975)              # Z for 95% CI = 1.96
t.ppf(0.975, df=24)          # t for 95%, df=24
chi2.ppf(0.99, 6)            # χ² critical at α=0.01, df=6
```
