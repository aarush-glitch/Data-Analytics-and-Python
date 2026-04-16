# 📊 Week 5 — Data Analytics with Python (NPTEL)
### Lectures 21–25 | Topics: Two-Sample Tests (continued), F-Test, Sample Size, ANOVA, Post Hoc Analysis

---

## 📌 LECTURE 21 — Two-Sample Tests II (Unequal Variances, Paired, Proportions)

### Case 3: σ₁², σ₂² Unknown, **Assumed UNEQUAL** → Welch's t-Test

When you cannot assume equal population variances:

`t = (X̄₁ − X̄₂) / √(S₁²/n₁ + S₂²/n₂)`

**Degrees of freedom** (Welch's formula — memorize structure, not exact):

`υ = (S₁²/n₁ + S₂²/n₂)² / [(S₁²/n₁)²/(n₁−1) + (S₂²/n₂)²/(n₂−1)]`

> 🔑 υ is always **smaller** than n₁+n₂−2 (more conservative)

```python
# Unequal variance (Welch's):
t_stat, p_val = stats.ttest_ind(metro, rural, equal_var=False)

# Equal variance (Pooled):
t_stat, p_val = stats.ttest_ind(a, b, equal_var=True)
```

**Worked Example (Arsenic)**: Metro (X̄₁=12.5, S₁=7.63, n₁=10) vs Rural (X̄₂=27.5, S₂=15.3, n₂=10)
- υ ≈ 13 (from formula), t_critical(α=0.05, two-tail, df=13) = ±2.160
- t_calc = −2.77 < −2.160 → **Reject H₀** (arsenic levels ARE different)

---

### Paired Samples (Dependent) t-Test

When SAME group is measured **before and after** a treatment (or the same units are tested twice):

`dᵢ = Xᵢ − Yᵢ` (find difference for each pair)

`t = (d̄ − µD) / (Sd/√n)` with **n−1 degrees of freedom**

where:
- `d̄` = mean of all differences = X̄ − Ȳ
- `Sd` = standard deviation of the differences
- `µD` = hypothesized difference (usually 0)

```python
# Paired t-test (dependent samples)
t_stat, p_val = stats.ttest_rel(before, after)
# Returns two-sided p-value
```

> 🔑 **Key identifier**: "same group before and after" = **paired**; "two separate groups" = **independent**

**Worked Example (Shear Strength)**: d̄=0.2736, Sd=0.1356, n=9
- t_calc = 0.2736/(0.1356/√9) = **6.05**
- t_critical(α=0.025, df=8) = ±2.306
- 6.05 > 2.306 → **Reject H₀** (methods yield different results)

---

### Two-Sample Proportion Z-Test

Comparing proportions from two independent populations:

**Pooled proportion** (when H₀: P₁ = P₂):
`p̄ = (n₁p̂₁ + n₂p̂₂) / (n₁ + n₂)`

**Test statistic**:
`Z = (p̂₁ − p̂₂) / √[p̄(1−p̄)(1/n₁ + 1/n₂)]`

**CI for P₁ − P₂**:
`(p̂₁ − p̂₂) ± Zα/2 × √(p̂₁q̂₁/n₁ + p̂₂q̂₂/n₂)`

```python
import math
def two_samp_proportions(p1, p2, n1, n2):
    p_pool = (n1*p1 + n2*p2) / (n1 + n2)
    var = p_pool * (1 - p_pool) * (1/n1 + 1/n2)
    z = (p1 - p2) / math.sqrt(var)
    p = stats.norm.cdf(z) if z < 0 else 1 - stats.norm.cdf(z)
    return z, 2*p  # two-sided

# St. John's Wort: p1=27/100=0.27, p2=9/100=0.09, n1=n2=100
z, p = two_samp_proportions(0.27, 0.09, 100, 100)  # z=1.33, p=0.17 → accept H₀
```

**Worked Example (Medicine)**: p̂₁=0.27, p̂₂=0.09, n₁=n₂=100, p̄=0.18
- Z = 1.35, p-value = 0.177 > 0.05 → **Do NOT reject H₀** (no evidence medicine works)

---

## 📌 LECTURE 22 — F-Test for Two Variances & When to Use Z vs t

### F-Test: Comparing Two Population Variances

**Hypotheses**:
| H₀ | H₁ | Test |
|----|-----|------|
| σ₁² = σ₂² | σ₁² ≠ σ₂² | Two-tailed F-test |
| σ₁² ≤ σ₂² | σ₁² > σ₂² | Right-tailed F-test |

**Test Statistic**:
`F = S₁² / S₂²` where **S₁² is the LARGER variance (always in numerator!)**

- **Numerator df** = n₁ − 1
- **Denominator df** = n₂ − 1
- F distribution is **right-skewed** (not normal, always positive)
- Two populations must be **independent** and **normally distributed**

> 🔑 **Rule**: Put LARGER sample variance in numerator → only need upper critical value → simplifies two-tailed test

**CI for σ₁/σ₂**: If the interval contains **1**, we CANNOT reject H₀ (σ₁ = σ₂ is plausible)

```python
import scipy.stats as stats

# F critical value (upper, α=0.05, df1=15, df2=10)
stats.f.ppf(1 - 0.05, dfn=15, dfd=10)     # → 2.85 (upper limit)

# F critical value (lower, α=0.05, df1=15, df2=10)
stats.f.ppf(0.05, dfn=15, dfd=10)          # → 0.39 (lower limit)

# Direct F-test from data
F = np.var(X, ddof=1) / np.var(Y, ddof=1)
dfn = len(X) - 1
dfd = len(Y) - 1
p_val = stats.f.cdf(F, dfn, dfd)
# If p_val < α → reject H₀ (variances are different)
```

**Worked Example (Grinding)**: n₁=11 (S₁=5.1), n₂=16 (S₂=4.0); α=0.10
- df₁=10, df₂=15 (numerator/denominator)
- F_upper = 2.85, F_lower = 0.39; CI for σ₁/σ₂ = (0.678, 1.887)
- **1 is inside the interval → Do NOT reject H₀** (variances are equal)

---

### When to Use Z Test vs t-Test (Decision Table)

| Population σ Known? | Sample Size | Use |
|---------------------|------------|-----|
| **Yes** | Any n | **Z-test** |
| No | n < 30 | **t-test** |
| No | **n ≥ 30** | **Z-test (acceptable)** — t approaches Z |

> 🔑 Most software only provides t-test because for large n, t ≈ Z. If σ is known, always Z.

---

### Determining Sample Size (for Hypothesis Testing)

**Sample size formula** — balancing both α and β:

`n = (Zα + Zβ)² × σ² / (µ₀ − µₐ)²`

For **two-tailed**, replace Zα with **Zα/2**.

```python
import scipy.stats as stats
import math

def samplesize(alpha, beta, mu1, mu2, sigma):
    z1 = -1 * stats.norm.ppf(alpha)    # Z_alpha
    z2 = -1 * stats.norm.ppf(beta)     # Z_beta
    n = ((z1 + z2) ** 2 * sigma**2) / (mu1 - mu2)**2
    return math.ceil(n)

# α=0.05, β=0.10, µ₀=12, µₐ=12.75, σ=3.2
n = samplesize(0.05, 0.10, 12, 12.75, 3.2)  # → 156
```

---

## 📌 LECTURE 23 — Sample Size Formulas & ANOVA Introduction

### Sample Size for Estimating µ

`n = Z² × σ² / E²`

where E = permissible error (margin of error).

> If σ unknown: **σ ≈ Range/4** (approximation rule)

**Examples**:
| E | σ | Z (conf. level) | n |
|---|---|-----------------|---|
| 1 | 4 | 1.645 (90%) | **44** (= 43.30 rounded up) |
| 2 | Range/4 = 25/4 = 6.25 | 1.96 (95%) | **38** |

### Sample Size for Estimating P

`n = Z² × P×Q / E²`

Where P = population proportion, Q = 1−P, E = permissible error.

> 🔑 **If P is unknown, use P = 0.5 → gives MAXIMUM (most conservative) sample size**

**Example**: E=0.03, 98% CI (Z=2.33), P=0.40, Q=0.60:
- n = (2.33)² × (0.4×0.6) / (0.03)² = **1448**

---

### Why ANOVA Instead of Multiple t-Tests?

Each t-test has Type I error = α. With k populations, you need C(k,2) pairwise comparisons.

**Familywise Error Rate**:
`Overall α = 1 − (1−α)^comparisons`

**Example**: 5 groups, α=0.05 → need C(5,2)=10 comparisons
- Overall error = 1 − (0.95)^10 = **1 − 0.599 = 0.401 = 40%!**

> 🔑 → Must use **ANOVA** when comparing > 2 means (controls error at α)

**Chi-Square vs ANOVA**:
| Test | Use | Analog |
|------|-----|--------|
| Z proportion (1-sample) | 1 proportion | — |
| Two-sample Z proportion | 2 proportions | — |
| **Chi-Square** | **>2 proportions** | Generalization of Z |
| t-test | 2 means | — |
| **ANOVA (F-test)** | **>2 means** | Generalization of t-test |

---

### ANOVA Intuition (Teaching Methodology Example)

**Setup**: 3 groups (Blackboard, Case Study, PowerPoint), n=3 each, overall X̄=3

| Group | Data | Mean |
|-------|------|------|
| Group 1 | 4, 3, 2 | 3 |
| Group 2 | 2, 4, 6 | 4 |
| Group 3 | 2, 1, 3 | 2 |

**Partition the variance**:
- **SST** (Total) = Σ(Xᵢ − X̄_grand)² = **18** (df = 9−1 = 8)
- **SSB** (Between/Treatment) = Σnⱼ(X̄ⱼ − X̄_grand)² = 3(3−3)² + 3(4−3)² + 3(2−3)² = **6** (df = 3−1 = 2)
- **SSE** (Error/Within) = SST − SSB = 18 − 6 = **12** (df = 8−2 = 6)
- **MSB** = SSB/df = 6/2 = **3**
- **MSE** = SSE/df = 12/6 = **2**
- **F_calc** = MSB/MSE = 3/2 = **1.5**
- F_table(α=0.05, df₁=2, df₂=6) = **5.14**
- 1.5 < 5.14 → **Do NOT reject H₀** (teaching method doesn't affect performance)

---

## 📌 LECTURE 24 — ANOVA II (Theory & Python)

### ANOVA Assumptions

1. Each group's response variable is **normally distributed**
2. All groups have the **same variance** (σ² is constant — homoscedasticity)
3. Observations are **independent**

### Key ANOVA Terminology

| Term | Symbol | Meaning |
|------|--------|---------|
| Factor | — | Independent variable (e.g., teaching method) |
| Levels | — | Groups/categories within factor (e.g., blackboard, PPT) |
| k | — | Number of groups/treatments |
| nᵢ | — | Sample size of group i |
| nT | — | Total observations = Σnᵢ |
| yᵢⱼ | — | j-th observation in i-th treatment |
| ȳᵢ. | — | Mean of i-th treatment |
| ȳ.. | — | Grand mean (overall mean) |

### ANOVA Table Structure

| Source | SS | df | MS | F |
|--------|----|----|----|---|
| Treatment (Between) | SSTR | k−1 | MSTR = SSTR/(k−1) | MSTR/MSE |
| Error (Within) | SSE | nT−k | MSE = SSE/(nT−k) | — |
| Total | SST | nT−1 | — | — |

**Key formulas**:
- `SSTR = Σⱼ nⱼ(ȳⱼ − ȳ..)²` (treatment effect)
- `SSE = ΣΣ(yᵢⱼ − ȳⱼ)²` = Σ(nⱼ−1)Sⱼ²
- `SST = SSTR + SSE`
- `MSE = SSE/(nT−k)` — estimates σ² ALWAYS

**Shortcut formulas** (for exam calculations!):
- `SST = Σᵢ Σⱼ yᵢⱼ² − y..²/N` (subtract correction factor)
- `SSTr = (1/n)Σᵢ yᵢ.² − y..²/N` (equal sample sizes)
- `SSE = SST − SSTr`

### H₀ and Test in ANOVA

- **H₀**: µ₁ = µ₂ = ... = µₖ (all means equal)
- **Hₐ**: Not all population means are equal (**at least 2 differ**)
- **Reject H₀** if F_calc > F_table OR if p-value ≤ α
- F-distribution is **right-skewed**: large F → reject H₀

```python
from scipy import stats
import statsmodels.api as sm
from statsmodels.formula.api import ols
import pandas as pd

# Quick ANOVA (arrays)
a = [4, 3, 2]; b = [2, 4, 6]; c = [2, 1, 3]
F, p = stats.f_oneway(a, b, c)   # F=1.5, p=0.295 → accept H₀

# Full ANOVA table using statsmodels
data = pd.read_excel('oneway.xlsx')
data_new = pd.melt(data.reset_index(), id_vars=['index'],
                   value_vars=['Method1', 'Method2', 'Method3'])
data_new.columns = ['index', 'treatment', 'value']

model = ols('value ~ C(treatment)', data=data_new).fit()
anova_table = sm.stats.anova_lm(model, typ=1)
print(anova_table)
```

**F interpretation**:
| F value | Meaning |
|---------|---------|
| F > 1 | Treatment variance > Error variance → treatment has effect |
| F ≈ 1 | Treatment and error variances equal → no treatment effect |
| F < 1 | Error dominates → no treatment effect |

---

## 📌 LECTURE 25 — Post Hoc Analysis (Tukey & LSD)

### Purpose of Post Hoc Analysis
When ANOVA **rejects H₀**, we know means are NOT all equal but **ANOVA doesn't say WHICH pairs differ**.
Post hoc tests identify the specific pairs.

---

### Fisher's LSD (Least Significant Difference) Method

**LSD formula** (equal sample sizes):
`LSD = tα/2(nT−k) × √(2MSE/n)`

**Decision rule**: If |ȳᵢ − ȳⱼ| > LSD → µᵢ ≠ µⱼ (significantly different)

For **unequal sample sizes**:
`LSD = tα/2(nT−k) × √(MSE × (1/nᵢ + 1/nⱼ))`

```python
import math
from scipy import stats

t_val = -1 * stats.t.ppf(0.025, 20)   # absolute t-value, df=nT-k=20
n = 6
MSE = 6.51
LSD = t_val * math.sqrt(2 * MSE / n)  # → 3.07

# Then compare |mean_i - mean_j| > LSD for each pair
```

---

### Tukey-Kramer Test (HSD — Honestly Significant Difference)

**Critical Range formula**:
`CR = Qα(c, nT−c) × √(MSE/2 × (1/nⱼ + 1/nⱼ'))`

For equal sample sizes: `CR = Qα × √(MSE/n)`

where **Q** is from the **Studentized Range distribution table** with c groups and nT−c df.

**Decision rule**: If |ȳᵢ − ȳⱼ| > CR → µᵢ ≠ µⱼ (significantly different)

```python
from statsmodels.stats.multicomp import pairwise_tukeyhsd
from statsmodels.stats.multicomp import MultiComparison

# MultiComparison takes (values, group_labels)
mc = MultiComparison(data_r1['value'], data_r1['treatment'])
result = mc.tukeyhsd(alpha=0.05)
print(result.summary())
# "reject=False" means that pair's means are NOT significantly different (equal)
# "reject=True" means that pair IS significantly different
```

> 🔑 **Tukey output**: `reject=False` → means ARE equal | `reject=True` → means are DIFFERENT

---

### ANOVA Worked Example (Paper Tensile Strength)

4 hardwood concentrations: 5%, 10%, 15%, 20%; n=6 per group; α=0.01

| Group | Mean (ȳⱼ) |
|-------|----------|
| 5% | 10.00 |
| 10% | 15.67 |
| 15% | 17.00 |
| 20% | 21.17 |

- SST = 512.96, SSTR = 382.79, SSE = 130.17
- df_treatment = 4−1 = **3**; df_error = 24−4 = **20**; df_total = 23
- MSTR = 382.79/3 = **127.6**; MSE = 130.17/20 = **6.51**
- F_calc = 127.6/6.51 = **19.6**
- F_table(α=0.01, df₁=3, df₂=20) = 4.94
- 19.6 >> 4.94 → **Reject H₀** (hardwood % affects tensile strength)

**Post Hoc (LSD, α=0.05)**:
- LSD = 2.086 × √(2×6.51/6) = **3.07**
- All pairs differ EXCEPT **10% vs 15%** (|15.67−17.00|=1.33 < 3.07)
- → 10% and 15% hardwood produce **same** tensile strength

**Tukey-Kramer** gives same result:
- Q table value (α=0.05, c=4, df=20) = **3.96**
- CR = 3.96 × √(6.51/6) = **4.12**
- Only pair within CR: 10% vs 15% → **µ₂ = µ₃**

---

### Complete ANOVA + Post Hoc Workflow

```
1. Plot data (boxplot) to visualize group differences
2. Run ANOVA: stats.f_oneway() or anova_lm()
3. Check F & p-value: if p ≤ α → reject H₀
4. If rejected → run Tukey HSD: MultiComparison().tukeyhsd()
5. Interpret: reject=False pairs have equal means
```

---

## 🎯 Week 5 Assignment — Key Questions & Answers

| Q | Topic | Answer | Working |
|---|-------|--------|---------|
| Q1 | H₀:µ=20, H₁:µ>20, α=0.05 → rejection region | **(b) Z > 1.645** | Right-tailed: Hₐ has >, critical value is +1.645 |
| Q2 | Welch's df: S₁²=16,n₁=10, S₂²=25,n₂=15 | **(c) 22** | Substitute in Welch's df formula |
| Q3 | n=36, X̄=34.6, σ=12; H₀:µ≤30, H₁:µ>30, 95% CI | **(b) be rejected** | Z=(34.6−30)/(12/√36)=2.3 > 1.645 → reject |
| Q4 | σ unknown, n=45, normal pop | **(b) Z-test using s** | n>30 → Z acceptable; σ unknown → use s |
| Q5 | Why ANOVA over multiple t-tests | **(c) Controls family-wise Type I error** | Multiple t-tests inflate α up to 40% |
| Q6 | Treatment means far apart relative to error → F | **(c) Large** | F = MSTR/MSE; large treatment effect → large F |
| Q7 | Tukey HSD CR=3.5; means are 18 and 14 | **(b) Significantly different** | |18−14|=4 > 3.5 → reject, means differ |
| Q8 | SS_Tr=240, df_Tr=? (4 treatments), MST=? | **(b) 80** | df_Tr=4−1=3; MSTR=240/3=80 |
| Q9 | Total df for Q8 (Error df=12, 4 treatments) | **(c) 15** | df_total = df_Tr + df_Error = 3+12 = 15 |
| Q10 | F-statistic for Q8 | **(c) 8** | MSE=120/12=10; F=80/10=8 |

---

## 💡 Quick-Fire Review Checklist
- [ ] Three two-sample t cases: σ known (Z), σ unknown+equal (pooled t, df=n₁+n₂−2), σ unknown+unequal (Welch's t, df from formula)
- [ ] Welch's df formula: numerator=(S₁²/n₁+S₂²/n₂)², denominator=sum of squares of each term/(n−1)
- [ ] Paired t-test: dᵢ=Xᵢ−Yᵢ, t=d̄/(Sd/√n), df=n−1; use `ttest_rel()`
- [ ] Two-sample proportion: pooled p̄=(n₁p̂₁+n₂p̂₂)/(n₁+n₂); Z=(p̂₁−p̂₂)/√[p̄q̄(1/n₁+1/n₂)]
- [ ] F-test for variances: F=S₁²/S₂² (larger variance in numerator!); F-distribution is right-skewed
- [ ] F-test: df₁=n₁−1 (numerator), df₂=n₂−1 (denominator)
- [ ] If CI for σ₁/σ₂ contains 1 → do NOT reject H₀ (variances can be equal)
- [ ] When to use Z vs t: σ known → always Z; σ unknown + n<30 → t; σ unknown + n≥30 → Z acceptable
- [ ] Sample size for µ: n = Z²σ²/E²; σ unknown → use Range/4
- [ ] Sample size for P: n = Z²PQ/E²; P unknown → use P=0.5 (max n)
- [ ] ANOVA: use when comparing >2 means; avoids inflated Type I error
- [ ] SST = SSTR + SSE (additive partition!); df_total = k−1 + (nT−k) = nT−1
- [ ] MSTR = SSTR/(k−1); MSE = SSE/(nT−k); F = MSTR/MSE
- [ ] Large F → treatment effect exists → reject H₀
- [ ] ANOVA assumptions: normality, equal variances, independence
- [ ] H₀ in ANOVA: µ₁=µ₂=...=µₖ; Hₐ: "not all means are equal" (≥2 differ)
- [ ] Python ANOVA: `stats.f_oneway(*groups)` or `sm.stats.anova_lm(model, typ=1)`
- [ ] Use `pd.melt()` to convert wide format → long format for OLS ANOVA
- [ ] Post hoc needed ONLY when H₀ is REJECTED in ANOVA
- [ ] LSD: `|ȳᵢ−ȳⱼ| > t_(α/2,nT−k) × √(2MSE/n)` → means differ
- [ ] Tukey-Kramer: `|ȳᵢ−ȳⱼ| > Q_(α,c,nT−c) × √(MSE/n)` → means differ  
- [ ] Python Tukey: `MultiComparison(values, groups).tukeyhsd(alpha=0.05)`; `reject=False` = means equal
- [ ] Shortcut: SST = Σyᵢⱼ² − y..²/N; SSTr = (1/n)Σyᵢ.² − y..²/N; SSE = SST − SSTr
