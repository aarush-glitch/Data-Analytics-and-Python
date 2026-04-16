# ⚡ Week 9 SUMMARY — Data Analytics with Python
## Lectures 41–45 | Confusion Matrix, ROC, Curvilinear & Interaction Regression

---

### 📍 Confusion Matrix
|  | Predicted 0 | Predicted 1 |
|--|-------------|-------------|
| **Actual 0** | TN | FP (Type I) |
| **Actual 1** | FN (Type II) | TP |

| Metric | Formula |
|--------|---------|
| **Accuracy** | (TP+TN)/Total |
| **Sensitivity** (Recall 1) | TP/(TP+FN) |
| **Specificity** (Recall 0) | TN/(TN+FP) |
| **Precision** | TP/(TP+FP) |
| **F1** | 2×(P×R)/(P+R) |
| **FPR** | FP/(FP+TN) = 1−Specificity |

- FP = Type I error (α); FN = Type II error (β)
- In classification report: recall of class 1 = **Sensitivity**; recall of class 0 = **Specificity**

---

### 📍 Threshold Effect
| Threshold t | Sensitivity | Specificity |
|------------|------------|------------|
| Very LOW (0.25) | **HIGH** | LOW |
| Default (0.50) | Moderate | Moderate |
| Very HIGH (0.75) | LOW | **HIGH** |

Optimal threshold = where TPR and (1−FPR) curves intersect

---

### 📍 ROC Curve & AUC
- Y-axis: **TPR (Sensitivity)**; X-axis: **FPR (1−Specificity)**
- Perfect point: (FPR=0, TPR=1) = **top-left corner**
- AUC=1.0 perfect; AUC=0.5 random; AUC<0.5 → reverse predictions
- Higher AUC = better model

---

### 📍 Regression Model Building
| Residual Pattern | Problem | Fix |
|-----------------|---------|-----|
| Horizontal band ✓ | None | Keep |
| U-shape / curve | Non-linearity | Add **x²** (curvilinear) |
| Funnel/cone | Heteroscedasticity | **log(Y)** transform |

**Curvilinear model:** `ŷ = b₀ + b₁x + b₂x²`  
**Interaction model:** `ŷ = b₀ + b₁x₁ + b₂x₂ + b₃(x₁×x₂)`

- "General linear model" = linear in **coefficients β** (not necessarily in x!)
- If interaction term t-test is significant → cannot interpret X₁ and X₂ independently
- Detect interaction: differences in means change across levels of other variable

---

### 📍 Log Transformation
- After log transform: back-transform with `e^(predicted)`
- Example: log(ŷ)=3.26 → ŷ = e^3.26 = 26.2 mpg

---

### 📍 Python Quick Ref
```python
from sklearn.metrics import (confusion_matrix, classification_report,
                             roc_curve, auc, accuracy_score)
from sklearn.preprocessing import binarize
import numpy as np

cm = confusion_matrix(y_test, y_pred)
print(classification_report(y_test, y_pred))

# ROC and AUC
fpr, tpr, thresholds = roc_curve(y_test, y_prob)
roc_auc = auc(fpr, tpr)

# Apply custom threshold
y_pred_new = binarize(y_prob.reshape(1,-1), threshold=0.45)[0]

# Curvilinear regression
x_sq = x_col ** 2
x_new = np.column_stack((x_col, x_sq))
model = sm.OLS(y, sm.add_constant(x_new)).fit()

# Interaction term
df['interaction'] = df['x1'] * df['x2']

# Log transform
df['log_y'] = np.log(df['y'])
model_log = sm.OLS(df['log_y'], sm.add_constant(df['x'])).fit()
y_original = np.exp(model_log.predict()) # back-transform
```
