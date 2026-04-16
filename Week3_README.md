# 📊 Week 3 — Data Analytics with Python (NPTEL)
### Lectures 11–15 | Topics: Python for Distributions, Sampling, Central Limit Theorem, Confidence Intervals

---

## 📌 LECTURE 11 — Python Demo for Distributions

### Binomial Distribution in Python
```python
from scipy.stats import binom

# P(X = exactly 19) out of 25, p = 0.65
binom.pmf(k=19, n=25, p=0.65)     # → 0.09 (9%)

# P(X ≤ 2) CDF — "2 or fewer unemployed workers"
binom.cdf(2, 20, 0.06)            # → 0.885 (88.5%)

# Single value pmf
binom.pmf(k=10, n=20, p=0.4)     # → 0.117
```

### Poisson Distribution in Python
```python
from scipy.stats import poisson

# P(X = 3) with mean λ = 2
poisson.pmf(3, 2)

# Bank: λ=3.2/4min, P(X=5 in 4min)
poisson.pmf(5, 3.2)               # → 11.39%

# P(X > 7): Use 1 - CDF
prob = poisson.cdf(7, 3.2)
1 - prob                          # → P(X > 7)

# Unit mismatch: λ=3.2/4min, want X=10 in 8min → adjust λ
poisson.pmf(10, 3.2 * 2)         # λ becomes 6.4 for 8min → 0.0527
```

> 🔑 **Always match units of λ and X in Poisson!**

### Uniform Distribution in Python
```python
from scipy.stats import uniform
import numpy as np

u = np.arange(27, 40, 1)         # array [27..39]

# Mean: loc=start, scale=range width
uniform.mean(loc=27, scale=12)   # → 33 (= (27+39)/2)

# CDF: P(30 ≤ X ≤ 35): CDF at 35 minus CDF at 30
uniform.cdf(35, loc=27, scale=12) - uniform.cdf(30, loc=27, scale=12)

# Standard Deviation
uniform.std(loc=200, scale=982)   # → 283.47
```

### Normal Distribution in Python
```python
from scipy.stats import norm

# P(X ≤ 68) where µ=65.5, σ=2.5
norm.cdf(68, 65.5, 2.5)          # → 0.8413

# P(X > 700) where µ=494, σ=100  
1 - norm.cdf(700, 494, 100)

# P(300 ≤ X ≤ 600)
norm.cdf(600, 494, 100) - norm.cdf(300, 494, 100)

# Reverse: Find X when P(X < x) = 0.95 (standard normal)
norm.ppf(0.95)                    # → 1.645 (Z value)

# Find Z for left-side area
norm.ppf(1 - 0.672)              # → -0.459
```

### Hypergeometric Distribution in Python
```python
from scipy.stats import hypergeom

# P(X ≥ 1): "1 or more" companies from Silicon Valley
# N=18, n=3 selected, A=12 successes in population
p = hypergeom.sf(0, 18, 3, 12)   # sf = 1 - CDF → 0.9754

# P(X ≤ 1): "1 or fewer" Hispanic officers
# N=18 officers, n=5 chosen, A=11 Hispanic
p = hypergeom.cdf(1, 18, 5, 11)  # → 0.04738

# Note: hypergeom(M=pop, n=draws, N=successes_in_pop)
```

### Exponential Distribution in Python
```python
from scipy.stats import expon

# Poisson mean: 1.38 defects per 20 min
# Exponential mean = 1/lambda (time between defects)
mu1 = 1 / 1.38                   # mean in same unit as Poisson

# P(time < 15 min) — convert to same unit: 15/20 = 0.75
expon.cdf(0.75, scale=mu1)       # → 0.644
```

---

## 📌 LECTURE 12 — Sampling and Sampling Distributions

### Descriptive vs. Inferential Statistics

| Type | Description |
|------|-------------|
| **Descriptive** | Collecting, presenting, describing data as-is. No generalization. |
| **Inferential** | Drawing conclusions about a **population** based on **sample** data. |

### Why Sample (not Census)?
- Less time & cost
- Sometimes destructive testing
- Population may be inaccessible
- Statistical results can still be precise enough

### When to Use Census Instead:
- Need extremely high accuracy
- Small population feasible to survey fully
- Sample results are suspect/non-representative

---

### Types of Sampling

#### Random (Probability) Sampling
Every unit has equal chance of selection → eliminates bias → suitable for inferential stats.

| Method | How | Key Trait |
|--------|-----|-----------|
| **Simple Random** | Random number table / computer | Each object equally likely; independent |
| **Stratified** | Divide into strata; sample from each | Strata homogeneous within, heterogeneous between |
| **Systematic** | Select every Kth unit (K = N/n) | Ordered frame; first unit selected randomly from first K |
| **Cluster** | Divide into clusters; randomly select clusters | Each cluster = miniature population; heterogeneous within |

