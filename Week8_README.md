# 📊 Week 8 — Data Analytics with Python (NPTEL)
### Lectures 36–40 | Topics: MLE (I & II), Logistic Regression (I & II), Linear vs. Logistic Comparison

---

## 📌 LECTURE 36 — Maximum Likelihood Estimation (MLE) I

### What is MLE?

Introduced by **R.A. Fisher** in the 1920s. MLE finds the **most likely density function** that generated the observed data.

| OLS (Least Squares) | MLE |
|---------------------|-----|
| Minimizes Σ(y−ŷ)² | Maximizes the likelihood of observing the data |
| Requires error ~ Normal | Works with **ANY distribution** |
| Closed-form solution | Often requires iterative computation |
| Used in linear regression | Used in logistic regression, generalized models |

> 🔑 **When to use MLE**: whenever the error term does NOT follow normal distribution (e.g., logistic regression)
> 🔑 MLE requires you to **assume a distribution first**, then estimate its parameters

---

### The Likelihood Function

**Step 1**: Write the probability of each observation (individual PMF/PDF)
**Step 2**: Multiply across all observations → **Likelihood Function L(θ)**
**Step 3**: Take **log** of L → **Log-Likelihood ℓ(θ)** (converts product to sum)
**Step 4**: Differentiate with respect to θ and set to 0 → solve for parameter

> 🔑 Maximizing L(θ) ≡ Maximizing log L(θ) — same answer, easier math!
> 🔑 Taking log converts **products → sums** (much easier to differentiate)

---

### MLE for Bernoulli/Binomial (Helmet Defect Example)

Data: n=10 helmets; helmets 1, 3, 10 are defective (X₁=X₃=X₁₀=1; others=0)
PMF: P(Xᵢ) = p^Xᵢ × (1−p)^(1−Xᵢ)

**Joint likelihood** (independent observations = multiply):
`L(p) = p × (1−p) × p × (1−p)×...= p³(1−p)⁷`

**Log-likelihood**:
`ℓ(p) = 3 ln(p) + 7 ln(1−p)`

**Differentiate & set to 0**:
`dℓ/dp = 3/p − 7/(1−p) = 0`
→ `3(1−p) = 7p` → **p̂ = 3/10 = 0.3** ← MLE estimate

> 🔑 p̂ = 0.3 = proportion of defects in sample (this makes intuitive sense!)
> 🔑 Binomial coefficient 10C3 is irrelevant to maximization (constant)

---

### MLE for Common Distributions

| Distribution | Parameter | Log-Likelihood | MLE Estimate |
|-------------|-----------|----------------|-------------|
| **Binomial** | p | 3ln(p) + 7ln(1−p) | **p̂ = x/n = sample proportion** |
| **Poisson** | μ | −nμ + nX̄ ln(μ) | **μ̂ = X̄ = sample mean** |
| **Exponential** | λ | n ln(λ) − λΣxᵢ | **λ̂ = n/Σxᵢ = 1/X̄** |
| **Normal** | μ, σ² | −(n/2)ln(2πσ²) − Σ(xᵢ−μ)²/2σ² | **μ̂ = X̄; σ̂² = Σ(xᵢ−X̄)²/n** |

> ⚠️ Normal MLE for σ² gives `Σ(xᵢ−X̄)²/n` (biased) NOT `Σ(xᵢ−X̄)²/(n−1)` (unbiased sample variance)
> 🔑 Unbiasedness ≠ MLE — two different principles can give different estimators!

**Poisson derivation key step**:
- Joint likelihood: `L(μ) = e^(−nμ) × μ^(nX̄)`
- Log-likelihood: `ℓ = −nμ + nX̄ ln(μ)`
- dℓ/dμ = 0 → `−n + nX̄/μ = 0` → **μ̂ = X̄** ✓

**Exponential derivation key step**:
- Log-likelihood: `ℓ = n ln(λ) − λΣxᵢ`
- dℓ/dλ = 0 → `n/λ = Σxᵢ` → **λ̂ = n/Σxᵢ = 1/X̄** ✓

---

## 📌 LECTURE 37 — Maximum Likelihood Estimation (MLE) II

### MLE vs OLS for Regression

