# 📊 Week 9 — Data Analytics with Python (NPTEL)
### Lectures 41–45 | Topics: Confusion Matrix, ROC Curve, Curvilinear & Interaction Regression

---

## 📌 LECTURE 41 — Confusion Matrix & ROC (Theory I)

### Why We Need More Than Accuracy

For binary classifiers (logistic regression), the output is a **probability**. We must choose a **threshold (cut-off t)** to convert probability → class label (0/1).

Default threshold: **t = 0.5** (above → predict 1; below → predict 0)

---

### Confusion Matrix Layout

|  | **Predicted 0** | **Predicted 1** |
|--|----------------|----------------|
| **Actual 0** | True Negative **(TN)** | False Positive **(FP)** |
| **Actual 1** | False Negative **(FN)** | True Positive **(TP)** |

**Diagonal cells** (TN, TP) = correct predictions
**Off-diagonal cells** (FP, FN) = errors

> 🔑 **False Positive (FP)** = Type I error (α) — says YES when NO
> 🔑 **False Negative (FN)** = Type II error (β) — says NO when YES
> 🔑 **Sensitivity** = Power = 1−β; **Specificity** = 1−α

---

### Key Metrics from Confusion Matrix

| Metric | Formula | Meaning |
|--------|---------|---------|
| **Accuracy** | (TP + TN) / Total | % of all predictions that are correct |
| **Sensitivity** (Recall for class 1) | TP / (TP + FN) | % of actual positives correctly identified |
| **Specificity** (Recall for class 0) | TN / (TN + FP) | % of actual negatives correctly identified |
| **Precision** | TP / (TP + FP) | When model says positive, how often correct? |
| **Overall Error Rate** | (FP + FN) / Total | % of all predictions that are wrong |
| **False Positive Rate** | FP / (FP + TN) | = 1 − Specificity |
| **False Negative Rate** | FN / (TP + FN) | = 1 − Sensitivity |

**Example**:
- TN=2689, TP=201, FP=25, FN=85, Total=3000
- Accuracy = (2689+201)/3000 = 96.33%
- Error Rate = (25+85)/3000 = 3.67%

---

### Effect of Threshold on Sensitivity vs Specificity

| Threshold t | Effect | Sensitivity | Specificity |
|------------|--------|------------|------------|
| **Very LOW** (e.g., 0.25) | Predict positive for everyone | **HIGH** | **LOW** |
| **Very HIGH** (e.g., 0.75) | Predict negative for everyone | **LOW** | **HIGH** |
| **Default = 0.5** | Balance | Moderate | Moderate |

> 🔑 There is a **trade-off**: increasing sensitivity decreases specificity and vice versa
> 🔑 Both false positives AND false negatives are dangerous (medical analogy: wrongly diagnosing disease or missing a real disease)

---

### Medical Analogy (Key Intuition)

| Error | Example | Danger |
|-------|---------|--------|
| **False Positive** | Person without disease → test says POSITIVE | Unnecessary medication, anxiety |
| **False Negative** | Person WITH disease → test says NEGATIVE | Patient doesn't get treatment ← VERY dangerous |

---

### ROC Curve: Receiver Operating Characteristic

- **Y-axis**: True Positive Rate (TPR = Sensitivity)
- **X-axis**: False Positive Rate (FPR = 1 − Specificity)

ROC curve is created by plotting (FPR, TPR) for **every possible threshold value** from 0 to 1.

**Corner points**:
| Point | (FPR, TPR) | Meaning |
|-------|-----------|---------|
| **A** | (0, 1) | **Perfect classifier** — no errors at all! |
| **B** | (1, 0) | Worst classifier — can't predict anything |
| **C** | (0, 0) | Predicts EVERYTHING as negative (t=1) |
| **D** | (1, 1) | Predicts EVERYTHING as positive (t=0) |
| **Diagonal** | (x, x) | Random guessing |

---

## 📌 LECTURE 42 — Confusion Matrix & ROC (AUC + Threshold Selection)

