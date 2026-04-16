# ⚡ Week 10 SUMMARY — Data Analytics with Python
## Lectures 46–50 | Chi-Square Tests & Cluster Analysis Intro

---

### 📍 Chi-Square: Which Test to Use?
| Situation | Test |
|-----------|------|
| 2 means | t-test |
| >2 means | ANOVA |
| 2 proportions | Z proportion test |
| **>2 proportions / categorical** | **Chi-Square** |

---

### 📍 Test of Independence
- H₀: Variables ARE independent | H₁: Variables are NOT independent
- `eᵢⱼ = (Row i total × Col j total) / N`
- `χ² = ΣΣ (fₒ−fₑ)²/fₑ`
- **df = (r−1)(c−1)**; all fₑ ≥ 5 (else merge categories)
- Reject H₀ if χ²_calc > χ²_critical OR p < α

---

### 📍 Goodness of Fit Test
- H₀: Data FOLLOWS [distribution]; H₁: Does NOT ← "NOT" in H₁
- `df = k − 1 − p` where p = number of estimated parameters

| Distribution | p | df |
|-------------|---|----|
| Uniform | 0 | k−1 |
| Poisson | 1 | k−2 |
| Normal | 2 | k−3 |

- Poisson μ = Σ(f×n)/Σf; Expected = n × P(X=x)
- Uniform: all expected = Total/k
- Normal: divide into k intervals using norm.ppf(j/k); each has equal expected freq
- Merge cells until all fₑ ≥ 5; k = intervals AFTER merging

---

### 📍 Cluster Analysis Basics
- **Unsupervised**: No Y variable; discovers unknown groups
- Within-cluster: SIMILAR; Between-cluster: DISSIMILAR
- vs Discriminant Analysis: Discriminant = supervised (groups known)

**Standardization:**
- Removes unit dependency; Z-score: zᵢf = (xᵢf − mf)/sf
- Use **MAD** (not SD) for robustness to outliers
- Warning: standardization can sometimes DESTROY cluster structure!

---

### 📍 Distance Measures
| Measure | Formula | Analogy |
|---------|---------|---------|
| **Euclidean** | √[Σ(xᵢf−xⱼf)²] | Bird's flight (shortest) |
| **Manhattan** | Σ\|xᵢf−xⱼf\| | City block grid |
| **Minkowski** | [Σ\|xᵢf−xⱼf\|ᵖ]^(1/p) | Generalization (p=1→Manhattan, p=2→Euclidean) |

- Manhattan ≥ Euclidean (always)
- Distance matrix: symmetric; diagonal = 0; show lower triangle
- 4 properties: d≥0; d(i,i)=0; d(i,j)=d(j,i); triangle inequality

---

### 📍 Python Quick Ref
```python
from scipy.stats import chi2_contingency, chisquare, chi2, poisson, norm
import pandas as pd

# Chi-square independence test
obs = pd.pivot_table(df, values='id', index='A', columns='B', aggfunc=len)
chi2_stat, p, dof, expected = chi2_contingency(obs)

# Critical value
stats.chi2.ppf(0.99, 6)   # α=0.01, df=6

# Goodness of fit
chi2_stat, p_val = chisquare(observed_freq, f_exp=expected_freq)

# Poisson expected frequencies
E = [n * poisson.pmf(i, mu) for i in range(k)]

# Normal interval boundaries
intervals = [norm.ppf(j/6, loc=mean, scale=std) for j in range(1, 6)]
```