Both give the **same answer** when errors ~ Normal!

**Example**: Data [(x=1,y=2), (x=4,y=6), (x=5,y=7), (x=6,y=9), (x=9,y=15)]
- OLS result: ŷ = −0.2882 + 1.6176x; σ̂(residuals) = 0.604
- MLE result: **slope = 1.6176, intercept = −0.2882, σ = 0.604** ← identical!

---

### MLE in Python for Regression

```python
import numpy as np
from scipy.optimize import minimize
import scipy.stats as stats

# Define negative log-likelihood function
def lik(parameters):
    m = parameters[0]   # slope
    b = parameters[1]   # intercept
    sigma = parameters[2]  # std dev of error term
    l = 0
    for i in np.arange(0, len(x)):
        y_exp = m * x[i] + b
        l += np.log(stats.norm.pdf(y[i], y_exp, sigma))  # log-likelihood
    return -l   # minimize negative = maximize positive

x = np.array([1, 4, 5, 6, 9])   # independent variable
y = np.array([2, 6, 7, 9, 15])  # dependent variable

# Minimize negative log-likelihood
like_model = minimize(lik, np.array([2, 2, 2]),   # initial guesses (arbitrary)
                      method='L-BFGS-B')
# Result: [slope=1.617, intercept=-0.288, sigma=0.604]
# Same as OLS: confirmed!
```

> 🔑 `np.array([2, 2, 2])` are **initial guesses** — can be any value; final answer is always same
> 🔑 Optimization methods available: `Nelder-Mead`, `Powell`, `CG`, `BFGS`, `Newton-CG`
> 🔑 MLE gives same β₀, β₁ as OLS when error ~ Normal (confirms OLS is a special case of MLE)

---

## 📌 LECTURE 38 — Logistic Regression I (Theory & Setup)

### When to Use Logistic Regression

| | Linear Regression | Logistic Regression |
|--|---|---|
| **Dependent variable** | Continuous | **Binary (0 or 1)** |
| **Independent variable** | Continuous or dummy | Continuous or dummy |
| **Error term** | Normal distribution | **Binomial distribution** |
| **Estimation method** | OLS (least squares) | **MLE (maximum likelihood)** |
| **Output** | Predicted Y value | **Probability P(Y=1)** |
| **Shape** | Straight line | **S-shaped (sigmoid) curve** |

**When Y is binary use logistic regression**: approve/reject loan, use coupon/not, pass/fail, male/female

---

### Logistic Regression Equation

**Population model**:
`E(Y) = P(Y=1 | x₁,…,xₚ) = e^(β₀+β₁x₁+…+βₚxₚ) / (1 + e^(β₀+β₁x₁+…+βₚxₚ))`

**Sample (estimated)**:
`ŷ = e^(b₀+b₁x₁+…+bₚxₚ) / (1 + e^(b₀+b₁x₁+…+bₚxₚ))`

Range: **0 ≤ ŷ ≤ 1** (always a valid probability) — This is why it works for binary outcomes!

**S-Shape explained**: As X increases from −∞ to +∞, ŷ moves smoothly from 0 → 1, crossing 0.5 at the inflection point.

---

### Worked Example (Simmons Stores — Coupon)

**Data**: n=100 customers; X₁=annual spending ($000s); X₂=credit card (1=yes, 0=no); Y=used coupon (1=yes, 0=no)

**Python code**:
```python
import statsmodels.api as sm

y = df['coupon']
x = df[['card', 'spending']]
x1 = sm.add_constant(x)   # adds intercept column

logit_model = sm.Logit(y, x1)
result = logit_model.fit()
print(result.summary2())
```

**Output**:
- Constant (b₀) = **−2.1464**
- Card (b₂) = **1.0987**
- Spending (b₁) = **0.3416**

**Estimated equation**:
`ŷ = e^(−2.1464 + 0.3416x₁ + 1.0987x₂) / (1 + e^(−2.1464 + 0.3416x₁ + 1.0987x₂))`

---

### Prediction/Interpretation

| X₁ (spending $000s) | X₂ (card) | ŷ (probability of using coupon) |
|---------------------|-----------|--------------------------------|
| 2 | 0 (no card) | **0.1880** (18.8%) |
| 2 | 1 (has card) | **0.4099** (41.0%) |