### Types of ROC Curves

| ROC Curve | Interpretation |
|-----------|----------------|
| **Hugs upper-left corner** (reverse-L shape) | Very **good** classifier — high TPR, low FPR |
| **Bulges upward from diagonal** | **Good** classifier — some overlapping errors |
| **Close to diagonal** | **Poor** classifier — barely better than random |
| **Below diagonal** | **Bad** classifier — reverse its predictions! |

---

### Area Under Curve (AUC)

`AUC = Area under the ROC curve`

| AUC Value | Interpretation |
|-----------|----------------|
| **AUC = 1.0** | **Perfect** classifier |
| **AUC = 0.9** | Excellent |
| **AUC = 0.7–0.8** | Good |
| **AUC = 0.5** | **Random guessing** (useless) |
| **AUC < 0.5** | Worse than random (reverse predictions!) |

> 🔑 **Higher AUC = better model** regardless of threshold
> 🔑 AUC lets you **compare multiple classifiers** on a single number
> 🔑 When two curves cross: compare using AUC — the one with larger AUC is overall better

**When distributions don't overlap** → AUC = 1 (perfect)
**When distributions completely overlap** → AUC = 0.5 (random)

---

### Simmons Store Example (Confusion Matrix via Python)

```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import (accuracy_score, confusion_matrix,
                             classification_report, roc_auc_score, roc_curve)

data = pd.read_excel('Simmons.xls')
x = data[['card', 'spending']]
y = data['coupon']

# 75% train, 25% test split
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.25, random_state=0)
# x_train: 75 rows; x_test: 25 rows

# Fit logistic regression
model = LogisticRegression(solver='lbfgs')
model.fit(x_train, y_train.ravel())

# Predict on test set (class labels)
y_pred = model.predict(x_test)

# Predict probabilities
y_prob = model.predict_proba(x_test)[:, 1]  # probability of class 1

# Default threshold = 0.5
accuracy = accuracy_score(y_test, y_pred)           # 0.76
cm = confusion_matrix(y_test, y_pred)
# [[TN=15, FP=1], [FN=5, TP=4]]

print(classification_report(y_test, y_pred))
# recall for class 0 = specificity = 0.94
# recall for class 1 = sensitivity = 0.44
# precision = TP/(TP+FP)
```

---

### How to Use Classification Report

```
              precision  recall  f1-score  support
           0       0.75    0.94      0.83       16
           1       0.80    0.44      0.57        9
```

| Column | Formula | Intuition |
|--------|---------|-----------|
| **precision** | TP/(TP+FP) | When it says YES, how often is it right? |
| **recall** | TP/(TP+FN) | Of all actual YES, how many did it catch? |
| **f1-score** | 2×(P×R)/(P+R) | Balance between precision and recall |
| **support** | Count of actual samples | How many in that class |

> 🔑 In classification report: recall for **class 1 = Sensitivity**; recall for **class 0 = Specificity**

---

## 📌 LECTURE 43 — Optimal Threshold via ROC (Python Demo)

### Default vs Optimal Threshold

At **t = 0.5** (default): Accuracy=0.76, Specificity=0.94, Sensitivity=0.44

As threshold changes:
| Threshold | TN | TP | Specificity | Sensitivity |
|-----------|----|----|------------|------------|
| **t = 0.35** (low) | 8 | 9 | 0.50 | ~1.0 |
| **t = 0.50** (default) | 15 | 4 | 0.94 | 0.44 |
| **t = 0.70** (high) | 16 | 0 | 1.00 | 0.00 |
| **t = 0.45** (**optimal**) | 14 | 8 | 0.88 | 0.89 ← **best balance** |

**Optimal threshold** = point where TPR and (1−FPR) curves **intersect** → gives balanced specificity + sensitivity

---

### Python: Finding Optimal Threshold

