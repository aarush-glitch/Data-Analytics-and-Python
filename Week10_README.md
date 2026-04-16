# 📊 Week 10 — Data Analytics with Python (NPTEL)
### Lectures 46–50 | Topics: Chi-Square Tests (Independence & Goodness of Fit), Cluster Analysis Intro

---

## 📌 LECTURE 46 — Chi-Square Test of Independence I

### Why Chi-Square?

You cannot use Z-test, T-test, ANOVA, or regression for **nominal/ordinal (categorical) data**.

| Data type | Test |
|-----------|------|
| Compare means (2 populations) | 2-sample T-test / Z-test |
| Compare means (>2 populations) | **ANOVA** |
| Compare proportions (2 populations) | 2-sample Z proportion test |
| Compare proportions (>2 populations) | **Chi-Square test** |

> 🔑 Chi-square is a **generalized** version of proportion tests — just like ANOVA generalizes T-tests!
> 🔑 Chi-square has **2 applications**: (1) Test of Independence; (2) Goodness of Fit

---

### Chi-Square Test of Independence

**When to use**: Two categorical variables; test if they are **related (dependent) or unrelated (independent)**

**Setup**: Data is arranged in a **contingency table** (also called a cross-classification table)

**H₀**: Variable A and Variable B are **independent**
**H₁**: Variable A and Variable B are **NOT independent** (they are related)

---

### Key Formula: Expected Frequency

`eᵢⱼ = (Row i total × Column j total) / Grand total N`

| Symbol | Meaning |
|--------|---------|
| eᵢⱼ | Expected frequency for cell (row i, column j) |
| n_i | Total for row i |
| n_j | Total for column j |
| N | Grand total of ALL observations |

**Derivation**: Under independence, P(A∩F) = P(A)×P(F); multiply by N to convert probability to frequency.

---

### Chi-Square Test Statistic

`χ² = ΣΣ [(fₒ − fₑ)² / fₑ]`

| Symbol | Meaning |
|--------|---------|
| fₒ | Observed frequency (given in data) |
| fₑ | Expected frequency (calculated) |

**Degrees of freedom**: `df = (r − 1) × (c − 1)`
where r = number of rows, c = number of columns

**Key assumption**: All expected frequencies must be **≥ 5**. If not, merge/collapse categories.

**Decision rule**: If χ²_calculated > χ²_critical → **Reject H₀** (variables are NOT independent)

---

### Finding Critical Value (Python)

```python
from scipy import stats
# For α = 0.01 (right tail = 1%), df = 6
chi2_critical = stats.chi2.ppf(0.99, 6)    # = 16.811
```

---

### Worked Example 1: Gasoline & Income

- Rows: Income levels (4 levels → r=4)
- Columns: Gasoline type (3 types → c=3)
- df = (4−1)×(3−1) = **6**
- α = 1%; χ²_critical = 16.812
- χ²_calculated = **70.75** → way above critical → **Reject H₀**
- **Conclusion**: Income and type of gasoline are **NOT independent** — higher income → higher quality fuel

**Expected frequency example**: Cell (row1, col1):
`eᵢⱼ = (107 × 238) / 385 = 66.15`

---

### Worked Example 2: Hand Preference & Gender (2×2 table)

- 300 students; 120 female, 180 male; 36 left-handed, 264 right-handed
- df = (2−1)×(2−1) = **1**
- χ²_calculated = **0.7576** < χ²_critical (3.841 @ α=5%)
- **Do NOT reject H₀** → Gender and hand preference are **independent**

**2×2 Expected frequency**:
- e(left-hand, female) = (36 × 120) / 300 = 14.4
- e(left-hand, male) = (36 × 180) / 300 = 21.6
- e(right-hand, female) = (264 × 120) / 300 = 105.6
- e(right-hand, male) = (264 × 180) / 300 = 158.4

---

### Worked Example 3: Comparing 3 Proportions