> 🔑 Having a card raises probability of coupon use from 18.8% → 41.0% for same spending level!

---

### G-Test: Overall Model Significance

**Equivalent to F-test in linear regression**

`G = −2 × [LL_null − LL_model] = −2 × [LL_null − LL_with_vars]`

`G = 2 × |LL_model − LL_null|`

**Formula**:
`G = 2 × (LL_model − LL_null)` = 2 × (−60.487 − (−67.307)) = 2 × 6.82 = **13.628**

**Distribution**: G follows **Chi-square (χ²) with df = p** (number of independent variables)
- Here: df = 2, G = 13.628, p-value = 0.001 < 0.05 → **Reject H₀** → model is significant!

**Python**:
```python
from scipy.stats import chi2
# LL values from result.summary2()
LL_null  = result.llnull     # log-likelihood with no variables = -67.307
LL_model = result.llf        # log-likelihood with variables = -60.487
G = 2 * (LL_model - LL_null)
p_value = 1 - chi2.cdf(G, df=2)

# Or directly:
chi2.pdf(13.628, 2)          # → p-value
```

---

## 📌 LECTURE 39 — Logistic Regression II (Significance, Odds, Interpretation)

### Wald Test (Z-test): Individual Variable Significance

**Equivalent to t-test in linear regression**

`z = b̂ᵢ / SE(b̂ᵢ)` → follows standard normal distribution

| Variable | Coefficient | SE | z = coef/SE | p-value | Significant? |
|---------|------------|-----|-------------|---------|-------------|
| Constant | −2.1464 | — | — | — | — |
| Card (x₂) | 1.0987 | 0.4447 | 1.0987/0.4447 = **2.47** | < 0.05 | ✓ **Yes** |
| Spending (x₁) | 0.3416 | 0.1287 | 0.3416/0.1287 = **2.65** | < 0.05 | ✓ **Yes** |

Both p-values < 0.05 → both variables are significant → use them in the model.

---

### Odds and Odds Ratio

**Odds**:
`Odds = P(success) / P(failure) = P(Y=1) / (1 − P(Y=1)) = P / (1−P)`

**Odds Ratio (OR)**:
`OR = Odds₁ / Odds₀` (ratio when moving from level 0 to level 1 of a variable)

**Example (Credit Card)**:
- Odds for card holder (x₂=1, x₁=2): 0.4099/(1−0.4099) = **0.6946**
- Odds for non-holder (x₂=0, x₁=2): 0.1880/(1−0.1880) = **0.2315**
- OR = 0.6946/0.2315 = **3.0**

> 🔑 **Interpretation**: Card holders are **3 times more likely** to use the coupon than non-holders (same spending level)

---

### KEY FORMULA: Odds Ratio from Coefficient

**Shortcut**: `Odds Ratio = e^(bᵢ)`

| Variable | bᵢ | OR = e^(bᵢ) | Interpretation |
|---------|-----|-------------|----------------|
| Card (x₂) | 1.0987 | e^1.0987 = **3.0** | Card holders 3× more likely to use coupon |
| Spending (x₁) | 0.3416 | e^0.3416 = **1.407** | Each $1000 more spending → 1.407× more likely |

**For c-unit change** (not just 1 unit):
`OR_c = e^(c × bᵢ)`

Example: Spending increases from $2,000 to $5,000 (c=3):
`OR₃ = e^(3 × 0.3416) = e^1.0248 = 2.79`
→ $5,000 spenders are **2.79× more likely** to use coupon than $2,000 spenders

```python
import numpy as np
# Odds ratio for each variable
OR_card = np.exp(1.0987)      # = 3.0
OR_spending = np.exp(0.3416)  # = 1.407

# For c-unit change
c = 3
OR_c = np.exp(c * 0.3416)    # = 2.79
```

---

### Logit Function (Linearization)

`logit(p) = ln(Odds) = ln[P/(1−P)] = β₀ + β₁x₁ + β₂x₂ + ... + βₚxₚ`

This shows: log-odds is a **linear function** of X → makes interpretation/estimation easier!

