# 📊 Week 1 — Data Analytics with Python (NPTEL)
### Lectures 1–5 | Topics: Intro to DA, Python Fundamentals, Central Tendency & Dispersion

---

## 📌 LECTURE 1 — Introduction to Data Analytics

### Core Definitions (MCQ-Critical)

| Term | Definition |
|------|-----------|
| **Variable** | A characteristic of any entity being studied that is capable of taking on different values (e.g., X) |
| **Measurement** | Standard process used to assign numbers to attributes of a variable |
| **Data** | Recorded measurements (e.g., X=5; the number 5 is the data) |
| **Data Analysis** | Post-mortem analysis — studying *what happened* in the **past** |
| **Data Analytics** | Studying *what will happen* in the **future** (prediction) |
| **Data Product** | An algorithm/model built with data (e.g., recommendation engines, Google Maps, driverless cars) |

> **Key Distinction**: Data Analysis = past. Data Analytics = future.

---

### Types of Data Analytics (In Order of Workflow + Difficulty)

| Type | Question Answered | Difficulty | Value | Tools |
|------|------------------|-----------|-------|-------|
| **Descriptive** | What happened? | Lowest | Low | Reports, dashboards, descriptive statistics |
| **Diagnostic** | Why did it happen? | Low | Medium | Data mining, correlations, data discovery |
| **Predictive** | What will happen? | High | High | Regression, time series, forecasting |
| **Prescriptive** | How can we make it happen? | Highest | Highest | Optimization, simulation, decision analysis |

> 🔑 **MCQ Trap**: The correct order is: **Descriptive → Diagnostic → Predictive → Prescriptive**

---

### Data Sources
- Humans, machines, human-machine combinations
- Social media (Facebook, LinkedIn), IoT devices, transactions

### Why Data is Important
- Better decisions | Solve underperformance | Evaluate performance | Benchmarking | Understanding customers/markets

### Data Analyst vs. Data Scientist
| Role | Focus |
|------|-------|
| **Data Analyst** | Domain-specific analytics (marketing analyst, finance analyst) |
| **Data Scientist** | Advanced algorithms + machine learning + building **data products** |

### 3 Skills for a Data Scientist
1. **Mathematics** (statistics, algorithms)
2. **Technology** (hacking skills — using data to extract information)
3. **Business & Strategy Acumen** (domain knowledge)

---

### Levels of Data Measurement (Critical for MCQs)

