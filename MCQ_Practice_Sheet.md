# 🎯 NPTEL Data Analytics with Python — MCQ Practice Sheet
### 120 Questions | All 12 Weeks | With Full Answers & Explanations

---

## 📘 WEEK 1 — Data Types, Central Tendency, Dispersion

**Q1.** Which type of analytics answers "What will happen in the future?"  
a) Descriptive  b) Diagnostic  c) **Predictive**  d) Prescriptive  
> ✅ C — Predictive analytics answers "what will happen." Prescriptive answers "how to make it happen."

**Q2.** The correct order of analytics from lowest to highest difficulty is:  
a) Diagnostic → Descriptive → Predictive → Prescriptive  
b) **Descriptive → Diagnostic → Predictive → Prescriptive**  
c) Predictive → Descriptive → Prescriptive → Diagnostic  
d) Descriptive → Predictive → Diagnostic → Prescriptive  
> ✅ B — Difficulty increases from describing the past to optimizing the future.

**Q3.** "Customer satisfaction measured on a 1–5 scale" belongs to which level of measurement?  
a) Nominal  b) **Ordinal**  c) Interval  d) Ratio  
> ✅ B — Ordered but arithmetic operations (adding, dividing) are meaningless on satisfaction ranks.

**Q4.** Which variable level allows ALL arithmetic operations (+, −, ×, ÷)?  
a) Nominal  b) Ordinal  c) Interval  d) **Ratio**  
> ✅ D — Ratio has a true zero (e.g., weight, age), so all operations are valid.

**Q5.** Which measure of central tendency can be used for ALL levels of measurement?  
a) Mean  b) Median  c) **Mode**  d) Percentile  
> ✅ C — Mode is the only measure valid for Nominal data (e.g., most common eye color).

**Q6.** A dataset is right-skewed. The correct relationship between measures is:  
a) Mean = Median = Mode  b) Mean < Median < Mode  
c) **Mode < Median < Mean**  d) Mode > Median > Mean  
> ✅ C — In right skew, large outliers pull the mean rightward: Mode < Median < Mean.

**Q7.** Sample variance uses (n−1) in the denominator instead of n because:  
a) It is always larger  b) **It is an unbiased estimator of population variance**  
c) It is easier to compute  d) It reduces the standard deviation  
> ✅ B — Dividing by n−1 corrects for the fact that x̄ is used instead of µ (one degree of freedom lost).

**Q8.** Stock A: µ=29, σ=4.6; Stock B: µ=84, σ=10. Which has better relative consistency?  
a) Stock A (CV=15.86%)  b) **Stock B (CV=11.90%)**  
c) Both equal  d) Cannot be compared  
> ✅ B — CV(B)=10/84×100=11.9% < CV(A)=4.6/29×100=15.86%. Lower CV = more consistent.

**Q9.** According to Chebysheff's theorem, at least what % of data lies within k=3 standard deviations?  
a) 75%  b) 95%  c) **88.9%**  d) 99.7%  
> ✅ C — Chebysheff: ≥1−(1/k²) = 1−(1/9) = 88.9%. (99.7% is empirical rule for normal distributions.)

**Q10.** In a box plot, if the median line is toward the left side of the box, the distribution is:  
a) Symmetric  b) Left skewed  c) **Right skewed (positive)**  d) Uniform  
> ✅ C — Median toward left means most data clusters left and tail extends right (right skew).

---

## 📘 WEEK 2 — Probability & Distributions

**Q11.** P(A) = 0.70, P(B) = 0.67, P(A∩B) = 0.56. What is P(A∪B)?  
a) 0.56  b) 0.70  c) **0.81**  d) 1.37  
> ✅ C — P(A∪B) = P(A)+P(B)−P(A∩B) = 0.70+0.67−0.56 = 0.81.

**Q12.** A supplier provides 65% of ribbons with 8% defect rate; another provides 35% with 12% defect rate. Given a defective ribbon, P(from supplier 1) is:  
a) 0.447  b) **0.553**  c) 0.65  d) 0.08  
> ✅ B — Bayes: Joint(A1)=0.65×0.08=0.052; Joint(A2)=0.35×0.12=0.042; P(A1|def)=0.052/0.094=0.553.

**Q13.** Which distribution should be used for "number of defects per unit length of wire"?  
a) Binomial  b) Normal  c) **Poisson**  d) Hypergeometric  
> ✅ C — Poisson models rare discrete events over a continuous interval.

**Q14.** The Poisson distribution has a unique property where:  
a) Mean > Variance  b) **Mean = Variance = λ**  c) Variance = λ²  d) Mean < Variance  
> ✅ B — Poisson is the only distribution where mean = variance = λ.