> 🔑 **Stratified**: Within-stratum = homogeneous | **Cluster**: Within-cluster = heterogeneous

**Systematic Example**: N=10,000, n=50 → K=200. Start at 45th → 45, 245, 445, 645 ...

#### Non-Random (Non-Probability) Sampling — NOT for inferential stats
| Method | Description |
|--------|-------------|
| **Convenience** | Easiest to reach |
| **Judgment** | Researcher's expertise selects who qualifies |
| **Quota** | Fixed number from each group |
| **Snowball** | Respondents refer others |

---

### Sampling Errors
- **Sampling Error**: Sample doesn't represent population
- **Non-Sampling Errors**: Missing data, recording errors, data entry errors, poorly designed questionnaires, response errors

---

### Sampling Distribution of the Sample Mean
When you take repeated samples of size n and plot the sample means:

> 🔑 **Key Insight**: Population is uniform, exponential, or any shape → as you increase sample size → distribution of sample means approaches **Normal**

**Central Limit Theorem (CLT)**:
- `E(X̄) = µ` (mean of sampling distribution = population mean)
- `σ_X̄ = σ/√n` (standard error — decreases as n increases)
- As n → large (n ≥ 25), X̄ ~ Normal regardless of population shape

> 🔑 **Standard Error (SE) = σ/√n** — This is NOT population SD. It's the SD of the sampling distribution.

**Z value for sampling distribution**:
`Z = (X̄ − µ) / (σ/√n)`

**Example**: µ=8, σ=3, n=36. P(7.8 < X̄ < 8.2)?
- σ_X̄ = 3/√36 = 0.5
- Z₁ = (7.8−8)/0.5 = **−0.4**, Z₂ = (8.2−8)/0.5 = **+0.4**
- P(−0.4 < Z < 0.4) = 2×0.1554 = **0.3108**

---

## 📌 LECTURE 13 — Sampling Distributions: Proportion & Variance

### Three Sampling Distributions Summary

| Sample Statistic | Plotted Distribution |
|-----------------|---------------------|
| **Sample Mean** (X̄) | Normal (by CLT) |
| **Sample Proportion** (p̂) | Normal (when nPQ > 5) |
| **Sample Variance** (s²) | Chi-Square distribution |

---

### Sampling Distribution of Sample Proportion

`p̂ = X/n` (number with characteristic / sample size)

**Parameters** (derived from Binomial → CLT):
- `E(p̂) = P` (mean = population proportion)
- `σ_p̂ = √(PQ/n)` where Q = 1 − P

**Z for proportions**:
`Z = (p̂ − P) / √(PQ/n)`

**Condition**: nPQ > 5 (approx. to Normal)

**Example**: P=0.4, n=200. P(0.40 ≤ p̂ ≤ 0.45)?
- σ_p̂ = √(0.4×0.6/200) = **0.0346**
- Z₁ = (0.40−0.40)/0.0346 = 0
- Z₂ = (0.45−0.40)/0.0346 = **1.44**
- P(0 ≤ Z ≤ 1.44) = **0.4251**

---

### Sampling Distribution of Sample Variance — Chi-Square

When samples are drawn from a **normal** population:

`χ² = (n−1)s² / σ²`

This statistic follows a **Chi-Square distribution** with **n−1 degrees of freedom**.