| Level | Ranking | Math Operations | Parametric? | Examples |
|-------|---------|-----------------|-------------|---------|
| **Nominal** | ❌ No ranking | None (not even mean) | ❌ Non-parametric only | Gender, Marital status, Eye color |
| **Ordinal** | ✅ Ranked | None (can't do arithmetic) | ❌ Non-parametric only | Customer satisfaction (1-5), Faculty rank, Grades |
| **Interval** | ✅ Ranked | + and − only (NO multiply/divide) | ✅ Parametric | Year (2019, 2020), Fahrenheit temp |
| **Ratio** | ✅ Ranked | +, −, ×, ÷ ALL operations | ✅ Parametric | Weight, Age, Salary, Kelvin temp |

> 🔑 **Why it matters**: Determines whether you use **parametric** or **non-parametric** tests.
> - Nominal/Ordinal → Non-parametric
> - Interval/Ratio → Parametric
> 
> Zero on interval scale is **arbitrary** (0°F ≠ no temperature); Zero on ratio scale is **absolute** (0 Kelvin = no heat)

### Classification of Data
```
Data
├── Categorical → Nominal (no ranking) / Ordinal (ranked)
└── Numerical
    ├── Discrete → Countable (e.g., number of children: 2, 3, not 2.5)
    └── Continuous → Infinite values possible (e.g., weight, voltage)
```

### Why Python?
- Free & open source | Interpreted (not compiled) | Dynamically typed | Extensive libraries
- Used by: Google, Facebook, NASA, Yahoo, eBay
- We use **Jupyter Notebook**: web-browser based, easy documentation, user-friendly

---

## 📌 LECTURE 2 — Python Fundamentals I

### Essential Python/Pandas Commands

| Command | What it does |
|---------|-------------|
| `import pandas as pd` | Load pandas library |
| `import numpy as np` | Load NumPy library |
| `import matplotlib.pyplot as plt` | Load plotting library |
| `df = pd.read_csv('path/file.csv')` | Load CSV file (use forward slashes `/`) |
| `df.head()` | Show **first 5 rows** |
| `df.tail()` | Show **last 5 rows** |
| `df.shape` | Returns **(rows, columns)** — NO parentheses |
| `df.columns` | Returns column names — NO parentheses |
| `df.dtypes` | Data type of each column — NO parentheses |
| `df.info()` | Full details about columns (name, non-null count, dtype) |

### Data Types in Pandas

| Pandas Type | Meaning |
|-------------|---------|
| `object` | String/character |
| `int` | Integer (whole number) |
| `float` | Decimal number |
| `datetime` | Date/time (must be imported) |

### Row/Column Access — `loc` vs `iloc`

| | `loc` | `iloc` |
|---|-------|--------|
| **Access by** | Label/name | Integer position |
| **Negative index** | ❌ Cannot use -1 | ✅ `-1` gets last row |
| **Example** | `df.loc[0]` → row 0 | `df.iloc[0]` → row 0 |
| **Multiple rows** | `df.loc[[0, 99, 999]]` | `df.iloc[[0, 99, 999]]` |

> 🔑 Python counts from **0** — 100th row = index 99

### Subsetting Columns
```python
# Single column
country_df = df['country']

# Multiple columns (double brackets)
subset = df[['country', 'continent', 'year']]

# Columns by position using iloc (: = all rows)
subset = df.iloc[:, [2, 4, -1]]   # -1 = last column

# Columns by range
small_range = list(range(5))       # [0,1,2,3,4]
subset = df.iloc[:, small_range]   # first 5 columns
```

### GroupBy & Stacked Tables
```python
# Group by year, get mean life expectancy
df.groupby('year')['lifeExp'].mean()

# Multi-level groupby
multi = df.groupby(['year','continent'])['lifeExp','gdpPercap'].mean()

# Flatten grouped table
flat = multi.reset_index()

# Unique counts
df.groupby('continent')['country'].nunique()
```

### Simple Plot
```python
global_yearly = df.groupby('year')['lifeExp'].mean()
global_yearly.plot()   # Auto x=year, y=lifeExp
```

---

## 📌 LECTURE 3 — Python Fundamentals II (Visualization)

### loc vs iloc (Extended)
```python
df.loc[0, 'country']          # Row 0, column 'country'
df.iloc[42, 0]                # Row 42, column 0 → 'Angola'
df.loc[10:13, ['country', 'lifeExp', 'gdpPercap']]  # Range of rows
```

### Chart Types & When to Use Them

| Chart | Use For | Key Notes |
|-------|---------|-----------|
| **Bar Chart** | Categorical comparisons | Different colors per category |
| **Pie Chart** | Proportions/composition | **Only for categorical (count) data** |
| **Histogram** | Distribution of continuous data | Shows shape (normal/skewed) |
| **Frequency Polygon** | Connect midpoints of histogram bars | Use **only for continuous** data |
| **Ogive (Cumulative Frequency)** | Cumulative distribution | Upper interval limit on x-axis |
| **Pareto Chart** | Quality control / 80-20 rule | Bars sorted descending + cumulative % line |
| **Scatter Plot** | Relationship between **two variables** | Shows trend (e.g., cars vs. gas sales) |

### Pareto Chart — Important Concept
- Bars arranged in **descending order** of frequency
- Cumulative frequency % plotted on second axis
- **80-20 Principle (Pareto Principle)**: 80% of problems are caused by 20% of reasons
- Example: 70% of motor failures from only 2 causes (poor wiring + short in coil)

### Principles of a Good Graph
1. Must NOT distort data
2. Scale on vertical axis should **begin at 0**
3. All axes must be properly labeled
4. Graph must have a title
5. Simplest possible graph for the dataset

---

## 📌 LECTURE 4 — Central Tendency

### Measures of Central Tendency — Summary

#### 1. Arithmetic Mean
- **Population Mean**: `µ = (X₁ + X₂ + ... + Xₙ) / N`
- **Sample Mean**: `x̄ = (X₁ + X₂ + X₃ + ... + Xₙ) / n`
- Applicable: **Interval and Ratio only** (NOT nominal, NOT ordinal)
- Affected by extreme values

**Example**: Data = {24, 13, 19, 26, 11} → µ = 93/5 = **18.6**

#### Mean of Grouped Data
`µ = Σ(fᵢ × mᵢ) / Σfᵢ`
- fᵢ = class frequency, mᵢ = class midpoint

**Example**:
| Class | Freq (f) | Midpoint (m) | f×m |
|-------|----------|--------------|-----|
| 20–30 | 6 | 25 | 150 |
| 30–40 | 18 | 35 | 630 |
| 40–50 | 11 | 45 | 495 |
| ... | ... | ... | ... |

µ = 2150 / 50 = **43**

---

#### 2. Weighted Average
`x̄_w = Σ(wᵢ × xᵢ) / Σwᵢ`

**Example**: Midterm score = 83 (weight 40%), Final = 95 (weight 60%)
`x̄_w = (0.4 × 83 + 0.6 × 95) / (0.4 + 0.6) = 90.2` → Gets A grade (≥90)

---

#### 3. Median
- Middle value in ordered array
- Applicable: **Ordinal, Interval, Ratio** (NOT nominal)
- **Not affected by extreme values** ✅

**Finding Median**:
- Median position = **(n + 1) / 2**
- Odd n: Take middle value
- Even n: Average of two middle values

**Example (odd, n=17)**: Position = (17+1)/2 = 9 → **9th value = 15**
**Example (even, n=16)**: Position = 8.5 → Average of 8th (14) + 9th (15) = **14.5**

**Median of Grouped Data**:
`Md = L + [(N/2 − cfp) / f_median] × W`
- L = lower limit of median class
- cfp = cumulative frequency before median class
- f_median = frequency of median class
- W = class width
- N = total frequency

**Example**: N=50, median position = 25. cfp=24, L=40, f=11, W=10
`Md = 40 + [(25 − 24) / 11] × 10 = 40.909`

---

#### 4. Mode
- Most frequently occurring value
- Applicable: **ALL levels** (Nominal, Ordinal, Interval, Ratio)
- Can be bimodal (2 modes) or multimodal

**Mode of Grouped Data**:
`Mo = L_Mo + [d₁ / (d₁ + d₂)] × W`
- d₁ = frequency of mode class − frequency of preceding class
- d₂ = frequency of mode class − frequency of following class

**Example**: Mode class 30–40 (freq=18), preceding=6, following=11
`Mo = 30 + [12 / (12 + 7)] × 10 = 36.31`

---

#### When to Use Which Central Tendency?
| Distribution Shape | Use |
|-------------------|-----|
| **Bell-shaped (symmetric)** | Mean = Median = Mode (any works) |
| **Right skewed (positive)** | Use **Median** |
| **Left skewed (negative)** | Use **Median** |

> 🔑 **Skewed data → always use Median** because it's always in the middle regardless of skewness.

---

#### 5. Percentile
- Divides data into **100 parts**
- P90 = at most 90% of data lies **below** it, at least 10% **above**
- Median = **50th percentile**
- Applicable: Ordinal, Interval, Ratio (NOT nominal)

**Computing Percentile — Location Index**:
`i = (P / 100) × n`
- If i is a whole number: percentile = average of values at position i and i+1
- If i is NOT a whole number: round up → use value at position ⌈i⌉

**Example**: n=8, P=30: i = (30/100) × 8 = 2.4 → not whole → use position 3 → value = **13**

---

### Measures of Dispersion

#### 1. Range
`Range = Largest value − Smallest value`
- Problem: ignores all values except extremes

**Example**: {35, 38, 40, 42, 44, 46, 48} → Range = 48 − 35 = **13**

#### 2. Quartiles
- **Q1** = 25th percentile (25% of data below)
- **Q2** = 50th percentile = **Median**
- **Q3** = 75th percentile (75% of data below)

**Example** (n=8, data sorted):
- Q1: i = 25/100 × 8 = 2 (even) → (109+114)/2 = **111.5**
- Q2: i = 4 → (116+121)/2 = **118.5**
- Q3: i = 6 → (122+125)/2 = **123.5**

#### 3. Interquartile Range (IQR)
`IQR = Q3 − Q1`
- Less influenced by extreme values

#### 4. Mean Absolute Deviation (MAD)
`MAD = Σ|Xᵢ − µ| / N`
- Average of absolute deviations from the mean
- Taking absolute value removes negatives (sum of deviations alone = 0)

#### 5. Population Variance
`σ² = Σ(Xᵢ − µ)² / N`
- Squaring: (1) removes negatives, (2) gives **higher penalty for larger deviations**

#### 6. Sample Variance
`s² = Σ(Xᵢ − x̄)² / (n − 1)`
- Divided by **(n−1)** to make it an **unbiased estimator** (one degree of freedom lost)

#### 7. Standard Deviation
- `σ = √(σ²)` (population)
- `s = √(s²)` (sample)
- In finance: SD = **risk** (higher SD = higher risk)
- In quality control: lower SD = better quality

---

## 📌 LECTURE 5 — Dispersion (Continued) + Python Implementation

### Empirical Rule (Normal/Bell-shaped distribution ONLY)
| Range | Coverage |
|-------|---------|
| µ ± 1σ | **68%** of all observations |
| µ ± 2σ | **95%** of all observations |
| µ ± 3σ | **99.7%** of all observations |

### Chebysheff's Theorem (ANY distribution shape)
`Proportion within k standard deviations ≥ 1 − (1/k²)`

| k | Minimum Coverage |
|---|-----------------|
| 2 | At least **75%** (vs 95% for normal) |
| 3 | At least **88.9%** (vs 99.7% for normal) |

> 🔑 Use **Empirical Rule** for bell-shaped, **Chebysheff** for unknown/non-normal distributions.

---

### Coefficient of Variation (CV)
`CV = (σ / µ) × 100`
- **Relative** measure of dispersion (%, not absolute)
- Used to **compare** two datasets with **different means/units**
- **Lower CV = better** (more consistent relative to its mean)

**Example**:
- Stock A: µ=29, σ=4.6 → CV = (4.6/29)×100 = **15.86%**
- Stock B: µ=84, σ=10 → CV = (10/84)×100 = **11.90%**
- **Stock B wins** (lower CV despite higher SD)

---

### Variance of Grouped Data
**Population**: `σ² = Σf(M − µ)² / N`
**Sample**: `s² = Σf(M − x̄)² / (n − 1)`
- M = midpoint of class, f = frequency

**Example**: Σf(M−µ)² = 7200, N=50 → σ² = 7200/50 = **144**, σ = **12**

---

### Skewness
Formula: `S = 3(µ − Median) / σ`
- S < 0 → **Negatively skewed** (left skewed)
- S = 0 → **Symmetric**
- S > 0 → **Positively skewed** (right skewed)

**Relationship between Mean, Median, Mode**:
| Shape | Relationship |
|-------|-------------|
| Symmetric | Mean = Median = Mode |
| Right skewed (+) | Mode < Median < **Mean** |
| Left skewed (−) | **Mean** < Median < Mode |

> 🔑 In right-skewed: mean is pulled to the **right** by large values.

### Kurtosis (Peakedness of Distribution)
| Type | Shape | Description |
|------|-------|-------------|
| **Leptokurtic** | High and thin | Very peaked, highly homogeneous |
| **Mesokurtic** | Normal | Normal distribution shape |
| **Platykurtic** | Flat and wide | Flat and spread out |

---

### Box and Whisker Plot
Five-number summary: **Min, Q1, Q2 (Median), Q3, Max**

Interpreting Skewness from Box Plot:
- Median line **toward right** of box → **Left (negative) skewed**
- Median line **toward left** of box → **Right (positive) skewed**
- Median line **in center** → **Symmetric**
- Points beyond whiskers (★) → **Outliers**

---

### Python Implementation Commands

```python
import pandas as pd
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt
import statistics

# Load data
table = pd.read_excel('IBM-313marks.xlsx')
x = table['total']   # select column

# Central Tendency
np.mean(x)           # Mean
np.median(x)         # Median
stats.mode(x)        # Mode (requires scipy)

# Percentile
a = np.array([1,2,3,4,5])
np.percentile(a, 50)   # 50th percentile = 3

# Quartiles
Q1 = np.percentile(a, 25)
Q2 = np.percentile(a, 50)
Q3 = np.percentile(a, 75)
IQR = Q3 - Q1

# Variance & Standard Deviation
np.var(x)                   # Population variance
statistics.pstdev(x)        # Population standard deviation
statistics.stdev(x)         # Sample standard deviation

# Skewness
from scipy.stats import skew
skew(x)                     # Positive = right skewed

# Box Plot
plt.boxplot(x, sym='*')
plt.show()

# Custom functions
def rangef(data):
    return max(data) - min(data)

def min_and_max(data):
    return min(data), max(data)

# For loop example
for i in range(10, 20, 2):   # start=10, end=20, step=2
    print(i, end=',')        # Output: 10,12,14,16,18
```

---

## 🎯 Week 1 Assignment — Key Questions & Answers

| Q | Topic | Answer |
|---|-------|--------|
| Q1 | Definition of data analytics | **(c)** Scientific process of transforming data into insights for decision-making |
| Q2 | Correct order of analytics types | **(b)** Descriptive → Diagnostic → Predictive → Prescriptive |
| Q3 | Level of measurement for "height" | **(d)** Ratio (true zero, all operations allowed) |
| Q4 | Select rows by integer position in Pandas | **(c)** `df.iloc[]` |
| Q5 | NOT a benefit of Jupyter Notebook | **(c)** Faster hardware execution |
| Q6 | Mean NOT appropriate for which data? | **(d)** Nominal (AND ordinal — but answer focuses on nominal/ordinal; nominal is "most wrong") |
| Q7 | Positively skewed distribution relationship | **(b)** Mode < Median < Mean |
| Q8 | Measure for comparing relative dispersion between two datasets | **(d)** Coefficient of Variation |
| Q9 | Mean=50, SD=10; % between 30 and 70 (i.e., µ±2σ) | **(c) 95%** (Empirical Rule) |
| Q10 | `y = x; y.append(40); len(x)` output | **(b) 4** (y is reference to x, so x is also modified) |

---

## 💡 Quick-Fire Review Checklist
- [ ] 4 types of analytics in order (D→D→P→P)
- [ ] Nominal/Ordinal = Non-parametric; Interval/Ratio = Parametric
- [ ] Mean: interval/ratio only | Median: ordinal+ | Mode: all
- [ ] Skewed data → **use Median**
- [ ] Population variance: divide by N | Sample variance: divide by **n−1**
- [ ] Empirical Rule: 68-95-99.7% for 1σ-2σ-3σ
- [ ] Chebysheff: ≥75% within 2σ (works for ANY distribution)
- [ ] CV = σ/µ × 100 (lower = better, used for comparison)
- [ ] `loc` cannot use -1; `iloc` can use -1 for last row
- [ ] `df.shape` and `df.columns` have **NO parentheses**
- [ ] Pie chart: **categorical only** | Histogram: **continuous data** only
- [ ] Pareto: 80-20 principle; bars in descending order + cumulative %
- [ ] Skewness formula: S = 3(µ − Median) / σ
- [ ] Box plot: median on right = left skewed; median on left = right skewed