**Q15.** A binomial experiment has n=10, p=0.4. What is the variance?  
a) 4  b) 24  c) **2.4**  d) 1.549  
> ✅ C — Var = npq = 10×0.4×0.6 = 2.4. (Mean = np = 4; Var ≠ Mean unlike Poisson.)

**Q16.** 4 out of 10 computers have illegal software. 3 are selected randomly WITHOUT replacement. P(exactly 2 have illegal software)?  
a) 0.189  b) 0.50  c) **0.30**  d) 0.40  
> ✅ C — Hypergeometric: (⁴C₂×⁶C₁)/¹⁰C₃ = (6×6)/120 = 0.30.

**Q17.** Flight time is uniform between 120 and 140 minutes. P(flight takes at most 130 min)?  
a) 0.25  b) **0.50**  c) 0.75  d) 1.0  
> ✅ B — P(X≤130) = (130−120)/(140−120) = 10/20 = 0.50.

**Q18.** If events in a period follow Poisson with λ=10 per hour, time between events follows:  
a) Normal  b) Poisson  c) Uniform  d) **Exponential with µ=1/10 hour**  
> ✅ D — Poisson counts ↔ Exponential measures time between events; µ = 1/λ.

**Q19.** X ~ N(µ=8, σ=5). What is P(X < 8.6)?  
a) 0.4522  b) 0.5000  c) **0.5478**  d) 0.9772  
> ✅ C — Z=(8.6−8)/5=0.12; from Z-table: P(Z<0.12)=0.5478.

**Q20.** Which test confirms whether data is normally distributed based on plotting?  
a) Chi-square test  b) F-test  c) Residual vs X plot  d) **QQ (normal probability) plot**  
> ✅ D — QQ plot: if points cluster on the 45° line, normality is satisfied.

---

## 📘 WEEK 3 — Sampling & Confidence Intervals

**Q21.** Within stratified sampling, the units within each stratum are:  
a) Heterogeneous  b) **Homogeneous**  c) Random  d) Sequential  
> ✅ B — Stratified: each stratum is homogeneous within; different strata are heterogeneous.

**Q22.** The Central Limit Theorem states that the distribution of sample means approaches Normal:  
a) Only if the population is normal  b) Only for large populations  
c) **For any population, when n is sufficiently large (≥25)**  d) Only for continuous variables  
> ✅ C — CLT works for ANY population shape when n is large enough.

**Q23.** Population: µ=50, σ²=100, n=25. What is Var(X̄)?  
a) 100  b) 10  c) 25  d) **4**  
> ✅ D — Var(X̄) = σ²/n = 100/25 = 4. Standard Error = √4 = 2.

**Q24.** For a 95% confidence interval with n=36, σ=12, what is the margin of error?  
a) 2.00  b) **3.92**  c) 12.00  d) 1.96  
> ✅ B — ME = 1.96 × (12/√36) = 1.96 × 2 = 3.92.

**Q25.** When population σ is unknown and n=20, the CI for µ uses:  
a) Z-distribution  b) F-distribution  c) **t-distribution with n−1=19 df**  d) Chi-square  
> ✅ C — σ unknown + small n → use t with df=n−1.

**Q26.** The correct interpretation of a 95% confidence interval is:  
a) P(µ is in this interval) = 0.95  
b) 95% of the data falls in this interval  
c) **95% of all such intervals would contain µ in repeated sampling**  
d) µ has a 95% chance of being the sample mean  
> ✅ C — Frequentist interpretation: not µ that varies, but the interval.

**Q27.** Degrees of freedom for chi-square test of sample variance with n=16 is:  
a) 16  b) **15**  c) 14  d) 17  
> ✅ B — df = n−1 = 16−1 = 15.

**Q28.** A 95% CI for population variance (n=17, s=74) uses which critical values?  
a) Z values  b) t values  c) F values  d) **Chi-square values**  
> ✅ D — CI for σ²: (n−1)s²/χ²_upper < σ² < (n−1)s²/χ²_lower.

**Q29.** To REDUCE the margin of error in a CI, you can:  
a) Increase confidence level  b) Decrease sample size  
c) **Increase sample size**  d) Decrease population SD (usually impossible)  
> ✅ C — ME = Z × σ/√n → larger n → smaller ME.

**Q30.** The finite population correction factor applies when:  
a) n < 30  b) Population is normal  
c) **n > 5% of N AND sampling without replacement**  d) σ is unknown  
> ✅ C — Both conditions must be met for correction: n/N > 0.05 AND without replacement.

---

## 📘 WEEK 4 — Hypothesis Testing

**Q31.** A company claims mean delivery time ≤ 30 minutes. A test checks if it actually exceeds 30 minutes. Hₐ is:  
a) µ ≤ 30  b) µ = 30  c) **µ > 30**  d) µ ≠ 30  
> ✅ C — The claim to PROVE is the alternative. Exceeding 30 → right-tailed: Hₐ: µ > 30.