**Problem**: 500 respondents; % objecting to data sharing: insurance (410), pharmacy (295), researcher (335)
- df = (2−1)×(3−1) = **2**
- χ²_calculated = **64.12** >> χ²_critical (5.991) → **Reject H₀**
- **Conclusion**: Proportions are NOT equal — people object differently depending on who accesses data

---

## 📌 LECTURE 47 — Chi-Square Test of Independence II + Goodness of Fit (Poisson)

### Python: Forming Contingency Table

```python
import pandas as pd
import numpy as np
from scipy.stats import chi2_contingency

# Read data from file
acad = pd.read_excel('acad.xlsx')

# Step 1: Form contingency table from raw data
obs = pd.pivot_table(acad, values='RespondentNumber',
                     index='g',   # gender in rows
                     columns='sm', # student motivation in columns
                     aggfunc=len)  # count occurrences

# Step 2: Chi-square test
chi2, p, dof, expected = chi2_contingency(obs)

print(f'Chi2 = {chi2:.3f}')        # 2.364
print(f'p-value = {p:.3f}')        # 0.306
print(f'Degrees of freedom = {dof}')  # 2
print(f'Expected frequencies:\n{expected}')
```

**Result**: p = 0.306 > 0.05 → Accept H₀ → Gender and student motivation are **independent**

> 🔑 `pd.pivot_table(..., aggfunc=len)` — fastest way to build contingency table from raw dataset!
> 🔑 `chi2_contingency(obs)` returns: chi2 statistic, p-value, degrees of freedom, expected frequencies

---

### Chi-Square Goodness of Fit (What Distribution Does Data Follow?)

**Purpose**: Test whether observed data follows a specified probability distribution

**IMPORTANT difference from independence test**:
| | Independence Test | Goodness of Fit |
|--|---|---|
| H₀ | Variables ARE independent | Data **DOES** follow [distribution] |
| H₁ | Variables are NOT independent | Data does **NOT** follow [distribution] |
| **"NOT" appears in** | H₁ (Alternative) | **H₁ (Alternative)** ← same! |

> 🔑 In goodness-of-fit test: H₀ states the claimed distribution (NOT traditional — NOT appears in H₁!)
> 🔑 **Minimum expected frequency ≥ 5** — SAME rule applies; merge intervals if not met!

---

### Degrees of Freedom for Goodness of Fit

`df = k − 1 − p`

| Distribution | Parameters (p) | df |
|-------------|---------------|----|
| **Uniform** | 0 (none estimated) | k − 1 |
| **Poisson** | 1 (μ = mean) | k − 2 |
| **Normal** | 2 (μ and σ) | k − 3 |

where k = number of categories/intervals after merging

---

### Worked Example: Goodness of Fit for Poisson Distribution

**Problem**: Parking garage — 100 one-minute intervals; test if arrivals ~ Poisson

**Step 1**: Find Poisson parameter μ
`μ = Σ(f × n) / Σf = 600 / 100 = 6`

**Step 2**: Calculate expected frequencies using Poisson PMF:
`P(X = x) = (e^(−μ) × μˣ) / x!`
Then multiply by n = 100

**Step 3**: Merge cells where expected frequency < 5:
- Merge X = 0, 1, 2 together → expected = 6.20
- Merge X = 10, 11, 12 together → expected = 8.39
- After merging: **k = 9 intervals**

**Step 4**: df = k − 1 − p = 9 − 1 − 1 = **7** (Poisson has 1 parameter)

**Step 5**: χ²_calculated = **3.27**, p-value = **0.911** > 0.05 → Accept H₀

**Conclusion**: Arrivals follow Poisson distribution ✓

```python
from scipy.stats import chi2, poisson
import pandas as pd

# Expected frequencies
E_freq = []
for i in range(len(observed_freq)):
    E_freq.append(100 * poisson.pmf(i, mu))

# Chi-square goodness of fit
from scipy.stats import chisquare
chi2_stat, p_val = chisquare(observed_freq, f_exp=expected_freq)
# χ² = 3.27, p = 0.911 → Accept H₀ → Poisson fits!
```

---

