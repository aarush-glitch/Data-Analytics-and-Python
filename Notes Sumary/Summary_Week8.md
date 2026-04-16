# ⚡ Week 8 SUMMARY — Data Analytics with Python
## Lectures 36–40 | MLE & Logistic Regression

---

### 📍 MLE vs OLS
| | OLS | MLE |
|-|-----|-----|
| Minimizes | Σ(y−ŷ)² | Maximizes likelihood L(θ) |
| Error dist. | Must be Normal | **Any distribution** |
| Same result? | YES (when errors ~ Normal) | YES |

- Log-likelihood: products → sums; maximizing L ≡ maximizing log L
- dℓ/dθ = 0 → MLE estimate

**MLE Results:**
| Dist | MLE of parameter |
|------|-----------------|
| Binomial | p̂ = x/n (sample proportion) |
| Poisson | μ̂ = X̄ (sample mean) |
| Exponential | λ̂ = 1/X̄ |
| Normal | μ̂ = X̄; **σ̂² = Σ(xᵢ−X̄)²/n ← biased!** |

---

### 📍 Linear vs Logistic Regression
| Concept | Linear | Logistic |
|---------|--------|---------|
| Y | Continuous | **Binary (0/1)** |
| Error | Normal | **Binomial** |
| Method | OLS | **MLE** |
| Shape | Line | **S-shaped sigmoid** |
| Total var | SST | −2LL_null |
| Model error | SSE | −2LL_model |
| Goodness of fit | **R²** | **Pseudo R²** |
| Overall test | **F-test** | **G-test** (χ², df=p) |
| Individual test | **t-test** | **Wald z-test** |
| Min sample | 5/param | **10/param** |

- Lower −2LL = better logistic model (like lower SSE)
- **Pseudo R² ≠ R²** — don't interpret as % explained!

---

### 📍 Logistic Regression Model
`ŷ = e^(b₀+b₁x₁+…) / (1 + e^(b₀+b₁x₁+…))`  always 0–1

**Logit (linear form):** `g(x) = b₀ + b₁x₁ + … = ln(P/(1−P))`

---

### 📍 Odds & Odds Ratio
- **Odds** = P/(1−P)
- If P=0.75 → Odds=3; if Odds=3 → P=0.75
- **OR = e^(bᵢ)** for 1-unit change ← KEY FORMULA
- For c-unit change: OR = e^(c×bᵢ)
- OR > 1 = positive effect; OR < 1 = negative; OR = 1 = no effect

---

### 📍 G-Test Formula
`G = 2 × (LL_model − LL_null)` → follows χ²(df=p)  
`p-value < α` → model is significant (like F-test)

---

### 📍 Python Quick Ref
```python
import statsmodels.api as sm
import numpy as np
from scipy.optimize import minimize

# Logistic regression
logit_model = sm.Logit(y, sm.add_constant(x)).fit()
print(logit_model.summary2())
# .llnull = LL of null model; .llf = LL of model

# G-test manually
from scipy.stats import chi2
G = 2 * (logit_model.llf - logit_model.llnull)
p = 1 - chi2.cdf(G, df=2)

# Odds ratios
OR = np.exp(logit_model.params)   # e^(coef) for each variable

# MLE (manual)
from scipy.optimize import minimize
result = minimize(neg_loglik, initial_guess=[2,2,2], method='L-BFGS-B')
```