**Q32.** A researcher rejects H₀ when it is actually true. This is:  
a) Type II Error  b) β error  c) **Type I Error (α)**  d) Power  
> ✅ C — Type I = incorrectly rejecting a true null = producer's risk = α.

**Q33.** p-value = 0.137, α = 0.05. The decision is:  
a) Reject H₀  b) Accept H₀  c) **Do not reject H₀**  d) Inconclusive  
> ✅ C — p=0.137 > α=0.05 → insufficient evidence to reject H₀.

**Q34.** Which action reduces BOTH Type I and Type II errors simultaneously?  
a) Decrease α  b) Increase β  c) **Increase sample size n**  d) Decrease σ  
> ✅ C — Only increasing n reduces both errors simultaneously. α and β normally have inverse relationship.

**Q35.** For the Z-test of proportions, the denominator uses:  
a) Sample proportion p̂  b) **Hypothesized proportion P₀**  c) Average of p̂ and P₀  d) Sample variance  
> ✅ B — Z = (p̂−P₀)/√(P₀Q₀/n). Always use P₀ in denominator.

**Q36.** Python's `ttest_1samp()` returns a p-value. For a one-tailed test, you should:  
a) Use it directly  b) Multiply by 2  c) **Divide by 2**  d) Add α to it  
> ✅ C — `ttest_1samp` always returns two-sided p-value; divide by 2 for one-tailed.

**Q37.** Power of a test equals:  
a) α  b) β  c) 1−α  d) **1−β**  
> ✅ D — Power = P(correctly rejecting false H₀) = 1 minus Type II error rate.

**Q38.** For two independent samples with unknown but EQUAL variances, use:  
a) Welch's t-test  b) Z-test  c) **Pooled t-test** with df=n₁+n₂−2  d) Paired t-test  
> ✅ C — Equal unknown variances → pooled Sp²; df = n₁+n₂−2.

**Q39.** X̄=32, µ₀=30, σ=10, n=30. What is the Z statistic?  
a) 2.00  b) **1.09**  c) 0.20  d) 3.65  
> ✅ B — Z = (32−30)/(10/√30) = 2/1.826 = 1.096 ≈ 1.09.

**Q40.** Var(X̄₁ − X̄₂) = ?  
a) σ₁²/n₁ − σ₂²/n₂  b) (σ₁+σ₂)²/(n₁n₂)  
c) **σ₁²/n₁ + σ₂²/n₂**  d) σ₁²σ₂²/(n₁n₂)  
> ✅ C — Variance of a difference = SUM of individual variances (even when subtracting!).

---

## 📘 WEEK 5 — ANOVA & Post Hoc

**Q41.** With 5 groups and α=0.05, running all pairwise t-tests would give an overall Type I error rate of approximately:  
a) 5%  b) 10%  c) 25%  d) **40%**  
> ✅ D — C(5,2)=10 comparisons; 1−(0.95)^10 = 1−0.599 = 0.401 ≈ 40%.

**Q42.** In ANOVA, SST = SSTR + SSE. If SSTR=240, k=4 treatments, what is MSTR?  
a) 60  b) **80**  c) 240  d) 30  
> ✅ B — df_treatment = k−1 = 3; MSTR = 240/3 = 80.

**Q43.** In a one-way ANOVA with k=4 groups and nT=24 total observations, the error df is:  
a) 3  b) 23  c) **20**  d) 21  
> ✅ C — df_error = nT−k = 24−4 = 20.

**Q44.** An F-statistic close to 1 in ANOVA indicates:  
a) Treatment is highly significant  b) H₀ should be rejected  
c) **No evidence of treatment effect (error dominates)**  d) Data is normally distributed  
> ✅ C — F = MSTR/MSE ≈ 1 → treatment variability ≈ error variability → no effect.

**Q45.** The F-test in ANOVA rejects H₀. What does this mean?  
a) All means differ  b) No means differ  
c) **At least 2 means are different**  d) Only the extreme means differ  
> ✅ C — ANOVA H₀: all means equal; Hₐ: NOT all equal (at least one pair differs).

**Q46.** Tukey HSD output shows `reject=True` for the pair (10%, 20%). This means:  
a) The means of these groups are equal  b) Do not reject H₀ for this pair  
c) **These two group means are significantly different**  d) Tukey test failed  
> ✅ C — `reject=True` in Tukey = significant difference. `reject=False` = means are equal.

**Q47.** For sample size to estimate a proportion P when P is completely unknown, you use P=:  
a) 0  b) 0.25  c) 1  d) **0.5**  
> ✅ D — P=0.5 maximizes P×Q=P×(1−P) → gives the largest (most conservative) needed sample size.

