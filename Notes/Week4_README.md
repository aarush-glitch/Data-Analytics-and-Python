# 📊 Week 4 — Data Analytics with Python (NPTEL)
### Lectures 16–20 | Topics: Hypothesis Testing (One-Sample, Two-Sample, Errors, Proportions)

---

## 📌 LECTURE 16 — Hypothesis Testing I (Foundations)

### What is Hypothesis Testing?
A procedure to determine whether a **statement about a population parameter** should be **rejected or not**, using sample data.

### Null (H₀) vs Alternative (Hₐ) Hypothesis

| H₀ (Null) | Hₐ (Alternative) |
|-----------|-----------------|
| Tentative assumption; **status quo** | What researcher wants to prove |
| Always contains **= sign** | **Never** contains = sign |
| "Nothing has happened" | "Something has changed" |
| Accept → no action needed | Accept → action required |

> 🔑 **Formulation Rule**: Start with Hₐ (researcher's claim). H₀ is always the complement/status quo.

### 3 Forms of Hypothesis

| H₀ | Hₐ | Test Type |
|----|-----|---------|
| µ ≥ µ₀ | µ < µ₀ | **Left-tailed** (lower-tailed) |
| µ ≤ µ₀ | µ > µ₀ | **Right-tailed** (upper-tailed) |
| µ = µ₀ | µ ≠ µ₀ | **Two-tailed** |

> 🔑 **Identify tail direction by the sign of Hₐ**: < → left tail, > → right tail, ≠ → two-tail

**Examples**:
- New drug reduces cholesterol more: Hₐ: µ_new < µ_old (left tail)
- Bonus plan increases sales: Hₐ: sales_new > sales_old (right tail)
- Milk bottle filling: Hₐ: µ ≠ 1000 ml (two-tail — overfilling AND underfilling are problems)

---

### Type 1 and Type 2 Errors

|  | H₀ True | H₀ False |
|--|---------|---------|
| **Reject H₀** | ❌ **Type I Error** (α) | ✅ Correct |
| **Do Not Reject H₀** | ✅ Correct | ❌ **Type II Error** (β) |

| Error | Name | Symbol | Also Called |
|-------|------|--------|------------|
| Reject true H₀ | **Incorrect Rejection** | **α** | Producer's Risk, Level of Significance |
| Accept false H₀ | **False Acceptance** | **β** | Consumer's Risk |

> ⚠️ **α + β ≠ 1** — they are independent probabilities!

> 🔑 We say **"Do not reject H₀"** — NOT "accept H₀" — because we can't prove H₀ is true, only fail to disprove it.

---

### 3 Approaches for Hypothesis Testing

| Approach | Decision Rule | Best For |
|----------|-------------|---------|
| **p-value** | Reject H₀ if p-value ≤ α | Most software uses this |
| **Critical Value** | Reject H₀ if Z_calc is in rejection region | Manual calculation |
| **Confidence Interval** | Reject H₀ if µ₀ not in CI | Two-tailed tests |

### p-value Approach
`p-value` = probability of observing test statistic as extreme or more, assuming H₀ is true.

- **p-value ≤ α** → Reject H₀
- **p-value > α** → Do Not Reject H₀
- High p-value = sample data supports H₀

```python
from scipy import stats
# Left-tailed p-value (Z = -1.46)
p_value = stats.norm.cdf(-1.46)     # left area directly

# Right-tailed p-value (Z = 2.29)
p_value = 1 - stats.norm.cdf(2.29)  # right area = 1 - left area

# Two-tailed p-value (Z = 2.74)
p_value = 2 * (1 - stats.norm.cdf(2.74))
```

### Critical Value Approach

**Critical Z values** (memorize!):
| α | Left-tail | Right-tail | Two-tail (±) |
|---|-----------|-----------|-------------|
| 0.10 | -1.28 | +1.28 | ±1.645 |
| 0.05 | -1.645 | +1.645 | **±1.96** |
| 0.01 | -2.326 | +2.326 | ±2.576 |

```python
# Find critical value for α=0.05 right-tailed
stats.norm.ppf(0.95)    # → 1.645

# Find critical value for α=0.05 two-tailed (right)
stats.norm.ppf(0.975)   # → 1.96

# Find lower critical value (left tail, α=0.05)
stats.norm.ppf(0.05)    # → -1.645
```

---

## 📌 LECTURE 17 — Hypothesis Testing II (Worked Examples)

### Z-Test for µ (σ Known) — Step-by-Step Framework

```
Step 1: H₀ and Hₐ
Step 2: α (significance level)
Step 3: Compute Z = (X̄ − µ₀) / (σ/√n)
Step 4: Find p-value OR critical value
Step 5: Decision and Conclusion
```

### Worked Example 1 — One-Tailed (Right-Tailed) Z-Test

**Pizza delivery**: X̄=32 min, n=30, σ=10, µ₀=30, α=0.05

- H₀: µ ≤ 30 | Hₐ: µ > 30 (right-tailed — Hₐ sign is >)
- Z = (32−30)/(10/√30) = **1.09**
- p-value = 1 − Φ(1.09) = **0.137**
- Since 0.137 > 0.05 → **Do NOT reject H₀**
- Conclusion: Insufficient evidence that pizza takes more than 30 min

**Critical value check**: Z_critical = 1.645; Z_calc = 1.09 < 1.645 → Accept H₀ ✓ (same conclusion)

---

### Worked Example 2 — Two-Tailed Z-Test

**Milk filling**: X̄=505 ml, n=30, σ=10, µ₀=500, α=0.03

- H₀: µ = 500 | Hₐ: µ ≠ 500 (two-tailed — both over/under filling are bad)
- Z = (505−500)/(10/√30) = **2.74**
- p-value = 2 × (1−Φ(2.74)) = 2 × 0.00307 = **0.0061**
- 0.0061 < 0.03 → **Reject H₀**

**Critical value check**: α/2 = 0.015 → Z_critical = ±2.17; Z_calc = 2.74 > 2.17 → Reject H₀ ✓

**CI Method**: X̄ ± Zα/2(σ/√n) = 505 ± 2.17(10/√30) = **(501.04, 508.96)**
- µ₀ = 500 NOT in interval → **Reject H₀** ✓

---

## 📌 LECTURE 18 — Hypothesis Testing III (t-Test & Proportion Test)

### t-Test for µ (σ Unknown)

When σ is unknown, replace σ with sample standard deviation s:

`t = (X̄ − µ₀) / (s/√n)` with **n−1 degrees of freedom**

**When to use t vs Z**:
| Situation | Use |
|-----------|-----|
| σ known | **Z-test** |
| σ unknown | **t-test** |
| n large (n>30), σ unknown | t-test still (they converge) |

```python
from scipy import stats
import numpy as np

# t-test for one sample
X = np.array([10, 12, 20, 21, 22, 24, 18, 15])
t_stat, p_value = stats.ttest_1samp(X, popmean=15)
# Returns: t statistic and TWO-SIDED p-value
# For one-tailed test: p_value / 2

# For one-tailed RIGHT test: if t_stat > 0, p = p_value/2; else p = 1 - p_value/2
```

> 🔑 **Python's `ttest_1samp` always returns 2-sided p-value. Divide by 2 for one-tailed tests!**

**Example — Ice cream sales** (n=20, µ₀=10, α=0.05, right-tailed):
- t from Python = −0.384, two-sided p = 0.7053
- One-tailed p = 0.7053/2 = **0.353**
- 0.353 > 0.05 → **Do not reject H₀**

---

### Z-Test for Proportion P

When dealing with proportions (binary outcomes):

`Z = (p̂ − P₀) / √(P₀(1−P₀)/n)`

> 🔑 **Use P₀ (assumed population proportion) in the denominator**, NOT p̂

**Assumption**: nP₀ ≥ 5 AND nQ₀ ≥ 5 (np and n(1-p) both ≥ 5 for normal approximation)

```python
from statsmodels.stats.proportion import proportions_ztest

count = 67      # number of successes
nobs = 120      # sample size
P0 = 0.5        # assumed population proportion

z_stat, p_value = proportions_ztest(count, nobs, P0)
# For two-tailed test: compare p_value > α
```

**Example — Drunk driving** (n=120, 67 drunk, P₀=0.5, α=0.05, two-tailed):
- p̂ = 67/120 = 0.558
- σ_p̂ = √(0.5×0.5/120) = 0.0456
- Z = (0.558−0.5)/0.0456 = **1.28**
- p-value = 2×(1−Φ(1.28)) = 2×0.1003 = **0.2006**
- 0.2006 > 0.05 → **Do NOT reject H₀** (no significant change in proportion)

---

## 📌 LECTURE 19 — Errors in Hypothesis Testing (Type I, II & Power)

### Computing Type I Error (α) in Practice

**Example** (burning rate, µ=50, σ=2.5, n=10, acceptance region: 48.5 < X̄ < 51.5):
- SE = σ/√n = 2.5/√10 = **0.7906**
- α = P(X̄ < 48.5 | µ=50) + P(X̄ > 51.5 | µ=50)
- = Φ(−1.898) + (1−Φ(1.898)) = **0.0288 + 0.0288 = 0.057**

```python
from scipy import stats

def z_value(x, mu, sem):
    z = (x - mu) / sem
    if z < 0:
        return stats.norm.cdf(z)
    else:
        return 1 - stats.norm.cdf(z)

sem = 0.7906
# Left tail probability
alpha_left = z_value(48.5, 50, sem)   # → 0.0288
# Right tail probability
alpha_right = z_value(51.5, 50, sem)  # → 0.0288
alpha = alpha_left + alpha_right       # → 0.057 = 5.7%
```

**Reducing Type I Error**:
1. Widen acceptance region (but this increases β!)
2. Increase sample size n (reduces both α and β!)

---

### Computing Type II Error (β)

**β = probability of accepting H₀ when H₀ is actually false** (true µ ≠ assumed µ)

**Key insight**: When true mean is CLOSE to assumed mean → β is HIGH (hard to detect small differences)

**Example** (µ₀=50, true µ=52, acceptance region: [48.5, 51.5], n=10, σ=2.5):
- β = P(X̄ falls in acceptance region | true µ=52)
- = P(48.5 ≤ X̄ ≤ 51.5 | µ=52)
- Z = (51.5−52)/0.7906 = **−0.632**
- β = Φ(−0.632) = **0.264**

```python
def type_2(mu1, mu2, sigma, n, alpha):
    # Find critical value X_bar from mu1's distribution
    z = stats.norm.ppf(alpha)
    x_bar = mu1 + z * (sigma / np.sqrt(n))
    
    # Find Z for mu2's distribution at that x_bar
    z2 = (x_bar - mu2) / (sigma / np.sqrt(n))
    
    if mu1 > mu2:
        beta = 1 - stats.norm.cdf(z2)
    else:
        beta = stats.norm.cdf(z2)
    return beta

# Example: mu0=8.3, true_mu=7.4, sigma=3.1, n=60, alpha=0.05
beta = type_2(8.3, 7.4, 3.1, 60, 0.05)   # → 0.2729
```

---

### α and β Relationship (Critical!)

| Action | Effect on α | Effect on β |
|--------|------------|------------|
| Widen acceptance region | Decreases | **Increases** |
| Narrow acceptance region | Increases | Decreases |
| **Increase sample size n** | **Decreases** | **Decreases** ← best option! |
| Decrease true-assumed µ difference | — | Increases |

> 🔑 **Only increasing n can reduce both α and β simultaneously!**
> 🔑 **α + β ≠ 1** — they are NOT complementary!

---

### Power of Test

**Power = 1 − β = P(correctly rejecting H₀ when it IS false)**

- High power → better test → more reliable decisions
- Power curve: plots **Power (1−β) vs true value of µ**
- As true µ moves farther from assumed µ → Power increases

**Factors affecting β (and thus Power)**:
| Factor | As it increases... | β changes... |
|--------|-------------------|------------|
| |µ_true − µ₀| (difference) | Larger | β **decreases** (power up) |
| n (sample size) | Larger | β **decreases** (power up) |
| σ (population SD) | Larger | β **increases** (power down) |
| α (significance level) | Larger | β **decreases** (power up) |

---

## 📌 LECTURE 20 — Two-Sample Hypothesis Tests

### Why Two-Sample Tests?
Compare two populations: Is µ₁ = µ₂? Is P₁ = P₂? Is σ₁² = σ₂²?

**Distribution of X̄₁ − X̄₂**:
- E(X̄₁ − X̄₂) = µ₁ − µ₂
- Var(X̄₁ − X̄₂) = σ₁²/n₁ + σ₂²/n₂ ← **(ADD variances when finding difference!)**

---

### Two-Sample Test Summary

| Scenario | Test | Statistic |
|----------|------|-----------|
| σ₁², σ₂² **known** | **Z-test** | Z = (X̄₁−X̄₂−D₀) / √(σ₁²/n₁ + σ₂²/n₂) |
| σ₁², σ₂² unknown, **assumed equal** | **t-test (pooled)** | t = (X̄₁−X̄₂) / (Sp√(1/n₁+1/n₂)), df=n₁+n₂−2 |
| σ₁², σ₂² unknown, **unequal** | **t-test (Welch's)** | Separate SE formula |

---

### Case 1: σ₁², σ₂² Known → Z-Test

`Z = (X̄₁ − X̄₂ − D₀) / √(σ₁²/n₁ + σ₂²/n₂)`

where D₀ = hypothesized difference (usually 0).

**CI for µ₁ − µ₂**:
`(X̄₁ − X̄₂) ± Zα/2 × √(σ₁²/n₁ + σ₂²/n₂)`

```python
from scipy import stats
import numpy as np

def z_and_p(x1, x2, sigma1, sigma2, n1, n2):
    z = (x1 - x2) / np.sqrt(sigma1**2/n1 + sigma2**2/n2)
    p = stats.norm.cdf(z) if z < 0 else 1 - stats.norm.cdf(z)
    return z, p

# Paint example: x1=121, x2=112, sigma=8, n1=n2=10
z, p = z_and_p(121, 112, 8, 8, 10, 10)   # z=2.52, p≈0.006 → reject H₀
```

**Worked Example (Paint drying)**:
- X̄₁=121, X̄₂=112, σ₁=σ₂=8, n₁=n₂=10, α=0.05 (right-tailed)
- H₀: µ₁=µ₂ | Hₐ: µ₁>µ₂
- Z = (121−112)/√(64/10+64/10) = 9/√12.8 = **2.52**
- Z_critical(α=0.05, right) = 1.645
- 2.52 > 1.645 → **Reject H₀** (new ingredient reduces drying time)

---

### Case 2: σ₁², σ₂² Unknown, **Assumed Equal** → Pooled t-Test

**Pooled Variance** (weighted average of two sample variances):
`Sp² = [(n₁−1)S₁² + (n₂−1)S₂²] / (n₁+n₂−2)`

**df = n₁ + n₂ − 2**

`t = (X̄₁ − X̄₂) / [Sp × √(1/n₁ + 1/n₂)]`

```python
# Assuming equal variances
t_stat, p_val = stats.ttest_ind(a, b, equal_var=True)

# NOT equal variance (Welch's):
t_stat, p_val = stats.ttest_ind(a, b, equal_var=False)
```

**Worked Example (Catalyst)**:
- n₁=n₂=8, X̄₁=92.255, X̄₂=92.733, S₁=2.39, S₂=2.98
- Sp² = (7×2.39² + 7×2.98²)/(8+8−2) = (39.82+62.23)/14 = **7.30** → Sp=**2.70**
- t = (92.255−92.733)/(2.70×√(1/8+1/8)) = −0.478/1.35 = **−0.35**
- df=14, α/2=0.025 → t_critical = ±2.145
- |−0.35| < 2.145 → **Do NOT reject H₀** (no significant difference between catalysts)

---

### Hypothesis Forms for Two-Sample Tests

| H₀ | Hₐ | Test Direction |
|----|-----|--------------|
| µ₁−µ₂ ≥ 0 (µ₁ ≥ µ₂) | µ₁−µ₂ < 0 | Left-tailed |
| µ₁−µ₂ ≤ 0 (µ₁ ≤ µ₂) | µ₁−µ₂ > 0 | Right-tailed |
| µ₁−µ₂ = 0 (µ₁ = µ₂) | µ₁−µ₂ ≠ 0 | Two-tailed |

---

## 🎯 Week 4 Assignment — Key Questions & Answers

| Q | Topic | Answer | Working |
|---|-------|--------|---------|
| Q1 | Z-value: X̄=32, µ₀=30, σ=10, n=30 | **(b) 1.09** | Z = (32−30)/(10/√30) = 2/1.826 = 1.09 |
| Q2 | p-value=0.137, α=0.05, one-tailed | **(c) Do not reject H₀** | p=0.137 > α=0.05 → cannot reject |
| Q3 | Rejecting a true null hypothesis | **(c) Type I Error** | Type I = incorrect rejection |
| Q4 | n fixed, α decreases → β: | **(b) Increases** | α and β have inverse relationship |
| Q5 | Z-test for proportion: 67/120 vs P₀=0.5 | **(c) 1.28** | Z=(0.558−0.5)/√(0.5×0.5/120) ≈ 1.28 |
| Q6 | p-value=0.2006, α=0.05 | **(c) Do not reject H₀** | 0.2006 > 0.05 → cannot reject |
| Q7 | σ₁², σ₂² known → test for µ₁−µ₂ | **(c) z statistic** | Known pop variance → Z-test |
| Q8 | Which hypothesis contains '='? | **(c) Null hypothesis** | H₀ always has = sign |
| Q9 | Reduces BOTH Type I and II errors | **(c) Increasing sample size** | Only larger n reduces both α and β |
| Q10 | Agra campaign: previously P=0.60, want increase | **(d) H₀: P ≤ 0.60, Hₐ: P > 0.60** | Research hypothesis = Hₐ = increase |

---

## 💡 Quick-Fire Review Checklist
- [ ] H₀ = status quo, always has =; Hₐ = researcher's claim, never has =
- [ ] Tail direction: look at sign of Hₐ (< = left, > = right, ≠ = two)
- [ ] Type I (α): reject true H₀ = "incorrect rejection" = producer's risk
- [ ] Type II (β): accept false H₀ = "false acceptance" = consumer's risk
- [ ] α + β ≠ 1 (critical!)
- [ ] Say "do not reject H₀" not "accept H₀"
- [ ] p-value ≤ α → reject H₀; p-value > α → do not reject H₀
- [ ] Z-test: σ known; t-test: σ unknown (uses s, df=n−1)
- [ ] Z formula: (X̄ − µ₀)/(σ/√n); t formula: (X̄ − µ₀)/(s/√n)
- [ ] Two-tailed p-value = 2 × (one-tail area); `ttest_1samp` returns two-sided, divide by 2 for one-tailed
- [ ] Proportion Z-test: Z = (p̂ − P₀)/√(P₀Q₀/n); use **P₀** in denominator
- [ ] Python: `proportions_ztest(count, nobs, P0)` from statsmodels
- [ ] Only **increasing n** reduces both α and β simultaneously
- [ ] Power = 1 − β = P(correctly reject false H₀)
- [ ] Power curve: plots 1−β vs true µ; larger difference → higher power
- [ ] β increases when: n↓, |µ_true−µ₀|↓, σ↑, α↓
- [ ] Two-sample: Var(X̄₁−X̄₂) = σ₁²/n₁ + σ₂²/n₂ (ADD variances!)
- [ ] Two-sample Z (σ known): Z = (X̄₁−X̄₂)/√(σ₁²/n₁+σ₂²/n₂)
- [ ] Two-sample t (σ unknown, equal): pooled Sp² = [(n₁−1)S₁²+(n₂−1)S₂²]/(n₁+n₂−2), df=n₁+n₂−2
- [ ] Python: `ttest_ind(a, b, equal_var=True)` for equal variance; `equal_var=False` for unequal