**Properties of Chi-Square**:
- Always **positive** (it's a squared quantity — same as Z²)
- **Right-skewed** (not symmetric like normal/Z)
- As degrees of freedom increase → approaches normal shape
- Different distribution for each df

**Connection to Z**:
- Z = (X−µ)/σ → Z² = (X−µ)²/σ²
- Σ(X−µ)²/σ² = chi-square
- So chi-square = sum of squared Z values

---

### Degrees of Freedom (df)
**Definition**: Number of values **free to vary** after a constraint (sample mean) is known.
- For n observations: **df = n − 1**
- One degree of freedom is "lost" because once you know the mean, the last value is fixed.

**Analogy**: 3 chairs, 3 students → 1st student: 3 choices, 2nd: 2 choices, 3rd: **no choice** → df = 2 = n−1

---

### Chi-Square Application (Finding max sample variance)
**Example**: σ²=16 (σ=4), n=14, α=0.05. Find max s² such that P(s² > k) ≤ 0.05.
- df = 14−1 = **13**
- χ²(13, 0.05) = **22.36** (from table)
- (n−1)k / σ² = χ² → (13)k / 16 = 22.36
- k = **27.52**
- If s² > 27.52 → evidence population variance exceeds 16

---

## 📌 LECTURE 14 — Confidence Interval Estimation (σ² Known)

### Key Concepts

| Term | Definition |
|------|-----------|
| **Estimator** | A random variable (formula) used to estimate a population parameter (e.g., X̄ estimates µ) |
| **Estimate** | A specific numerical value from the estimator |
| **Point Estimate** | Single number (e.g., X̄ = 50). Less informative. |
| **Interval Estimate** | Range with stated confidence level (e.g., 45 < µ < 55). More informative. |
| **Confidence Interval** | The specific interval [LCL, UCL] |
| **Confidence Level** | Probability (1−α) that the interval contains the parameter |

### Key Estimators (Unbiased)
| Estimator | Estimates |
|-----------|----------|
| X̄ (sample mean) | µ (population mean) |
| s² (sample variance) | σ² (population variance) |
| p̂ (sample proportion) | P (population proportion) |

**Unbiasedness**: E(θ̂) = θ. Bias = E(θ̂) − θ. Unbiased → Bias = 0

**Efficiency**: Among unbiased estimators, most efficient = **smallest variance**

---

### Confidence Interval — Population Mean (σ Known)

**Formula**:
`X̄ − Zα/2 (σ/√n) < µ < X̄ + Zα/2 (σ/√n)`

Or equivalently: **X̄ ± ME**

**Margin of Error**: `ME = Zα/2 × (σ/√n)`

**Common Z values (memorize!)**:
| Confidence Level | α | α/2 | Zα/2 |
|-----------------|---|-----|------|
| 80% | 0.20 | 0.10 | **1.28** |
| 90% | 0.10 | 0.05 | **1.645** |
| 95% | 0.05 | 0.025 | **1.96** |
| 99% | 0.01 | 0.005 | **2.576** |

**Reducing Margin of Error**:
- Reduce σ (improve process consistency)
- Increase n (larger sample)
- Reduce confidence level (accept more uncertainty)

**Example**: n=11, X̄=2.20, σ=0.35, 95% CI:
- ME = 1.96 × (0.35/√11) = **0.2068**
- CI: **1.9932 < µ < 2.4068**

**Interpretation**: "We are 95% confident the true mean resistance is between 1.99 and 2.41 ohms."

> 🔑 **Correct interpretation**: 95% of all CIs constructed this way will contain µ. NOT: "there's a 95% chance µ is in this interval."

---

## 📌 LECTURE 15 — Confidence Interval (σ Unknown, Proportion, Variance)

### CI for Population Mean — σ UNKNOWN (Use t-distribution)

**When**: Population variance σ² unknown **AND** n < 30

**t-statistic**: `t = (X̄ − µ) / (s/√n)` follows t-distribution with **n−1 df**

**Formula**:
`X̄ − t(n−1, α/2) × (s/√n) < µ < X̄ + t(n−1, α/2) × (s/√n)`

**t vs Z Properties**:
- Both bell-shaped and symmetric
- t has **flatter/fatter tails** than Z
- As df → ∞, t → Z (they converge)
- Z table gives areas; **t table gives critical values** (not areas)

**t-values at 95% confidence** (compare with Z=1.96):
| df | t value |
|----|---------|
| 10 | 2.228 |
| 20 | 2.086 |
| 30 | 2.042 |
| ∞ | 1.960 (= Z) |

**Example**: n=25, X̄=50, s=8, 95% CI:
- df = 24, α/2 = 0.025 → t(24, 0.025) = **2.064**
- ME = 2.064 × (8/√25) = 3.302
- CI: **46.698 < µ < 53.302**

---

### CI for Population Proportion P

**Formula** (large n, nPQ > 5):
`p̂ − Zα/2 × √(p̂q̂/n) < P < p̂ + Zα/2 × √(p̂q̂/n)`

> Note: Use sample p̂ and q̂ = 1 − p̂ (since population P is unknown!)

**Example**: n=100, 25 are left-handed, 95% CI:
- p̂ = 25/100 = 0.25
- σ_p̂ = √(0.25×0.75/100) = 0.0433
- ME = 1.96 × 0.0433 = 0.0849
- CI: **0.1651 < P < 0.3349** (16.51% to 33.49%)

---

### CI for Population Variance σ²

Uses Chi-Square distribution (sample variances from normal pop → χ²):

**Formula**:
`(n−1)s² / χ²(n−1, α/2) < σ² < (n−1)s² / χ²(n−1, 1−α/2)`

> 🔑 **Lower limit**: divide by LARGER chi-square value (right tail = α/2)
> **Upper limit**: divide by SMALLER chi-square value (left tail = 1−α/2)

**Example**: n=17, s=74, 95% CI for σ²:
- df = 16
- χ²(16, 0.025) = **28.25** (right tail)
- χ²(16, 0.975) = **6.91** (left tail)
- Lower σ² = 16×74² / 28.25 = **3037**
- Upper σ² = 16×74² / 6.91 = **12,683**
- CI for σ: **55.1 < σ < 112.6**

---

### Finite Population Correction Factor
Apply when: sample size > **5% of population** AND sampling **without replacement**

**Corrected Standard Error**: `σ_X̄ = (σ/√n) × √((N−n)/(N−1))`

**Corrected Variance CI**: multiply variance by `(N−n)/(N−1)`

---

### Quick Reference: When to Use Which Distribution?

| Situation | Distribution | Formula Key |
|-----------|-------------|-------------|
| CI for µ, σ known | **Z** | Zα/2 × σ/√n |
| CI for µ, σ unknown, any n | **t** | t(n−1, α/2) × s/√n |
| CI for P (proportion) | **Z** (large n) | Zα/2 × √(p̂q̂/n) |
| CI for σ² (variance) | **Chi-Square** (χ²) | (n−1)s²/χ² |

---

## 🎯 Week 3 Assignment — Key Questions & Answers

| Q | Topic | Answer | Working |
|---|-------|--------|---------|
| Q1 | Variance of sampling distribution: µ=50, σ²=100, n=25 | **(c) 4** | Var(X̄) = σ²/n = 100/25 = 4 |
| Q2 | Which about CLT is FALSE? | **(b) "Requires normal population"** | CLT works for ANY population shape |
| Q3 | σ of sampling dist. of p̂: P=0.6, n=150 | **(a) 0.04** | √(0.6×0.4/150) = √(0.0016) = 0.04 |
| Q4 | ME for 95% CI: n=36, σ=12 | **(b) 3.92** | ME = 1.96 × (12/√36) = 1.96 × 2 = 3.92 |
| Q5 | Which estimator is unbiased? | **(b) Sample variance** | s² is unbiased for σ² |
| Q6 | CI width decreases when: | **(b) Sample size increases** | Larger n → smaller SE → narrower CI |
| Q7 | Correct interpretation of 95% CI | **(c)** "95% of CIs in repeated sampling contain µ" | Classic frequentist interpretation |
| Q8 | Distribution when σ² unknown + small n | **(d) t distribution** | Z requires σ known; t uses s and n−1 df |
| Q9 | Z-score for X=65, µ=50, σ=10 | **(b) 1.5** | Z = (65−50)/10 = 1.5 |
| Q10 | df for chi-square with n=16 | **(a) 15** | df = n−1 = 16−1 = 15 |

---

## 💡 Quick-Fire Review Checklist
- [ ] Descriptive = past data only; Inferential = generalize to population
- [ ] Random sampling types: **S**imple, **S**tratified, **S**ystematic, **C**luster
- [ ] Stratified: within-stratum homogeneous; Cluster: within-cluster heterogeneous
- [ ] CLT: sample mean ALWAYS normal if n ≥ 25 (regardless of population shape)
- [ ] Standard Error = σ/√n (NOT σ); increases n → SE decreases
- [ ] Z for sampling mean: Z = (X̄ − µ) / (σ/√n)
- [ ] Sample proportion: p̂ = X/n; E(p̂) = P; σ_p̂ = √(PQ/n)
- [ ] Z for proportions: Z = (p̂ − P) / √(PQ/n)
- [ ] Sample variance → Chi-square distribution (right-skewed, always positive)
- [ ] df = n−1; chi-square has different shapes for different df
- [ ] Degrees of freedom = observations free to vary after mean is known
- [ ] CI for µ (σ known): X̄ ± Zα/2(σ/√n)
- [ ] CI for µ (σ unknown): X̄ ± t(n−1,α/2)(s/√n)
- [ ] Z values: 80%→1.28, 90%→1.645, 95%→**1.96**, 99%→2.576
- [ ] t distribution: bell-shaped, flatter tails than Z; t → Z as df → ∞
- [ ] t table: rows=df, columns=α/2, **body contains t values (not probabilities)**!
- [ ] CI for P: p̂ ± Zα/2 × √(p̂q̂/n), valid when nPQ > 5
- [ ] CI for σ²: (n−1)s² / χ²_upper < σ² < (n−1)s² / χ²_lower
- [ ] Finite population correction: apply when n > 5% of N AND without replacement
- [ ] Correct CI interpretation: "95% of such intervals will contain µ" (NOT "95% chance µ is here")