## 📌 LECTURE 48 — Chi-Square Goodness of Fit (Uniform & Normal)

### Goodness of Fit for Uniform Distribution

**Problem**: Monthly milk sales (12 months) — are sales uniformly distributed?

**H₀**: Monthly milk sales are uniformly distributed
**H₁**: Monthly milk sales are NOT uniformly distributed

**Expected frequency** (uniform = equal for all):
`E = Total sum / k = 18,447 / 12 = 1,537.25` (same for every month)

**df = k − 1 − 0 = 12 − 1 = 11** (uniform has no parameters)

At α = 0.01, df = 11: χ²_critical = **24.725**
χ²_calculated = **74.37** >> 24.725 → **Reject H₀**

**Conclusion**: Sales are NOT uniformly distributed — some months sell significantly more/less

**Python**:
```python
chi2_critical = stats.chi2.ppf(0.99, 11)   # α=0.01, df=11 → 24.72
chi2_stat, p = chisquare(x, f_exp=expected_freq)
# p = 1.7×10⁻¹¹ << 0.05 → Reject H₀
```

> 🔑 **If data follows uniform distribution → the data is RANDOM** (useful for testing random number generators!)

---

### Goodness of Fit for Normal Distribution

**Problem**: Sales figures of 30 salespeople; mean = 71, SD = 18.23. Do they follow Normal?

**Key Steps**:

1. **Divide by ≥5 expected per interval**: 30 / 6 = 5 → use **6 intervals** (so each has minimum 5)

2. **Find interval boundaries** using equal probability areas (1/6 = 0.1667 each area):
```python
from scipy.stats import norm
intervals = [norm.ppf(j/6, loc=mean, scale=std) for j in range(1, 6)]
# Gives: 53.36, 63.14, 71, 78.85, 88.63
```

3. **Count observed** in each interval (manually from data)

4. **Expected = 5 for each interval** (30 ÷ 6 = 5)

5. **df = k − 1 − p = 6 − 1 − 2 = 3** (Normal has 2 parameters: μ and σ)

6. χ²_critical at α = 5%, df=3 = **7.815**; χ²_calculated = **1.6** → **Accept H₀**

**Conclusion**: Sales data follows Normal distribution ✓

```python
chi2_stat, p = chisquare(observed_freq, f_exp=expected_freq)
# p = 0.90 >> 0.05 → Accept H₀ → Normal distribution fits!
```

---

## 📌 LECTURE 49 — Cluster Analysis: Introduction I

### What is Cluster Analysis?

**Cluster Analysis** = finding groups (clusters) in data where:
- Objects **within** a cluster are as SIMILAR as possible
- Objects **across** clusters are as DISSIMILAR as possible

**Unsupervised learning** — no prior knowledge of groups needed; no labels, no Y variable

---

### Cluster vs. Discriminant Analysis

| | Cluster Analysis | Discriminant Analysis |
|--|---|---|
| **Type** | **Unsupervised** | Supervised |
| **Groups known?** | No — discovers groups | Yes — groups defined in advance |
| **Labels needed?** | No | Yes (Y = group membership) |
| **Purpose** | Explore structure in data | Assign new objects to known groups |
| **Analogy** | Finding unknown classes | Classifying into known classes |

---

### Types of Data for Clustering

**1. Interval/Ratio Data**: Continuous variables (height, weight, temperature) → use distance measures

**2. Nominal Data**: Categorical variables → different algorithms needed

**Practical issue**: Clustering results CHANGE with units!

| Units | Clustering result |
|-------|-------------------|
| kg and cm | Clear two clusters (children vs adults) |
| pounds and inches | Different, less clear clusters |

→ **Standardize to remove unit dependency!**

---

### Standardization (Z-Score for Clustering)

**Standard deviation**: `sf = √[Σ(xif − mf)² / (n−1)]` — sensitive to outliers

**Mean Absolute Deviation (MAD)** (preferred for clustering):
`sf = (1/n) × Σ|xif − mf|` — more robust to outliers