**Q48.** The F-test for comparing two population variances requires:  
a) σ₁² < σ₂²  b) Both samples from exponential populations  
c) **Both populations normally distributed and independent**  d) Equal sample sizes  
> ✅ C — F-test assumptions: normality and independence of the two populations.

**Q49.** Welch's t-test is used when:  
a) Variances are equal  b) Samples are dependent  
c) **Population variances are unknown and assumed unequal**  d) n > 30  
> ✅ C — When σ₁² ≠ σ₂² are both unknown, use Welch's (unequal variance t-test).

**Q50.** In post-hoc analysis using LSD, if |ȳ₁−ȳ₂| = 1.33 and LSD = 3.07:  
a) µ₁ ≠ µ₂  b) Reject H₀ for this pair  
c) **µ₁ = µ₂ (do not reject; difference ≤ LSD)**  d) Need AUC to decide  
> ✅ C — 1.33 < 3.07; the difference is smaller than LSD → means are NOT significantly different.

---

## 📘 WEEK 6 — RBD, Two-Way ANOVA & Simple Regression

**Q51.** RBD reduces error compared to CRD by:  
a) Increasing treatment effect  b) Removing interaction effect  
c) **Isolating and removing nuisance (blocking) variation from MSE**  d) Increasing sample size  
> ✅ C — Blocking puts nuisance into SSBL, making MSE smaller → F ratio larger.

**Q52.** In a Two-Way ANOVA, an interaction plot with PARALLEL lines indicates:  
a) Both factors are significant  b) **No interaction between factors**  
c) Strong interaction  d) Factor A is significant, Factor B is not  
> ✅ B — Parallel lines = no interaction; crossed/unparallel lines = interaction exists.

**Q53.** The slope b₁ in simple regression is calculated as:  
a) Syy/Sₓₓ  b) Sₓₓ/Sₓᵧ  c) **Sₓᵧ/Sₓₓ**  d) Sₓᵧ/Syy  
> ✅ C — b₁ = Sₓᵧ/Sₓₓ = Cov(X,Y)/Var(X).

**Q54.** R² = 0.87 in a regression. The correct interpretation is:  
a) Slope is 0.87  b) 87% of Y values equal ŷ  
c) **87% of the variability in Y is explained by the linear relationship with X**  d) r = 0.87  
> ✅ C — R² = proportion of Y's total variation explained by the regression model.

**Q55.** If the 95% CI for β₁ is (1.56, 8.44), the conclusion about β₁ is:  
a) β₁ = 0 (no relationship)  b) **β₁ ≠ 0 (statistically significant relationship)**  
c) β₁ is positive and negative  d) Cannot determine  
> ✅ B — 0 is NOT in the interval → reject H₀: β₁=0 → relationship is significant.

**Q56.** For simple regression with n=5 observations, degrees of freedom for error is:  
a) 5  b) 4  c) **3**  d) 2  
> ✅ C — df_error = n−p−1 = 5−1−1 = 3 (p=1 predictor; plus intercept = 2 parameters estimated).

**Q57.** Which Python argument gives Two-Way ANOVA (not one-way)?  
a) `typ=1`  b) **`typ=2`**  c) `typ=3`  d) Both are fine  
> ✅ B — `typ=1` for one-way ANOVA and RBD; `typ=2` for two-way factorial ANOVA.

**Q58.** The least squares regression line always passes through:  
a) The origin (0,0)  b) The mode of X  c) **The centroid (x̄, ȳ)**  d) The median point  
> ✅ C — By derivation, the OLS line always passes through the means of X and Y.

**Q59.** If b₁ = 5 (TV ads vs car sales) and X increases from 3 to 5, predicted change in Y is:  
a) 5  b) 15  c) **10**  d) 25  
> ✅ C — ΔY = b₁ × ΔX = 5 × (5−3) = 10. Not 15 (that would be 5×3).

**Q60.** Rejecting H₀: β₁=0 proves:  
a) X causes Y  b) Y causes X  c) **A statistically significant linear relationship** (not necessarily causal)  d) R² = 1  
> ✅ C — Correlation/regression establishes statistical association, not causation.

---

## 📘 WEEK 7 — Advanced Regression

**Q61.** A prediction interval is wider than a confidence interval for the same xp because:  
a) CI uses the t-distribution  b) PI uses the F-distribution  
c) **PI includes extra individual-level variability (added S² term)**  d) PI has fewer degrees of freedom  
> ✅ C — PI predicts ONE individual Y, adding uncertainty about where that individual falls.

**Q62.** A funnel/widening pattern in the residual plot indicates:  
a) Non-linearity  b) **Heteroscedasticity (non-constant variance)**  c) Autocorrelation  d) Outliers  
> ✅ B — Funnel/cone = variance increases with X = heteroscedasticity. Fix: log-transform Y.