**Simmons equation logit form**:
`g(x₁,x₂) = −2.1464 + 0.3416x₁ + 1.0987x₂`

Then: `ŷ = P(Y=1) = e^g / (1 + e^g)`

---

### Managerial Strategy (Cut-off = 0.40 probability)

| Customer group | Send catalog if... |
|----------------|-------------------|
| Has Simmons card | Annual spending ≥ **$2,000** (prob ≥ 0.40) |
| No Simmons card | Annual spending ≥ **$6,000** (prob ≥ 0.40) |

---

## 📌 LECTURE 40 — Linear vs. Logistic Regression (Complete Comparison)

### Side-by-Side Comparison Table

| Concept | Linear Regression | Logistic Regression |
|---------|------------------|---------------------|
| **Dependent variable** | Continuous | Binary (0/1) |
| **Error distribution** | Normal | Binomial |
| **Estimation** | OLS (minimize SSE) | MLE (maximize likelihood) |
| **Shape** | Straight line | S-shaped sigmoid curve |
| **Total variation** | SST | −2LL (null model) |
| **Model error** | SSE | −2LL (proposed model) |
| **Explained variation** | SSR = SST − SSE | Δ(−2LL) = LL_null − LL_model |
| **Goodness of fit** | **R²** = SSR/SST | **Pseudo R²** = (LL_null − LL_model)/LL_null |
| **Overall model test** | **F-test** (F = MSR/MSE) | **G-test** (χ² with df=p) |
| **Individual var. test** | **t-test** (t = b/Sb) | **Wald/z-test** (z = b̂/SE) |
| **Sample size (rule of thumb)** | ≥5 obs per parameter | **≥10 obs per parameter** |

> 🔑 **Lower −2LL = better model fit** (analogous to lower SSE in linear regression)
> 🔑 **Pseudo R² ≠ R²** — do NOT interpret pseudo R² as % variability explained!

---

### Pseudo R² Formula

`R²_logit = (−2LL_null − (−2LL_model)) / (−2LL_null) = (LL_null − LL_model) / |LL_null|`

or equivalently: `R²_logit = 1 − (LL_model / LL_null)`

**3-Step Model Building for Logistic Regression**:
1. **Null model**: Run logistic regression with NO independent variables → get LL_null (baseline)
2. **Proposed model**: Add independent variables → get LL_model (must be lower)
3. **Assess**: G = 2(LL_model − LL_null); if significant → model improved

---

### Logistic vs. Discriminant Analysis

| | Logistic Regression | Discriminant Analysis |
|--|---------------------|----------------------|
| **Levels in Y** | 2 only (binary) | 2 or more |
| **Assumptions** | **Relaxed** (robust) | Strict: normality + equal variance + equal covariance |
| **Estimation** | MLE | Various |
| **Preferred when** | Assumptions not met OR 2 categories | More than 2 categories |

> 🔑 **Logistic regression is preferred** because discriminant analysis requires multivariate normality and equal covariance matrices — assumptions rarely met in practice!

---

### Coefficient Interpretation Comparison

| | Linear Regression | Logistic Regression |
|--|---|---|
| **Interpretation** | "1-unit ↑ in X → bᵢ change in Y (holding others constant)" | "1-unit ↑ in X → e^(bᵢ) change in **odds** (holding others constant)" |
| **Key metric** | bᵢ (regression coefficient) | **e^(bᵢ) = Odds Ratio** |
| **Scale** | Absolute change in Y | **Multiplicative** change in odds |

---

## 🎯 Week 8 Assignment — Key Questions & Answers