**Z-score (standardized value)**:
`zif = (xif − mf) / sf`

**After standardization**: mean = 0; absolute deviation = 1; **unitless**

> ⚠️ Standardization is NOT always beneficial!
> - Sometimes clusters disappear after standardization (e.g., age-height example → 4 corners of a square)
> - If variables are measured in same units → don't standardize
> - Subject matter knowledge determines if standardization is appropriate
> - Outliers → use MAD instead of standard deviation

---

## 📌 LECTURE 50 — Cluster Analysis: Distances & Dissimilarity Measures

### Why Standardization Can Mislead

**Example**: 4 persons, age (years) and height (cm):
- Before standardization: A, B and C, D form clear clusters
- After standardization → 4 points at corners of a square → NO clear cluster!

**Rule**: Variables with absolute meaning (or same units) should NOT be standardized.

---

### Distance Measures

#### 1. Euclidean Distance (most popular)

`d(i, j) = √[Σ(xᵢf − xⱼf)²]`

= `√[(xᵢ₁−xⱼ₁)² + (xᵢ₂−xⱼ₂)² + … ]`

**Analogy**: Direct flight from A to B (bird's path) — shortest straight-line distance
**Corresponds to**: Hypotenuse of a right triangle (Pythagoras theorem)

**Numerical example**: x₁=(1,2), x₂=(3,5):
`d = √[(3−1)² + (5−2)²] = √[4+9] = √13 = 3.61`

#### 2. Manhattan Distance (City Block / Rectilinear)

`d(i, j) = Σ|xᵢf − xⱼf|`

= `|xᵢ₁−xⱼ₁| + |xᵢ₂−xⱼ₂| + …`

**Analogy**: Car in a city grid — must follow street blocks (no diagonal shortcuts!)
**Manhattan > Euclidean** always (longer path)

**Numerical example**: x₁=(1,2), x₂=(3,5):
`d = |3−1| + |5−2| = 2 + 3 = 5`

#### 3. Minkowski Distance (Generalization)

`d(i, j) = [Σ|xᵢf − xⱼf|ᵖ]^(1/p)`

| p value | Reduces to |
|---------|-----------|
| p = 1 | Manhattan distance |
| p = 2 | **Euclidean distance** |
| p → ∞ | Chebyshev distance |

---

### Properties of Valid Distance Function

| Property | Formula | Meaning |
|----------|---------|---------|
| D1: Non-negativity | d(i,j) ≥ 0 | Distances are never negative |
| D2: Self-distance | d(i,i) = 0 | Distance to itself is 0 |
| D3: Symmetry | d(i,j) = d(j,i) | A→B same distance as B→A |
| D4: Triangle inequality | d(i,j) ≤ d(i,h) + d(h,j) | Direct path ≤ detour |

> 🔑 Distance matrix is always **symmetric** — upper triangle = lower triangle
> 🔑 All diagonal elements = 0
> 🔑 It's sufficient to display only the **lower triangle**

---

### n×n Distance Matrix

For 8 objects (A–H), compute pairwise Euclidean distances:
- d(B, E): B=(49kg, 156cm), E=(85kg, 178cm): `√[(85−49)² + (178−156)²] = √[1296+484] = 42.2`
- d(A, A) = 0 (diagonal)
- d(A, C) = 2.0 (closest pair → likely same cluster!)

**Reading the matrix**: Find intersection of row and column → that's the distance.
e.g., d(B,E) = row B, col E = row E, col B = 42.2

---

### Variable Selection for Clustering

> 🔑 **Irrelevant variables DESTROY clustering structure!**

- Non-informative variables (e.g., phone numbers) add random noise to distances
- **Solution**: Give zero weight (i.e., remove) to non-informative variables
- Variable selection may involve trial and error + subject matter knowledge
- Cluster analysis is an **exploratory technique**

---

## 🎯 Week 10 Assignment — Key Questions & Answers

| Q | Question | Answer | Key Concept |
|---|----------|--------|-------------|
| Q1 | Both variables categorical → which test? | **Chi-square test of independence** | Independence test |
| Q2 | df for r=4, c=3 | **(4−1)×(3−1) = 6** | df = (r−1)(c−1) |
| Q3 | What if expected freq < 5? | **Combine/merge categories** | Assumption: fe ≥ 5 |
| Q4 | If independent, observed vs expected? | **Observed = Expected** | No deviation under H₀ |
| Q5 | Chi-square assumption violated when? | **Expected frequency < 5** | Minimum fe condition |
| Q6 | Why standardize for clustering? | **Variables on different scales** | Scale distorts distances |
| Q7 | Clustering affected by extreme values? | **K-means clustering** | Means shift with outliers |
| Q8 | What causes poor clustering? | **Poor variable selection** | Irrelevant vars add noise |
| Q9 | Example of cluster analysis use? | **Grouping customers by behavior** | Market segmentation |
| Q10 | After standardization, properties? | **Mean = 0, deviation = 1** | Z-score properties |

---

## 💡 Quick-Fire Review Checklist

**Chi-Square Test of Independence:**
- [ ] Use for: two categorical variables; test if they are independent or not
- [ ] H₀: variables are independent; H₁: variables are NOT independent
- [ ] Expected frequency: `eᵢⱼ = (row i total × col j total) / N`
- [ ] Test stat: `χ² = ΣΣ (fₒ−fₑ)²/fₑ`
- [ ] df = **(r−1)(c−1)**; all expected freq must be **≥ 5** (else merge)
- [ ] Python: `chi2, p, dof, expected = chi2_contingency(obs)`
- [ ] Build contingency table: `pd.pivot_table(df, index='A', columns='B', aggfunc=len)`

**Chi-Square Goodness of Fit:**
- [ ] Use for: test if data follows a specific distribution (Poisson, Normal, Uniform)
- [ ] H₀: data FOLLOWS [dist]; H₁: data does NOT follow [dist] ← "NOT" in H₁! (opposite of usual)
- [ ] df = k − 1 − p; p=0 (uniform), p=1 (Poisson), p=2 (Normal)
- [ ] Merge cells until all expected freq ≥ 5; k = number of intervals AFTER merging
- [ ] Poisson μ: `μ = Σ(f×n)/Σf`; expected freq = n × P(X=x)
- [ ] Uniform expected freq: `total / k` (equal for all)
- [ ] Normal: divide into 6 intervals using `norm.ppf(j/6, mean, std)` so each has min 5 expected
- [ ] Python: `chi2_stat, p = chisquare(observed, f_exp=expected)`
- [ ] All random numbers FOLLOW uniform distribution (use goodness of fit to verify randomness!)

**Cluster Analysis:**
- [ ] Unsupervised: no Y variable; discovers unknown groups in data
- [ ] vs Discriminant Analysis: discriminant = supervised; groups known in advance
- [ ] Within-cluster similarity HIGH; between-cluster dissimilarity HIGH
- [ ] Units matter → different units = different clusters → standardize!
- [ ] Standardization: mean = 0, deviation = 1; use MAD (not SD) for robustness to outliers
- [ ] Standardization can sometimes DESTROY clustering structure → use judgment!

**Distance Measures:**
- [ ] **Euclidean**: `√[Σ(xᵢf−xⱼf)²]` — bird's flight; default; p=2 in Minkowski
- [ ] **Manhattan**: `Σ|xᵢf−xⱼf|` — city blocks; always ≥ Euclidean; p=1 in Minkowski
- [ ] **Minkowski**: `[Σ|xᵢf−xⱼf|ᵖ]^(1/p)` — generalization of both
- [ ] Distance matrix: n×n; diagonal = 0; symmetric (upper = lower triangle)
- [ ] Properties: d(i,j)≥0; d(i,i)=0; d(i,j)=d(j,i); d(i,j)≤d(i,h)+d(h,j)
- [ ] Euclidean < Manhattan (always) — shorter to fly than to drive grid streets
- [ ] Irrelevant variables → always REMOVE (give zero weight) before clustering