```python
from sklearn.metrics import roc_auc_score, roc_curve, auc
from sklearn.preprocessing import binarize
import numpy as np, pandas as pd

# ROC curve for training data
fpr1, tpr1, thresholds1 = roc_curve(y_train, y_prob_train)
roc_auc1 = auc(fpr1, tpr1)          # AUC = 0.64 for training

# ROC curve for test data
fpr, tpr, thresholds = roc_curve(y_test, y_prob)
roc_auc = auc(fpr, tpr)             # AUC = 0.90 for test ← model generalizes well!

# Find optimal threshold (intersection of TPR and 1-FPR)
i = np.arange(len(tpr))
roc = pd.DataFrame({
    'fpr': pd.Series(fpr, index=i),
    'tpr': pd.Series(tpr, index=i),
    '1-fpr': pd.Series(1-fpr, index=i),
    'threshold': pd.Series(thresholds, index=i)
})
# Optimal where |TPR - (1-FPR)| is minimized → threshold ≈ 0.457

# Apply optimal threshold
optimal_t = 0.45   # or 0.457
y_pred_optimal = binarize(y_prob.reshape(1, -1), threshold=optimal_t)[0].astype(int)
cm_optimal = confusion_matrix(y_test, y_pred_optimal)
# TN=14, TP=8; Specificity=0.88, Sensitivity=0.89
```

> 🔑 AUC of test set (0.90) > AUC of train set (0.64) → model is performing **better on unseen data** (which is unusual; typically you want train ≈ test AUC)
> 🔑 Baseline accuracy = proportion of majority class = 60% (60 non-coupon users out of 100)
> 🔑 Any model must beat the **baseline accuracy** to be useful

---

## 📌 LECTURE 44 — Regression Model Building I (Curvilinear Relationships)

### Model Building Philosophy

**Model building** = choosing the right form (linear vs non-linear) AND the right variables.

Residual plots → first check; if non-rectangular → model form may be wrong!

---

### First-Order vs Second-Order (Curvilinear) Model

| Model | Equation | When to use |
|-------|----------|------------|
| **First-order (linear)** | ŷ = b₀ + b₁x₁ | Residual plot: horizontal band |
| **Second-order (quadratic)** | ŷ = b₀ + b₁x₁ + b₂x₁² | Residual plot shows curve/U-shape |
| **General linear model** | ŷ = b₀ + Σbⱼzⱼ + ε | Any combination of x, x², x₁x₂, dummy vars |

> 🔑 **"General linear model" = linear in coefficients (β)**, NOT necessarily linear in x!
> The model `ŷ = b₀ + b₁x + b₂x²` is called **general linear** because β₀, β₁, β₂ are all to the power 1.

---

### Worked Example (Reynolds — Salesperson Months vs Scales Sold)

**Data**: Y=scales sold; X=months employed; n=15

**Step 1**: Fit simple linear regression:
```
ŷ = 111.22 + 2.37 × (months)
R² = 0.781; p < 0.05 → significant
```

**Step 2**: Check residual plot → **curvilinear pattern** (not rectangular) → linear model insufficient!

**Step 3**: Add x² as a new variable:
```python
x_sq = x ** 2                                    # square the independent variable
x_new = np.column_stack((x, x_sq))              # combine x and x²
x_new2 = sm.add_constant(x_new)                 # add intercept
model2 = sm.OLS(y, x_new2).fit()
```

**Step 4**: New quadratic model:
```
ŷ = 45.3 + 6.34×(months) − 0.0345×(months²)
R² = 0.90 (up from 0.781!)
Adj. R² = 0.886
Both x and x² have p < 0.05 → both significant
```

**Step 5**: Residual plot of new model → **rectangular (random scatter)** → assumptions satisfied ✓

> 🔑 **Always check residual plots** — significant R² + p-values alone are NOT enough!
> 🔑 Curvilinear/U-shape residuals → add x² term → check residuals again

```python
# Residual plot for detecting non-linearity
E = model.resid_pearson              # standardized residuals
y_hat = model.fittedvalues           # predicted values
plt.scatter(y_hat, E)
# If U-shaped or curved → go curvilinear (add x²)
# If rectangular band → model is good
```