| Q | Question | Answer | Working |
|---|----------|--------|---------|
| Q1 | Difference between linear and logistic regression | **Linear: continuous Y; Logistic: binary Y** | Fundamental distinction |
| Q2 | Model fit measure in logistic regression | **−2 Log Likelihood (−2LL)** | Lower = better fit |
| Q3 | Overall significance test in logistic regression | **G-test** | G ~ χ² with df = p; equivalent to F-test |
| Q4 | How logistic regression coefficients are interpreted | **Via odds ratios** | 1-unit increase → e^(bᵢ) change in odds |
| Q5 | What does odds ratio measure? | **Change in odds for 1-unit increase in predictor** | OR = odds₁/odds₀ |
| Q6 | If odds = 3, what is P? | **P = 0.75** | odds = P/(1−P) = 3 → P = 3(1−P) → 4P=3 → P=0.75 |
| Q7 | G follows what distribution? | **Chi-square distribution** | G = 2(LL_model − LL_null) ~ χ²(df=p) |
| Q8 | Wald statistic formula | **z = β̂/SE(β̂)** | Follows standard normal distribution |
| Q9 | Function used to model probability in logistic regression | **Logistic (sigmoid) function** | S-shaped curve; output 0–1 |
| Q10 | If P=0.40, what is odds? | **0.67** | Odds = P/(1−P) = 0.40/0.60 = 0.667 |

---

## 💡 Quick-Fire Review Checklist

**MLE Fundamentals:**
- [ ] MLE: find θ that maximizes likelihood L(θ) = product of individual probabilities
- [ ] Use log-likelihood ℓ(θ) = log L(θ): products → sums; easier to differentiate
- [ ] Maximizing L ≡ Maximizing log L (same answer)
- [ ] Step: set dℓ/dθ = 0 → solve for θ̂
- [ ] Binomial MLE: p̂ = x/n (sample proportion) ✓
- [ ] Poisson MLE: μ̂ = X̄ (sample mean) ✓
- [ ] Exponential MLE: λ̂ = 1/X̄ (inverse of sample mean) ✓
- [ ] Normal MLE: μ̂ = X̄ (unbiased) but **σ̂² = Σ(xᵢ−X̄)²/n (biased!)**
- [ ] MLE for regression = OLS when errors ~ Normal (both give same β₀, β₁)
- [ ] Python MLE: `from scipy.optimize import minimize; minimize(neg_loglik, initial_guess, method='L-BFGS-B')`
- [ ] Initial guesses (np.array values) don't matter; final answer is always the same

**Logistic Regression:**
- [ ] Use when: **dependent variable is binary (0/1)**
- [ ] Model: ŷ = e^(b₀+b₁x₁+...)/(1 + e^(b₀+b₁x₁+...)); output = probability P(Y=1)
- [ ] S-shaped sigmoid curve; output always 0–1
- [ ] Error term follows **binomial** distribution (NOT normal → can't use OLS)
- [ ] Estimation: **MLE** (not OLS)
- [ ] Logit function: g(x) = b₀ + b₁x₁ + ... = ln(P/(1−P)) — makes it linear!
- [ ] Python: `sm.Logit(y, sm.add_constant(x)).fit(); result.summary2()`

**Significance Testing in Logistic Regression:**
- [ ] **G-test** (overall): G = 2(LL_model − LL_null); follows χ²(df=p); equivalent to F-test
- [ ] **Wald/z-test** (individual): z = b̂ᵢ/SE(b̂ᵢ); equivalent to t-test
- [ ] Both: p-value < 0.05 → significant
- [ ] LL_null from `result.llnull`; LL_model from `result.llf`

**Odds and Odds Ratio:**
- [ ] Odds = P/(1−P); if P=0.75 → Odds=3; if Odds=3 → P=0.75
- [ ] Odds Ratio = Odds₁/Odds₀ = e^(bᵢ) for 1-unit change
- [ ] For c-unit change: OR = e^(c×bᵢ)
- [ ] OR > 1 → positive effect; OR < 1 → negative effect; OR = 1 → no effect
- [ ] Interpretation: "1 unit ↑ in X → OR times change in odds of Y=1"

**Linear vs. Logistic Comparison (MCQ favourite):**
- [ ] SST ↔ −2LL_null; SSE ↔ −2LL_model; SSR ↔ Δ(−2LL)
- [ ] R² ↔ Pseudo R²; F-test ↔ G-test; t-test ↔ Wald z-test
- [ ] Min sample: linear = 5 per param; **logistic = 10 per param**
- [ ] Lower −2LL = better logistic model (analogous to lower SSE)
- [ ] Pseudo R²: NOT interpreted as % variability explained (unlike R²)
- [ ] Logistic preferred over discriminant analysis: robust (no normality/equal variance needed)