**Q63.** A U-shaped pattern in the residual plot of a linear model indicates:  
a) Heteroscedasticity  b) **Non-linearity (need curvilinear/x² term)**  c) Normality violation  d) The model is perfect  
> ✅ B — Curved residuals = missed non-linearity. Solution: add x² term to model.

**Q64.** Adjusted R² decreased when a new variable was added. This means:  
a) R² also decreased  b) The variable is highly significant  
c) **The new variable is noise and should be removed**  d) The model improved  
> ✅ C — Adj. R² increases only if the new variable genuinely improves fit. If it drops, remove the variable.

**Q65.** If k=3 categories exist in a categorical variable added to regression, how many dummies are needed?  
a) 3  b) **2**  c) 1  d) 4  
> ✅ B — k categories → k−1 dummies. The omitted one becomes the reference group.

**Q66.** In regression with dummy variable β₂ = −0.789 for gender (female=1, male=0), it means:  
a) Males earn 0.789 more than females  b) **Females earn 0.789 units less than males (same experience)**  c) No difference  d) Salary decreases  
> ✅ B — β₂ < 0 and female=1, so being female → −0.789 units in salary vs male reference.

**Q67.** The Overall F-test in multiple regression tests:  
a) Each coefficient individually  b) Normality of residuals  
c) **That at least one predictor has β ≠ 0**  d) That all coefficients equal 1  
> ✅ C — H₀: β₁=…=βₚ=0; rejecting means at least one predictor is useful.

**Q68.** Standardized residuals beyond ±2 are considered:  
a) Normal  b) Random  c) Good  d) **Potential outliers**  
> ✅ D — 95% of observations should fall within ±2 standardized residuals.

**Q69.** For multiple regression, which plot should be used to check residual assumptions?  
a) Residual vs X₁  b) Residual vs X₂  
c) **Residual vs ŷ (fitted values)**  d) Residual vs y  
> ✅ C — In multiple regression, plot once against ŷ (catches all predictors together).

**Q70.** The QQ plot for residuals — what does a good outcome look like?  
a) All points above the diagonal  b) Points scattered randomly  
c) **Points clustered along the 45° diagonal line**  d) Flat horizontal line  
> ✅ C — Points on the 45° line → residuals are normally distributed → assumption met.

---

## 📘 WEEK 8 — MLE & Logistic Regression

**Q71.** MLE for the Normal distribution gives σ̂² = Σ(xᵢ−X̄)²/n. This differs from sample variance because:  
a) It uses n+1  b) **It is biased (uses n not n−1)**  c) It uses µ  d) They are identical  
> ✅ B — MLE for σ² uses n in denominator (biased); sample variance uses n−1 (unbiased). Different principles!

**Q72.** Logistic regression is used when the dependent variable is:  
a) Continuous  b) Ordinal  c) **Binary (0 or 1)**  d) Count  
> ✅ C — Logistic regression models P(Y=1) for binary outcomes. Use linear regression for continuous Y.

**Q73.** The output of logistic regression (ŷ) represents:  
a) Predicted Y value  b) Log-likelihood  
c) **P(Y=1) — probability of the positive outcome**  d) Odds ratio  
> ✅ C — Logistic regression outputs a probability between 0 and 1.

**Q74.** The G-test in logistic regression follows which distribution?  
a) Normal  b) F-distribution  c) t-distribution  d) **Chi-square with df=p**  
> ✅ D — G = 2(LL_model − LL_null) ~ χ²(df = number of predictors p).

**Q75.** If the logistic regression coefficient for "card" is b₂ = 1.0987, the Odds Ratio is:  
a) 1.0987  b) 2.0  c) **3.0 (≈ e^1.0987)**  d) 1.5  
> ✅ C — OR = e^(bᵢ); e^1.0987 ≈ 3.0 → card holders 3× more likely to use coupon.

**Q76.** If P = 0.40, the odds are:  
a) 0.40  b) 1.40  c) **0.667**  d) 2.5  
> ✅ C — Odds = P/(1−P) = 0.40/0.60 = 0.667.

**Q77.** A better logistic model (more predictors) would show:  
a) Higher −2LL  b) **Lower −2LL**  c) −2LL = 0  d) Same −2LL  
> ✅ B — Lower −2LL = better fit (analogous to lower SSE in linear regression).

**Q78.** MLE for the Exponential distribution gives:  
a) λ̂ = X̄  b) λ̂ = n × X̄  c) **λ̂ = 1/X̄**  d) λ̂ = σ  
> ✅ C — dℓ/dλ = 0 → n/λ = Σxᵢ → λ̂ = n/Σxᵢ = 1/X̄.