---

## 📌 LECTURE 45 — Regression Model Building II (Interaction)

### What is Interaction?

**Interaction** between X₁ and X₂ = the effect of X₁ on Y **depends on** the value of X₂ (and vice versa).

**Interaction variable** = Z₃ = X₁ × X₂ (product of the two independent variables)

**Second-order model with interaction**:
`ŷ = β₀ + β₁x₁ + β₂x₂ + β₃x₁² + β₄x₂² + β₅(x₁×x₂)`

Simple interaction model (no squares):
`ŷ = β₀ + β₁x₁ + β₂x₂ + β₃(x₁×x₂)`

---

### How to Detect Interaction

**Summary Table Analysis**: Compute difference in mean Y between levels of X₂ for each level of X₁.

**If differences are CONSTANT** → No interaction
**If differences CHANGE** → Interaction exists!

| Price (X₁) | Diff in mean sales (Adv=100k vs Adv=50k) |
|------------|------------------------------------------|
| $2.00 | 347,000 |
| $2.50 | 282,000 |
| $3.00 | 43,000 |

Differences are NOT constant → decreasing as price increases → **Interaction exists!**

> 🔑 At higher price, advertising becomes **less effective** (diminishing returns) → interaction between price and advertising

---

### Worked Example (Shampoo Sales — Price × Advertising)

Variables: Y=units sold; X₁=unit price; X₂=advertising expenditure

**Python**:
```python
# Create interaction variable: Z3 = X1 × X2
tbl2['PriceAdv'] = tbl2['Price'] * tbl2['AdvExp']   # interaction term

# Regression with interaction
result = smf.ols('Sales ~ Price + AdvExp + PriceAdv', data=tbl2).fit()
print(result.summary())
```

**Output**:
```
ŷ = −276 + 175×Price + 19.7×AdvExp − 6.08×(Price×AdvExp)
R² = 0.978  (excellent fit)
All p-values < 0.05 → all variables (including interaction) significant
```

> 🔑 Coefficient of interaction term (−6.08) is NEGATIVE → as price↑, effect of advertising↓
> 🔑 If the interaction term t-test is significant → interaction is real; both X₁ and X₂ effects can only be interpreted **jointly**, not separately

---

### Transforming the Dependent Variable (Y)

**When to transform Y**: Residual plot shows **funnel/cone shape** (heteroscedasticity — variance grows with X)

**Fix**: Take **log(Y)** as the new dependent variable

```python
# Example: Y = miles per gallon; X = weight
# Residual plot shows cone → take log
tbl3['log_Y'] = np.log(tbl3['mpg'])                 # log transformation of Y

model_log = sm.OLS(tbl3['log_Y'], sm.add_constant(tbl3['weight'])).fit()
# Residual plot now = rectangular ✓

# Interpretation: predicted y is log scale → back-transform!
log_y_hat = model_log.predict()
y_hat_original = np.exp(log_y_hat)                  # anti-log to get original units

# Example: log(ŷ) = 3.26 → ŷ = e^3.26 = 26.2 miles per gallon
```

> ⚠️ After log transformation, always **back-transform** using `e^(predicted value)` to interpret in original units!
> 🔑 Cone/funnel shape residuals → transform Y (log); curved residuals → transform X (add x²)

---

### Transformation Decision Guide

| Residual Plot Pattern | Problem | Fix |
|----------------------|---------|-----|
| Horizontal band ✓ | None | Keep current model |
| U-shape / curve | Non-linearity | Add X² term (transform X) |
| Funnel/cone | Heteroscedasticity | Log transform Y |
| Both patterns | Complex | Try both transformations |

---

## 🎯 Week 9 Assignment — Key Questions & Answers

