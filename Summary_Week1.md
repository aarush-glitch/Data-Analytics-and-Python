# ⚡ Week 1 SUMMARY — Data Analytics with Python
## Lectures 1–5 | Data Types, Central Tendency, Dispersion

---

### 📍 Core Definitions
| Term | Key Fact |
|------|---------|
| **Data Analysis** | Studies the **past** |
| **Data Analytics** | Predicts the **future** |
| **Analytics order** | Descriptive → Diagnostic → Predictive → Prescriptive |
| **Data Scientist** | Math + Technology + Business Skills |

---

### 📍 Levels of Measurement
| Level | Ranking | Ops | Test | Example |
|-------|---------|-----|------|---------|
| Nominal | ❌ | None | Non-param | Gender |
| Ordinal | ✅ | None | Non-param | Grades |
| Interval | ✅ | +/− | Parametric | Year, °F |
| Ratio | ✅ | All | Parametric | Weight, Age |

> Zero on interval = arbitrary; Zero on ratio = absolute

---

### 📍 Central Tendency Formulas
| Measure | Formula | Data Level |
|---------|---------|-----------|
| **Mean** | µ = Σxᵢ/N (pop), x̄ = Σxᵢ/n (sample) | Interval/Ratio |
| **Median (grouped)** | L + [(N/2 − cfp)/f] × W | Ordinal+ |
| **Mode (grouped)** | L + [d₁/(d₁+d₂)] × W | All levels |
| **Weighted mean** | Σ(wᵢxᵢ)/Σwᵢ | Interval/Ratio |
| **Percentile** | i = (P/100)×n; if decimal → round up | Ordinal+ |

- Skewed data → **use Median**
- Symmetric: Mean = Median = Mode
- Right skewed: Mode < Median < **Mean**; Left skewed: **Mean** < Median < Mode

---

### 📍 Dispersion Formulas
| Measure | Formula |
|---------|---------|
| Range | Max − Min |
| IQR | Q3 − Q1 |
| MAD | Σ|Xᵢ−µ|/N |
| **Population Variance** | σ² = Σ(Xᵢ−µ)²/**N** |
| **Sample Variance** | s² = Σ(Xᵢ−x̄)²/**(n−1)** |
| CV | (σ/µ)×100 — lower = better |
| Skewness | S = 3(µ−Median)/σ |

---

### 📍 Empirical Rule & Chebysheff
| Rule | 1σ | 2σ | 3σ |
|------|----|----|-----|
| **Empirical (Normal only)** | 68% | **95%** | 99.7% |
| **Chebysheff (Any dist.)** | — | ≥75% | ≥88.9% |

---

### 📍 Python Quick Ref
```python
df.shape           # (rows, cols) — NO parentheses
df.columns         # column names — NO parentheses
df.loc[0]          # by label; CANNOT use -1
df.iloc[-1]        # by position; CAN use -1
np.mean(x); np.median(x); stats.mode(x)
np.percentile(a, 50)    # Q2/Median
np.var(x)               # Population variance
statistics.stdev(x)     # Sample SD
plt.boxplot(x, sym='*') # boxplot, * = outlier marker
```

### 📍 Graph Rules
- Pie chart: **categorical only** | Histogram: **continuous only**
- Pareto: bars descending + cumulative % line; **80-20 rule**
- Box plot: median on right = left skewed; median on left = right skewed