**Q79.** The individual variable test in logistic regression is called:  
a) F-test  b) G-test  c) LRT  d) **Wald z-test** (z = b̂ᵢ/SE)  
> ✅ D — Wald test is the logistic analog of the t-test in linear regression.

**Q80.** Minimum sample size per parameter recommendation for logistic regression is:  
a) 5  b) **10**  c) 30  d) 100  
> ✅ B — Logistic regression needs ≥10 observations per parameter (more than linear regression's 5).

---

## 📘 WEEK 9 — Confusion Matrix, ROC & Model Building

**Q81.** A model predicts "disease positive" for a healthy person. This is a:  
a) True Positive  b) True Negative  c) **False Positive (Type I error)**  d) False Negative  
> ✅ C — Predicted 1 (positive), Actual 0 (negative) = False Positive = Type I error.

**Q82.** Sensitivity = TP/(TP+FN). Which classification metric is this equal to?  
a) Precision  b) Specificity  c) F1-score  d) **Recall for class 1**  
> ✅ D — In classification report, recall for class 1 = sensitivity = TP/(TP+FN).

**Q83.** The ROC curve's X-axis is:  
a) Sensitivity  b) True Positive Rate  c) **False Positive Rate (1−Specificity)**  d) Precision  
> ✅ C — X-axis = FPR = FP/(FP+TN) = 1−Specificity; Y-axis = TPR = Sensitivity.

**Q84.** AUC = 0.5 means:  
a) Perfect classifier  b) Good classifier  c) Negative classifier  d) **Random guessing (useless)**  
> ✅ D — AUC=0.5 corresponds to the diagonal line = no better than random.

**Q85.** Lowering the classification threshold from 0.5 to 0.25 will:  
a) Increase specificity  b) Decrease sensitivity  
c) **Increase sensitivity, decrease specificity**  d) Have no effect  
> ✅ C — Lower threshold → more predicted positives → more TP but also more FP → sensitivity↑, specificity↓.

**Q86.** Curvilinear regression adds which term to the linear model?  
a) x₁×x₂  b) log(x)  c) **x²**  d) 1/x  
> ✅ C — Second-order (quadratic) model adds x² to capture curvature: ŷ = b₀+b₁x+b₂x².

**Q87.** The general linear model is "linear" in:  
a) The predictors X  b) The predictions ŷ  c) **The coefficients β**  d) The residuals  
> ✅ C — "Linear" = linear in parameters. x² is still a "general linear" model since β is to the power 1.

**Q88.** An interaction term between X₁ and X₂ is created as:  
a) X₁ + X₂  b) X₁ − X₂  c) X₁²  d) **X₁ × X₂**  
> ✅ D — Interaction variable = product of the two predictors.

**Q89.** In the log(Y) transform for heteroscedasticity, how do you get back the predicted Y?  
a) Square the prediction  b) Add log  c) Divide by mean  d) **Take e^(predicted value)** (anti-log)  
> ✅ D — After log transform, back-transform: ŷ_original = e^(log ŷ).

**Q90.** Optimal threshold in ROC is determined by:  
a) Maximizing AUC  b) Setting threshold = 0.5  
c) **Finding where TPR and (1−FPR) curves intersect**  d) Minimizing specificity  
> ✅ C — Optimal threshold balances sensitivity and specificity (intersection of both curves).

---

## 📘 WEEK 10 — Chi-Square & Cluster Analysis Intro

**Q91.** Chi-square test of independence: H₀ is:  
a) Variables are correlated  b) Frequencies are equal  
c) **Variables are independent (no relationship)**  d) Proportions are equal  
> ✅ C — H₀: variables are independent. H₁: variables are NOT independent (related).

**Q92.** A contingency table has r=4 rows and c=3 columns. df for chi-square test is:  
a) 12  b) 7  c) **6**  d) 11  
> ✅ C — df = (r−1)(c−1) = (4−1)(3−1) = 3×2 = 6.

**Q93.** Which assumption must all expected cell frequencies satisfy?  
a) fₑ ≤ 5  b) fₑ = fₒ  c) **fₑ ≥ 5**  d) fₑ > 0 only  
> ✅ C — If any expected frequency < 5, merge adjacent categories and recompute.

**Q94.** For a Goodness of Fit test with a Poisson distribution, df = k−2 because:  
a) Poisson has 2 parameters  b) **Poisson has 1 parameter (λ) estimated from data**  
c) df always −2  d) k intervals always start at 2  
> ✅ B — df = k−1−p; Poisson has p=1 parameter (λ=mean), so df = k−1−1 = k−2.

**Q95.** In goodness of fit, H₁ states:  
a) Data follows the distribution  b) Mean = expected mean  
c) **Data does NOT follow the tested distribution**  d) Variables are independent  
> ✅ C — Unlike independence test, H₁ says "does NOT follow"—opposite of usual.

