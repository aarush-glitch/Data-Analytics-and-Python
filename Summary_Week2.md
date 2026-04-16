# ⚡ Week 2 SUMMARY — Data Analytics with Python
## Lectures 6–10 | Probability & Probability Distributions

---

### 📍 Probability Laws
| Rule | Formula |
|------|---------|
| **Addition** | P(A∪B) = P(A)+P(B)−P(A∩B) |
| **Mutually Exclusive** | P(A∪B) = P(A)+P(B) |
| **Multiplication (general)** | P(X∩Y) = P(X)×P(Y\|X) |
| **Independent** | P(X∩Y) = P(X)×P(Y) |
| **Conditional** | P(A\|B) = P(A∩B)/P(B) |
| **Complement** | P(A') = 1−P(A) |
| **Bayes** | P(X\|Y) = P(Y\|X)×P(X) / ΣP(Y\|Xᵢ)P(Xᵢ) |

> Independence test: if P(A\|B) = P(A), then A and B are independent.

---

### 📍 Key Distribution Comparison Table
| Distribution | D/C | When | Mean | Variance | Formula |
|-------------|-----|------|------|----------|---------|
| **Binomial** | D | n trials, 2 outcomes, independent, const p | np | npq | ⁿCₓ pˣ qⁿ⁻ˣ |
| **Poisson** | D | Rare events per interval (**mean = var = λ**) | λ | **λ** | λˣe⁻ˡ/x! |
| **Hypergeometric** | D | Without replacement, finite | An/N | complex | (ᴬCₓ)(ᴺ⁻ᴬCₙ₋ₓ)/(ᴺCₙ) |
| **Uniform** | C | Equal probability on [a,b] | (a+b)/2 | (b−a)²/12 | P = (x₂−x₁)/(b−a) |
| **Exponential** | C | Time **between** events | µ | µ² | P(X≤x₀)=1−e^(−x₀/µ) |
| **Normal** | C | Bell-shaped | µ | σ² | Z=(X−µ)/σ |

---

### 📍 Critical Facts
- **Poisson unique**: Mean = Variance = λ
- **Poisson ↔ Exponential**: Poisson counts events; Exponential = time between (µ = 1/λ)
- **With replacement / N is large** → Binomial; **Without replacement, finite** → Hypergeometric
- **Unit warning**: match units of λ and X in Poisson!
- Normality check: skewness≈0, IQR≈1.33σ, range≈6σ

---

### 📍 Normal Distribution
- Z = (X−µ)/σ; Standard normal: µ=0, σ=1
- Z-table gives area from −∞ to Z
- P(X>a) = 1 − P(X<a)
- To find X: X = µ + Z·σ
- Curve never touches x-axis (provision for rare events)

---

### 📍 Python Quick Ref
```python
from scipy.stats import binom, poisson, norm, hypergeom, expon, uniform
binom.pmf(k, n, p)         # P(X=k)
binom.cdf(k, n, p)         # P(X≤k)
poisson.pmf(x, mu)         # P(X=x)
poisson.pmf(10, 3.2*2)     # unit adjust: 8min → λ=6.4
norm.cdf(x, mu, sigma)     # P(X≤x)
norm.ppf(0.95)             # Z-value for area=0.95
hypergeom.cdf(x, N, n, A) # P(X≤x) without replacement
1 - norm.cdf(700, 494, 100) # P(X>700)
```
