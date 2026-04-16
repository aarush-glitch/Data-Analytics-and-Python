# 📊 Week 2 — Data Analytics with Python (NPTEL)
### Lectures 6–10 | Topics: Probability, Probability Laws, Probability Distributions

---

## 📌 LECTURE 6 — Introduction to Probability I

### Core Definitions

| Term | Definition |
|------|-----------|
| **Probability** | Numerical measure of likelihood an event will occur (range: 0 to 1) |
| **Experiment** | A process that produces outcomes |
| **Trial** | One repetition of an experiment |
| **Elementary Event** | Cannot be decomposed; the simplest possible outcome |
| **Event** | One or more outcomes; usually denoted by uppercase letters (A, B, E) |
| **Sample Space** | Set of ALL possible elementary events |

> 🔑 P(impossible event) = 0 | P(certain event) = 1 | Sum of all mutually exclusive & exhaustive events = **1**

---

### 3 Methods of Assigning Probability

| Method | Basis | Key Trait |
|--------|-------|-----------|
| **Classical** | Number of favourable outcomes / total outcomes | Determined **a priori** (before experiment); all outcomes equally likely |
| **Relative Frequency** | Historical data; frequency / total trials | Determined **after** experiment |
| **Subjective** | Personal intuition/reasoning | Different people may assign different probabilities to same event |

**Formula (Classical & Relative Frequency)**:
`P(E) = nₑ / N`
where nₑ = number of outcomes in E, N = total outcomes/trials

---

### Key Probability Terminology

| Term | Meaning |
|------|---------|
| **Union (A ∪ B)** | A **or** B occurring (combine both sets) |
| **Intersection (A ∩ B)** | A **and** B occurring (common elements only) |
| **Mutually Exclusive** | No common outcomes; A ∩ B = ∅; one happening prevents the other |
| **Independent Events** | Occurrence of one does NOT affect the other |
| **Collectively Exhaustive** | Contains ALL elementary events; covers entire sample space |
| **Complementary (A')** | All outcomes NOT in A; P(A') = 1 − P(A) |

---

### Counting Rules

| Rule | Formula | Example |
|------|---------|---------|
| **mn Rule** | n₁ × n₂ × ... × nₖ | Toss 2 coins: 2 × 2 = 4 outcomes |
| **With Replacement** | Nⁿ | 1000 returns, select 3 with replacement: 1000³ = 1 billion |
| **Without Replacement** | NCn = N! / (n!(N−n)!) | 1000 returns, select 3 without: 1000C3 = 166,167,000 |

---

### Types of Probability

| Type | Notation | Description |
|------|----------|-------------|
| **Marginal** | P(A) | Simple probability of one event |
| **Union** | P(A ∪ B) | Probability of A **or** B |
| **Joint** | P(A ∩ B) | Probability of A **and** B (inside cells of contingency table) |
| **Conditional** | P(A \| B) | Probability of A **given** B has occurred |

> 🔑 In a contingency table: **Inside cells = joint probabilities; Row/column totals = marginal probabilities**

---

### Law of Addition (General)
`P(A ∪ B) = P(A) + P(B) − P(A ∩ B)`

**For Mutually Exclusive events** (A ∩ B = 0):
`P(A ∪ B) = P(A) + P(B)`

**Example** (Office Design survey):
- P(Noise Reduction) = 0.70, P(Storage) = 0.67, P(Both) = 0.56
- P(N ∪ S) = 0.70 + 0.67 − 0.56 = **0.81**

---

### Conditional Probability
`P(A | B) = P(A ∩ B) / P(B)`

**Example** (Cars with AC and CD):
- P(AC) = 0.70, P(CD) = 0.40, P(both) = 0.20
- P(CD | AC) = 0.20 / 0.70 = **0.2857**

---

## 📌 LECTURE 7 — Introduction to Probability II

### Law of Multiplication
**General**: `P(X ∩ Y) = P(X) × P(Y|X) = P(Y) × P(X|Y)`

**Special (Independent events only)**:
`P(X ∩ Y) = P(X) × P(Y)`