**Q96.** Two points: A=(1,2), B=(3,5). Manhattan distance?  
a) 3.61  b) 2  c) 3  d) **5**  
> ✅ D — Manhattan = |3−1|+|5−2| = 2+3 = 5. Euclidean = √13 = 3.61 (shorter).

**Q97.** Standardization in cluster analysis is done to:  
a) Improve accuracy  b) Reduce computation  
c) **Remove the effect of different variable scales/units**  d) Increase cluster number  
> ✅ C — Variables measured in different units distort distances → standardize.

**Q98.** Cluster analysis is classified as:  
a) Supervised learning  b) Classification technique  
c) **Unsupervised learning**  d) Regression technique  
> ✅ C — No Y variable; no labels needed → unsupervised. Discriminant analysis = supervised.

**Q99.** The Minkowski distance with p=2 reduces to:  
a) Manhattan distance  b) Chebyshev distance  c) Cosine distance  d) **Euclidean distance**  
> ✅ D — Minkowski formula: [Σ|diff|ᵖ]^(1/p); p=2 → Euclidean; p=1 → Manhattan.

**Q100.** Irrelevant variables in clustering should be:  
a) Given double weight  b) Standardized separately  c) Averaged with others  d) **Removed (given zero weight)**  
> ✅ D — Non-informative variables add noise to distances and destroy cluster structure.

---

## 📘 WEEK 11 — Variable Types & Clustering Methods

**Q101.** For asymmetric binary variables, dissimilarity EXCLUDES:  
a) q (both=1)  b) r (i=1, j=0)  c) s (i=0, j=1)  d) **t (both=0)**  
> ✅ D — "Double negatives" (both=0) are not informative for asymmetric variables (e.g., HIV=negative for both).

**Q102.** Jaccard coefficient is:  
a) (q+t)/(q+r+s+t)  b) (r+s)/(q+r+s)  c) q/(q+r+s+t)  d) **q/(q+r+s)**  
> ✅ D — Jaccard similarity for asymmetric binary: q matches / (q+r+s). Excludes t.

**Q103.** Ordinal variable with ranks 1,2,3 (3 states). Standardized z for rank=2 is:  
a) 0  b) 1  c) **0.5**  d) 2  
> ✅ C — z = (rank−1)/(Mmax−1) = (2−1)/(3−1) = 1/2 = 0.5.

**Q104.** For a categorical variable with p=4 total features and m=3 matches, dissimilarity is:  
a) 3/4  b) **1/4 = 0.25**  c) 4/3  d) 0.75  
> ✅ B — d = (p−m)/p = (4−3)/4 = 0.25.

**Q105.** K-Means criterion function minimizes:  
a) Between-cluster distances  b) Number of clusters  
c) **Within-cluster Sum of Squared Distances (SSE)**  d) Euclidean distances between all points  
> ✅ C — E = Σ|p − mᵢ|² measures compactness. K-Means iterates until E is minimized.

**Q106.** K-Means assigns each point to:  
a) The farthest centroid  b) A randomly chosen centroid  
c) **The nearest centroid (minimum distance)**  d) The centroid with most points  
> ✅ C — Core step of K-Means: assign to the closest centroid based on Euclidean distance.

**Q107.** Which clustering method is MORE common — Agglomerative or Divisive?  
a) Divisive  b) **Agglomerative (AGNES)**  c) Both equal  d) Neither  
> ✅ B — AGNES (agglomerative/bottom-up) is the standard hierarchical clustering method.

**Q108.** Single linkage in hierarchical clustering uses:  
a) Maximum distance between clusters  b) Average distance  
c) Centroid distance  d) **Minimum distance between any two points across clusters**  
> ✅ D — Single linkage = min distance = nearest neighbour method. Produces elongated clusters.

**Q109.** Hierarchical clustering is NOT suitable for:  
a) Small datasets  b) When k is unknown  c) Dendrogram generation  d) **Very large datasets**  
> ✅ D — HAC requires computing and storing n×n distance matrix → computationally expensive for large n.

**Q110.** Compared to Hierarchical clustering, K-Means:  
a) Does not require k  b) Is reproducible  c) Produces a dendrogram  d) **Is faster for large datasets**  
> ✅ D — K-Means is O(n) vs HAC's O(n²) matrix. For large datasets, use K-Means.

---

## 📘 WEEK 12 — HAC Example & CART

**Q111.** In HAC with single linkage, the FIRST merge is between:  
a) Any two random objects  b) **The pair with minimum Euclidean distance**  c) The largest objects  d) The objects at center  
> ✅ B — HAC always starts by merging the two objects with the smallest pairwise distance.