| Q | Topic | Answer | Formula |
|---|-------|--------|---------|
| Q1 | Model predicts positive but actual is negative | **False Positive (FP)** | Type I error |
| Q2 | Accuracy formula | **(TP + TN) / Total** | Diagonal sum / all |
| Q3 | Metric showing TP/(TP+FN) | **Recall / Sensitivity** | Catches true positives |
| Q4 | Effect of lowering cutoff threshold | **Increases predicted positives** | More classified as 1 |
| Q5 | X-axis of ROC curve | **False Positive Rate** | FPR = FP/(FP+TN) = 1−Specificity |
| Q6 | Perfect AUC value | **1** | No false positives or negatives |
| Q7 | Which metric reduces false negatives? | **Sensitivity** | High sensitivity → fewer FN |
| Q8 | Simple first-order regression | **ŷ = β₀ + β₁x₁ + ε** | One predictor, linear |
| Q9 | Curvilinear term added to model | **x²** | Squared predictor captures curves |
| Q10 | Interaction variable created by | **x₁ × x₂** | Product of two predictors |

---

## 💡 Quick-Fire Review Checklist

**Confusion Matrix:**
- [ ] Rows = Actual; Columns = Predicted (or vice versa, but be consistent)
- [ ] TN = correct 0→0; TP = correct 1→1; FP = wrong 0→1; FN = wrong 1→0
- [ ] FP = Type I error (α); FN = Type II error (β)
- [ ] Accuracy = (TP+TN)/Total; Sensitivity = TP/(TP+FN); Specificity = TN/(TN+FP)
- [ ] Precision = TP/(TP+FP); F1 = 2×(P×R)/(P+R)
- [ ] Classification report: recall for class 1 = sensitivity; recall for class 0 = specificity
- [ ] Python: `confusion_matrix(y_test, y_pred)`, `classification_report(y_test, y_pred)`

**ROC Curve:**
- [ ] Y-axis = TPR (Sensitivity); X-axis = FPR (1−Specificity)
- [ ] Perfect point: (FPR=0, TPR=1) = top-left corner
- [ ] High threshold t → high specificity, low sensitivity (predict mostly negative)
- [ ] Low threshold t → high sensitivity, low specificity (predict mostly positive)
- [ ] AUC=1 perfect; AUC=0.5 random guessing; AUC<0.5 → reverse predictions
- [ ] Optimal threshold = intersection of TPR and (1−FPR) curves → balanced specificity and sensitivity
- [ ] Python: `roc_curve(y_true, y_prob)`, `auc(fpr, tpr)`, `roc_auc_score(y_true, y_prob)`
- [ ] To apply threshold: `binarize(y_prob.reshape(1,-1), threshold=t)`

**Curvilinear (Nonlinear) Regression:**
- [ ] Detect via residual plot: curve/U-shape → add x² term
- [ ] z₂ = x₁² is the new variable; model stays "general linear" (linear in β)
- [ ] "Linear" in linear regression means linear in **coefficients**, NOT in x!
- [ ] Always check residual plot AFTER adding x² → should become rectangular band
- [ ] R² improves when correct transformation is used
- [ ] Python: `x_sq = x**2; x_new = np.column_stack((x, x_sq)); sm.OLS(y, sm.add_constant(x_new)).fit()`

**Interaction:**
- [ ] Detect via summary table: if differences in means change across levels → interaction!
- [ ] Interaction variable = x₁ × x₂ (product of the two predictors)
- [ ] Add interaction term z₃ = x₁×x₂ to regression model
- [ ] If interaction coefficient t-test significant → interaction is real
- [ ] Can't interpret X₁ and X₂ effects independently when interaction is present
- [ ] Python: `df['interaction'] = df['x1'] * df['x2']`; include in OLS formula

**Y Transformation:**
- [ ] Cone/funnel residual plot → heteroscedasticity → log transform Y
- [ ] New model: log(Y) = b₀ + b₁x₁ (same X, but log Y as dependent variable)
- [ ] After fitting, back-transform: predicted Y = e^(ŷ_log)
- [ ] Always back-transform to interpret results in original units!
- [ ] Python: `df['log_y'] = np.log(df['y']); sm.OLS(df['log_y'], sm.add_constant(df['x'])).fit()`