> 🔑 **Testing Independence**: If P(A|B) = P(A), then A and B are independent.

**Example** (Company employees, 155 total):
- P(F ∪ P) = P(F) + P(P) − P(F ∩ P)
- = 55/155 + 44/155 − 13/155 = 0.355 + 0.284 − 0.084 = **0.555**

---

### Contingency Table — Master Technique

> 🔑 **Always build a contingency table** for probability problems. Once you know one joint cell and the marginals, you can fill the rest by subtraction.

**Format**:
|  | B | B' | Total |
|--|---|----|----|
| **A** | P(A∩B) | P(A∩B') | P(A) |
| **A'** | P(A'∩B) | P(A'∩B') | P(A') |
| **Total** | P(B) | P(B') | **1** |

---

### Bayes' Theorem
Used to **revise** (update) prior probabilities using new information.

`P(X|Y) = [P(Y|X) × P(X)] / Σ[P(Y|Xᵢ) × P(Xᵢ)]`

**The idea**: You know P(defect | supplier), you want to find P(supplier | defect) — Bayes reverses the conditional.

**Tabular approach** (easier):
1. List all prior probabilities (e.g., % from each supplier)
2. List conditional probabilities (e.g., defect rate per supplier)
3. Compute joint probabilities (prior × conditional)
4. Sum all joints → total probability of event
5. Posterior = joint / total

**Example** (Printer ribbons):
- Alamo: P(A) = 0.65, P(defect|A) = 0.08 → Joint = 0.052
- South Jersey: P(SJ) = 0.35, P(defect|SJ) = 0.12 → Joint = 0.042
- Total P(defect) = 0.052 + 0.042 = **0.094**
- P(Alamo | defective) = 0.052 / 0.094 = **0.553** (55.3%)
- P(SJ | defective) = 0.042 / 0.094 = **0.447** (44.7%)

---

## 📌 LECTURE 8 — Probability Distributions I

### What is a Distribution?
Describes the **shape** of a batch of numbers. Defined by **parameters** (e.g., normal distribution has µ and σ).

### Random Variable
- A variable whose values come from chance outcomes
- **Discrete**: Countable values (e.g., number of heads)
- **Continuous**: Any value in a range (e.g., mass, time)

### Probability Distribution Requirements
1. All probabilities: 0 ≤ P(x) ≤ 1
2. Sum of all probabilities = **1**

### Expected Value (Mean) of Discrete Distribution
`µ = E(X) = Σ[x · P(x)]`

**Example** (credit cards): 0(0.08) + 1(0.28) + 2(0.38) + ... + 6(0.01) = **1.97 ≈ 2**

### Variance of Discrete Distribution
`σ² = Σ[(x − µ)² · P(x)]`

**Shortcut formula**: `V(X) = E(X²) − [E(X)]² = Σ[x² · P(x)] − µ²`

**Example** (Quiz scores, µ=21):
σ² = 0.08(12−21)² + 0.15(18−21)² + 0.31(20−21)² + ...= **13.25**, σ = **3.64**

---

### Key Properties of Expected Value
| Property | Formula |
|----------|---------|
| Constant | E(c) = c |
| Sum | E(X + Y) = E(X) + E(Y) |
| Linear | E(aX + b) = aE(X) + b |
| Independent product | E(XY) = E(X)·E(Y) **only if independent** |

### Key Properties of Variance
| Property | Formula |
|----------|---------|
| Constant | Var(c) = 0 |
| Independent sum | Var(X + Y) = Var(X) + Var(Y) |
| Independent diff | Var(X − Y) = **Var(X) + Var(Y)** (still add!) |
| Scaled | Var(aX) = a²·Var(X) |
| Linear | Var(aX + b) = a²·Var(X) |

---

### Covariance and Correlation

**Covariance**:
`Cov(X,Y) = σₓᵧ = Σ(x − µₓ)(y − µᵧ) · P(x,y) = E(XY) − µₓ · µᵧ`
- Positive: variables move in same direction
- Negative: variables move in opposite directions
- If independent: Cov(X,Y) = 0

**Correlation coefficient**:
`ρ = Cov(X,Y) / (σₓ · σᵧ)`
- Gives **magnitude** of relationship (covariance only gives direction)

**Slope of regression** (preview):
`m = Cov(x,y) / Var(x)`

---

## 📌 LECTURE 9 — Probability Distributions II (Special Distributions)

### DISCRETE DISTRIBUTIONS

#### 1. Binomial Distribution
**When to use**: Fixed number of trials (n), each trial has exactly 2 outcomes (S/F), trials are **independent**, constant probability p.

**Probability Formula**:
`P(X = x) = ⁿCₓ · pˣ · qⁿ⁻ˣ`
where: ⁿCₓ = n! / (x!(n−x)!), q = 1 − p

**Parameters**:
- **Mean**: µ = **np**
- **Variance**: σ² = **npq**
- **Std Dev**: σ = **√(npq)**

**Example**: n=3, p=0.30, P(X=2)?
= ³C₂ × (0.3)² × (0.7)¹ = 3 × 0.09 × 0.7 = **0.189**

**Scale-up**: n=1000, p=0.30 → µ = 1000×0.3 = **300**, σ = √(1000×0.3×0.7) = **14.49**

---

#### 2. Poisson Distribution
**When to use**: Rare events; discrete occurrences over a continuous interval; arrivals, defects, errors.

**Properties**:
- Independent occurrences
- Occurrences range from 0 to ∞
- λ (lambda) must stay constant throughout

**Probability Formula**:
`P(X = x) = (λˣ · e⁻ˡ) / x!`
where e = 2.71828, λ = mean (long-run average)

**Parameters**:
- **Mean**: µ = **λ**
- **Variance**: σ² = **λ** ← SAME as mean (unique/univariate property!)
- **Std Dev**: σ = **√λ**

> 🔑 **Critical**: In Poisson, mean = variance = λ. This is a unique property!

> ⚠️ **Unit Caution**: λ and X must have the **same unit**. If λ = 3.2/4min and you want P(X=10 in 8min), adjust λ → 3.2 × 2 = 6.4/8min.

**Examples of Poisson**: Arrivals at airport/bank/computer, defects per unit length/area, errors per typed page.

---

#### 3. Hypergeometric Distribution
**When to use**: Sampling **without replacement** from a **finite** population.

**Key Difference from Binomial**: Trials are **NOT independent** (p changes each draw).

**Probability Formula**:
`P(x) = (ᴬCₓ × ᴺ⁻ᴬCₙ₋ₓ) / ᴺCₙ`

Where:
- N = Population size
- A = Number of successes in population
- n = Sample size
- x = Number of successes in sample

**Parameters**:
- **Mean**: µ = **An/N**
- **Variance**: σ² = **A(N−A)n(N−n) / N²(N−1)**

**Example**: N=10 computers, A=4 have illegal software, n=3 selected, P(x=2)?
= (⁴C₂ × ⁶C₁) / ¹⁰C₃ = (6 × 6) / 120 = **0.30**

> 🔑 **Binomial vs Hypergeometric**: With replacement (or infinite pop) → Binomial. Without replacement from finite pop → Hypergeometric. Binomial is acceptable approximation when N/10 ≥ n.

---

### CONTINUOUS DISTRIBUTIONS

#### 4. Uniform Distribution
**When to use**: Equal probability for all outcomes in range; also called **Rectangular distribution**.

Defined for interval **a ≤ x ≤ b**:

`f(x) = 1/(b − a)` for a ≤ x ≤ b; 0 elsewhere

**Parameters**:
- **Mean**: µ = **(a + b) / 2**
- **Std Dev**: σ = **(b − a) / √12**

**Probability between x₁ and x₂**:
`P(x₁ ≤ X ≤ x₂) = (x₂ − x₁) / (b − a)`

**Example**: Uniform over [41, 47]:
- f(x) = 1/6
- µ = (41+47)/2 = **44**
- σ = (47−41)/√12 = 6/3.464 = **1.732**
- P(42 ≤ X ≤ 45) = (45−42)/(47−41) = 3/6 = **0.50**

**Flight time example**: Uniform [120,140 min], P(120 ≤ x ≤ 130):
= (130−120)/(140−120) = 10/20 = **0.50**

---

#### 5. Exponential Distribution
**When to use**: Time **between** events (not count of events). Always use "time between" as the trigger phrase.

`f(x) = (1/µ) · e^(−x/µ)` where µ = mean time between events

**Cumulative probability** (most useful form):
`P(X ≤ x₀) = 1 − e^(−x₀/µ)`

**Example** (petrol pump): Mean between arrivals = 3 min. P(X ≤ 2)?
= 1 − e^(−2/3) = **0.4866**

---

### Poisson ↔ Exponential Relationship (MCQ Critical!)
| Distribution | Describes | Mean |
|-------------|-----------|------|
| **Poisson** | Number of occurrences **in** an interval | λ |
| **Exponential** | Length of interval **between** occurrences | 1/λ |

> Example: If 10 cars arrive per hour (Poisson λ=10), then time between arrivals is Exponential with µ = 1/10 hour.

---

#### 6. Normal Distribution
**Properties**:
- Bell-shaped, symmetric
- Mean = Median = Mode
- Location determined by µ; spread by σ
- Range: −∞ to +∞
- Total area under curve = 1

**Density function**:
`f(X) = (1/√(2πσ)) · e^(−½((x−µ)/σ)²)`

---

## 📌 LECTURE 10 — Normal Distribution (Deep Dive)

### Standardized Normal Distribution (Z-distribution)
Any normal distribution can be converted to **Standard Normal** (µ=0, σ=1).

**Z-transformation formula**:
`Z = (X − µ) / σ`

**Density function after standardization**:
`f(Z) = (1/√(2π)) · e^(−Z²/2)`

> 🔑 Why standardize? To use **Z-tables** instead of integrating every time.

### How to Use Z-Table

**Step-by-step**:
1. Draw the normal curve and shade the area of interest
2. Convert X to Z using Z = (X − µ)/σ
3. Look up area in Z-table
4. Adjust for direction (complement if needed)

**Key areas from table**:
| Z | Area (−∞ to Z) |
|---|----------------|
| 0 | 0.5000 |
| 0.12 | 0.5478 |
| 1.28 | 0.8997 |
| 1.96 | 0.9750 |
| 2.00 | 0.9772 |

**Example**: X ~N(µ=8, σ=5). Find P(X < 8.6):
- Z = (8.6 − 8)/5 = **0.12**
- P(Z < 0.12) = **0.5478**

**P(X > 8.6)**: 1 − 0.5478 = **0.4522**

**P(8 < X < 8.6)**:
- Z₁ = 0, Z₂ = 0.12
- Area = 0.5478 − 0.5000 = **0.0478**

### Finding X from Probability (Reverse)
If P(X < x) = 0.20, find x. Given µ=8, σ=5:
- From Z-table: area=0.20 → Z = **−0.84**
- X = µ + Z·σ = 8 + (−0.84)(5) = **3.80**

---

### Checking Normality (Conditions)
1. **Histogram** appears bell-shaped
2. **Box-whisker plot**: median line is centered
3. **Skewness ≈ 0**
4. **Interquartile range ≈ 1.33σ**
5. **Range ≈ 6σ**
6. ~68% observations within µ ± 1σ
7. ~95% observations within µ ± 2σ
8. ~99.7% within µ ± 3σ

### Why Does Normal Curve Never Touch the X-Axis?
The distribution provides provision for **rare extreme events** (beyond 3σ). The probability of values beyond ±3σ is only 0.3%, but not exactly 0 → curve approaches but never reaches x-axis.

---

## 📌 Distribution Summary Table (All distributions in one view)

| Distribution | Discrete/Continuous | When | Mean | Variance | Key Formula |
|-------------|--------------------|----|------|----------|-------------|
| **Binomial** | Discrete | n trials, 2 outcomes, independent, const. p | np | npq | ⁿCₓ pˣ qⁿ⁻ˣ |
| **Poisson** | Discrete | Rare events/interval | λ | **λ** (same!) | λˣe⁻ˡ / x! |
| **Hypergeometric** | Discrete | Without replacement, finite pop | An/N | A(N−A)n(N−n)/N²(N−1) | (ᴬCₓ)(ᴺ⁻ᴬCₙ₋ₓ)/(ᴺCₙ) |
| **Uniform** | Continuous | Equal probability over [a,b] | (a+b)/2 | (b−a)²/12 | 1/(b−a) |
| **Exponential** | Continuous | Time **between** events | µ | µ² | (1/µ)e^(−x/µ) |
| **Normal** | Continuous | Bell-shaped phenomena | µ | σ² | Complex PDF |

---

## 🎯 Week 2 Assignment — Key Questions & Answers

| Q | Topic | Answer | Why |
|---|-------|--------|-----|
| Q1 | Distribution for rare events over fixed interval | **(c) Poisson** | Poisson = rare, discrete events per interval |
| Q2 | Fair coin tossed twice, P(exactly 1 head) | **(b) 0.50** | ²C₁ × 0.5¹ × 0.5¹ = 2 × 0.25 = 0.50 |
| Q3 | Time between events in Poisson process | **(b) Exponential** | Poisson ↔ Exponential relationship |
| Q4 | Which is a discrete distribution? | **(b) Poisson** | Normal/Exponential/Gamma are continuous |
| Q5 | Binomial n=10, p=0.4, mean? | **(b) 4** | µ = np = 10 × 0.4 = 4 |
| Q6 | Hypergeometric sampling is done: | **(a) Without replacement** | Hypergeometric = finite pop, no replacement |
| Q7 | `poisson.var(4)` output | **(b) 4** | Poisson: variance **=** λ = 4 |
| Q8 | Mean and variance of Poisson? | **(d) λ and λ** | Both equal λ (unique property!) |
| Q9 | `norm.pdf(0)` output (standard normal PDF at Z=0) | **(c) 0.399** | f(0) = 1/√(2π) ≈ 0.3989 |
| Q10 | `stats.norm.cdf(0)` output | **(c) 0.50** | CDF at Z=0 = area from −∞ to 0 = 50% |

---

## 💡 Quick-Fire Review Checklist
- [ ] 3 methods of assigning probability (Classical, Relative Frequency, Subjective)
- [ ] Law of Addition: P(A∪B) = P(A) + P(B) − P(A∩B)
- [ ] Mutually exclusive → P(A∩B) = 0, so P(A∪B) = P(A) + P(B)
- [ ] Conditional: P(A|B) = P(A∩B)/P(B)
- [ ] Independence test: P(A|B) = P(A) → independent
- [ ] Bayes: P(X|Y) reverses conditional using joint/marginal
- [ ] Binomial: ⁿCₓpˣqⁿ⁻ˣ; mean=np; var=npq; **independent trials**
- [ ] Poisson: λˣe⁻ˡ/x!; mean = var = **λ** (unique!)
- [ ] Hypergeometric: **without replacement**, finite population
- [ ] Uniform: mean=(a+b)/2; SD=(b-a)/√12; P(x₁≤X≤x₂)=(x₂-x₁)/(b-a)
- [ ] Exponential: models time **between** events; P(X≤x₀) = 1−e^(−x₀/µ)
- [ ] Poisson(count in interval) ↔ Exponential(time between events); λ ↔ 1/λ
- [ ] Normal: bell-shaped, symmetric, mean=median=mode
- [ ] Z-transform: Z = (X−µ)/σ; Standard normal: µ=0, σ=1
- [ ] P(X > a) = 1 − P(X < a)
- [ ] To get X from probability: X = µ + Z·σ
- [ ] Normal curve never touches x-axis (provision for rare extreme events)
- [ ] Normality check: skewness≈0, IQR≈1.33σ, range≈6σ