**Q112.** CART stands for:  
a) Cluster Analysis and Regression Trees  b) Classification Algorithm for Random Trees  
c) **Classification And Regression Trees**  d) Collaborative Analysis of Recursive Tables  
> ✅ C — CART: supervised trees for both classification (categorical Y) and regression (continuous Y).

**Q113.** Which attribute selection measure does sklearn's `DecisionTreeClassifier()` use by default?  
a) Information Gain (entropy)  b) Gain Ratio  c) **Gini Index**  d) Chi-square  
> ✅ C — Default `criterion='gini'`. Use `criterion='entropy'` for Information Gain.

**Q114.** Information Gain = 0.940 − 0.694 = 0.246 for Age. This means:  
a) Age has the lowest information  b) Age is the worst split  
c) **Age has highest gain → chosen as root node**  d) Age gives maximum entropy  
> ✅ C — Highest Information Gain → best attribute for splitting → root node.

**Q115.** Information Gain is biased toward attributes that have:  
a) Few values  b) Binary values only  c) High entropy  d) **Many distinct values (e.g., ID numbers)**  
> ✅ D — Attributes with many values create many small pure partitions → spuriously high gain.

**Q116.** Gain Ratio corrects Information Gain bias by dividing by:  
a) Number of attributes  b) Dataset entropy  c) **SplitInfo (entropy of the attribute's split)**  d) Gini index  
> ✅ C — GainRatio = Gain/SplitInfo. SplitInfo penalizes attributes that split into many branches.

**Q117.** Gini(D) for a dataset with 9 Yes and 5 No (14 total) is:  
a) 0.940  b) 0.5  c) 0.357  d) **0.459**  
> ✅ D — Gini(D) = 1−(9/14)²−(5/14)² = 1−0.413−0.128 = 0.459.

**Q118.** Post-pruning replaces a subtree with a leaf node whose class is:  
a) Random  b) The most common in the full dataset  
c) **The majority class in that subtree**  d) Always class 0  
> ✅ C — Post-pruning: removed subtree → leaf assigned the majority class of that subtree.

**Q119.** In the LabelEncoder scheme `age: middle-aged=0, senior=1, youth=2`, the CART condition `age_n ≤ 0.5` is TRUE for:  
a) Youth  b) Senior  c) Youth and Senior  d) **Middle-aged only**  
> ✅ D — middle-aged = 0, which is ≤ 0.5. Senior=1 and Youth=2 are both > 0.5.

**Q120.** After encoding: student = {no=0, yes=1}. The CART condition `student_n ≤ 0.5` means:  
a) Student = Yes  b) **Student = No**  c) Both  d) Neither  
> ✅ B — no=0 which is ≤ 0.5 (True). yes=1 which is > 0.5 (False). So ≤ 0.5 → student = No.

---

## 🔑 FORMULA QUICK REFERENCE

### Statistics
| Formula | Name |
|---------|------|
| µ = Σxᵢ/N | Population mean |
| s² = Σ(xᵢ−x̄)²/(n−1) | Sample variance |
| CV = (σ/µ)×100 | Coefficient of Variation |
| Z = (X−µ)/σ | Standardization |
| Z = (X̄−µ)/(σ/√n) | Z for sample mean |
| Z = (p̂−P₀)/√(P₀Q₀/n) | Z for proportion |

### Confidence Intervals
| CI | Formula |
|----|---------|
| µ (σ known) | X̄ ± Zα/2(σ/√n) |
| µ (σ unknown) | X̄ ± t(n−1,α/2)(s/√n) |
| Proportion | p̂ ± Zα/2√(p̂q̂/n) |
| Variance | (n−1)s²/χ²_hi < σ² < (n−1)s²/χ²_lo |

### ANOVA
| Term | Formula |
|------|---------|
| F | MSTR/MSE |
| MSTR | SSTR/(k−1) |
| MSE | SSE/(nT−k) |
| LSD | t(α/2,nT−k) × √(2MSE/n) |
| Tukey CR | Q × √(MSE/n) |

### Regression
| Term | Formula |
|------|---------|
| b₁ | Sₓᵧ/Sₓₓ |
| b₀ | ȳ − b₁x̄ |
| R² | SSR/SST |
| Adj R² | 1−(1−R²)(n−1)/(n−p−1) |
| OR | e^(bᵢ) |

### CART
| Formula | Use |
|---------|-----|
| Info(D) = −Σpᵢlog₂(pᵢ) | Entropy |
| Gain(A) = Info(D)−Info_A(D) | Info gain (MAX) |
| GainRatio = Gain/SplitInfo | Gain ratio (MAX) |
| Gini(D) = 1−Σpᵢ² | Gini index (MIN) |

---
*120 Questions | Covers all 12 weeks | NPTEL Data Analytics with Python*
