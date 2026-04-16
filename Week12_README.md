# 📊 Week 12 — Data Analytics with Python (NPTEL)
### Lectures 56–60 | Topics: Hierarchical Clustering (HAC), CART (Decision Trees), Attribute Selection Measures

---

## 📌 LECTURE 56 — Hierarchical Clustering II: Agglomerative Example + Python

### HAC Step-by-Step Algorithm (AGNES)

**Input**: n objects with measured variables
**Output**: Dendrogram (tree showing all merges)

**Algorithm**:
1. Compute **n×n distance matrix** (all pairwise Euclidean distances)
2. Find **minimum distance** in matrix → merge those 2 objects into a cluster
3. **Update distance matrix**: Replace merged pair with new cluster
   - For single linkage: new distance = **min(d_ia, d_ja)** for any other object a
   - i.e., take the SMALLER of the two distances from the merged objects
4. Repeat steps 2–3 until all objects are in 1 cluster

---

### HAC Numerical Example (7 Objects, Single Linkage)

Data: 7 objects with Var1 and Var2 coordinates

**Initial distance matrix** (key distances):
- d(1,4) = **0.71** ← MINIMUM → merge first!
- d(2,3) = 1.12; d(6,7) = 1.95; d(3,7) = 1.68; d(2,6) = 1.81

**Merging sequence (single linkage)**:

| Step | Merge | Min distance | Resulting cluster |
|------|-------|-------------|-------------------|
| 1 | {1} + {4} | **0.71** | {1,4} |
| 2 | {2} + {3} | **1.12** | {2,3} — update: d({2,3}, {1,4}) = min(4, 4.27)=4 |
| 3 | {1,4} + {5} | **1.41** | {1,4,5} — d(5,1)=1.41; d(5,4)=1.58; min=1.41 |
| 4 | {2,3} + {7} | **1.68** | {2,3,7} — d(3,7)=1.68; d(2,7)=2.51; min=1.68 |
| 5 | {2,3,7} + {6} | **1.81** | {2,3,6,7} — d(2,6)=1.81 |
| 6 | {1,4,5} + {2,3,6,7} | **4.0** | {1,2,3,4,5,6,7} — all merged! |

**Reading the dendrogram**:
- Horizontal level of merge = distance at which objects joined
- Cut at d=4 → 2 clusters: {1,4,5} and {2,3,6,7}
- Cut at d=1.5 → 3+ clusters

---

### HAC Python

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from scipy.cluster.hierarchy import dendrogram, linkage, fcluster
from scipy.spatial.distance import pdist
from sklearn.cluster import AgglomerativeClustering

# Data
data = np.array([[2,2],[5.5,4],[5,5],[1.5,2.5],[1,1],[7,5],[5.75,6.5]])

# Draw Dendrogram (single linkage)
linked = linkage(data, method='single')
dendrogram(linked, labels=[str(i+1) for i in range(7)])
plt.show()

# Apply with k=2
model = AgglomerativeClustering(n_clusters=2, metric='euclidean', linkage='single')
labels = model.fit_predict(data)
# labels = [1, 0, 0, 1, 1, 0, 0]
# Cluster 0: objects {2,3,6,7}; Cluster 1: objects {1,4,5}

# Visualize
plt.scatter(data[:, 0], data[:, 1], c=labels)
plt.show()
```

> 🔑 `linkage(data, method='single')` → single linkage (min distance); method can also be 'complete', 'average', 'ward'
> 🔑 `AgglomerativeClustering(n_clusters=k)` → specify k to cut dendrogram at right level
> 🔑 Optimal k: Look for large gaps in dendrogram OR use visual inspection

---

## 📌 LECTURE 57 — CART (Classification & Regression Trees) I: Introduction

### What is CART?

**Classification And Regression Trees (CART)**: A **supervised** machine learning technique that predicts outcomes using a tree structure.

| | Classification | Regression |
|--|---|---|
| **Dependent variable** | Categorical (e.g., buy/don't buy) | Continuous (e.g., salary in $) |

**Characteristics**:
- **Supervised learning**: Y (dependent variable) is known in advance
- **Greedy + non-backtracking**: once a split is made, it's permanent
- **Top-down, recursive divide-and-conquer**
- Very interpretable — even non-statisticians can understand the tree!

---

### CART Terminology

| Term | Description |
|------|-------------|
| **Root node** | Topmost node; represents entire dataset; the first split variable |
| **Internal node** | Intermediate split; tests an attribute |
| **Leaf node / Child node** | Terminal node; no further split; gives final class label |
| **Branch** | Outcome of a test on an attribute |

**Example** (Buys Computer dataset):
- Root node: **Age** (youth, middle-aged, senior)
- Internal node: **Student** (yes, no)
- Leaf node: **Yes** / **No** (buys computer or not)

---

### Decision Tree Algorithm: Termination Conditions (Stop when...)

1. All tuples in the partition belong to **same class** → leaf = that class
2. **No remaining attributes** → use **majority voting** to assign class
3. **Partition is empty** (no tuples for a branch) → leaf = majority class of parent

**Process at each node**:
1. Apply attribute selection measure → choose best splitting attribute
2. Label node with that attribute
3. For each value of the attribute → grow a sub-tree
4. Recursively repeat for each sub-tree

---

### Types of Splits

| Attribute type | Split type | Example |
|----------------|-----------|---------|
| Discrete (multiway allowed) | All values of attribute | Age → youth / middle-aged / senior |
| Continuous | Binary: ≤ split point vs > split point | Income ≤ 42,000 / > 42,000 |
| Discrete (binary forced) | Binary: belongs to subset SA or not | Income ∈ {low, medium} / high |

> 🔑 Gini index → **only binary splits**!
> 🔑 Information gain → **multiway splits** allowed!

---

### Attribute Selection Measures (Overview)

| Measure | Selects attribute with... | Notes |
|---------|--------------------------|-------|
| **Information Gain** | HIGHEST gain | Biased toward many-valued attributes |
| **Gain Ratio** | HIGHEST ratio | Corrects info-gain bias |
| **Gini Index** | LOWEST impurity | Used by CART; always binary split |

> 🔑 CART algorithm uses **Gini Index** as attribute selection measure!

---

### Tree Pruning

**Why prune?** Trees built from training data may overfit (noise/outliers in data).

| Method | How | When |
|--------|-----|------|
| **Pre-pruning** | Stop growing tree early | During construction (threshold on info gain, Gini, statistical tests) |
| **Post-pruning** | Remove branches from fully grown tree | After construction → replace subtree with leaf = majority class |

**Post-pruning rule**: If subtree is removed, new leaf = **most frequent class** in that subtree.
**Benefit**: Smaller, simpler, more accurate on new data!

---

## 📌 LECTURE 58 — Attribute Selection: Information Gain (Detailed Calculation)

### Information Gain: Formulas

**Step 1: Entropy of class (Info D)**
`Info(D) = −Σᵢ pᵢ × log₂(pᵢ)`

where pᵢ = probability of class i = |Cᵢ| / |D|

**Step 2: Expected info after splitting on attribute A**
`Info_A(D) = Σⱼ (|Dⱼ| / |D|) × Info(Dⱼ)`

This is a weighted average of entropies for each partition Dⱼ.

**Step 3: Information Gain**
`Gain(A) = Info(D) − Info_A(D)`

**Choose attribute with MAXIMUM Gain!**

---

### Worked Example: Buys Computer Dataset (14 tuples: 9 Yes, 5 No)

**Dataset entropy (class)**:
`Info(D) = −(9/14)log₂(9/14) − (5/14)log₂(5/14) = **0.940 bits**`

**Entropy for attribute Age (3 levels)**:

| Level | Count | Yes | No | Entropy |
|-------|-------|-----|----|---------|
| Youth | 5 | 2 | 3 | −(2/5)log₂(2/5) − (3/5)log₂(3/5) = 0.971 |
| Middle-aged | 4 | 4 | 0 | −(4/4)log₂(4/4) − 0 = **0** (pure!) |
| Senior | 5 | 3 | 2 | −(3/5)log₂(3/5) − (2/5)log₂(2/5) = 0.971 |

`Info_Age(D) = (5/14)×0.971 + (4/14)×0 + (5/14)×0.971 = **0.694**`
`Gain(Age) = 0.940 − 0.694 = **0.246 bits** ← HIGHEST`

**Information gains for all attributes**:

| Attribute | Info_A(D) | Gain(A) |
|-----------|----------|---------|
| **Age** | 0.694 | **0.246** ← Winner! |
| Income | 0.911 | 0.029 |
| Student | 0.789 | 0.151 |
| Credit Rating | 0.892 | 0.048 |

→ **Age** has highest information gain → **Age is the root node!**

**Decision**: Middle-aged → all YES → stop (leaf node = YES)
For Youth and Senior → recurse on remaining attributes {Income, Student, Credit Rating}

---

## 📌 LECTURE 59 — Attribute Selection: Gain Ratio & Gini Index

### Gain Ratio (Corrects Info-Gain Bias)

**Problem with Info Gain**: Attributes with many distinct values (e.g., product ID) always give maximum gain → useless for classification!

**Solution: Split Information**
`SplitInfo_A(D) = −Σⱼ (|Dⱼ|/|D|) × log₂(|Dⱼ|/|D|)`

**Gain Ratio**:
`GainRatio(A) = Gain(A) / SplitInfo(A)`

**Choose attribute with MAXIMUM Gain Ratio!**

**Example (Income):**
- Gain(Income) = 0.029
- SplitInfo(Income) = −(4/14)log₂(4/14) − (6/14)log₂(6/14) − (4/14)log₂(4/14) = **0.926**
- GainRatio(Income) = 0.029 / 0.926 = **0.031**

Apply same formula to all attributes → choose the one with **max GainRatio**.

---

### Gini Index: Theory

**Gini index of dataset D**:
`Gini(D) = 1 − Σᵢ pᵢ²`

For Buys Computer (9 Yes, 5 No):
`Gini(D) = 1 − (9/14)² − (5/14)² = **0.459**`

**Gini for a binary split** (D split into D1, D2):
`Gini_A(D) = (|D1|/|D|) × Gini(D1) + (|D2|/|D|) × Gini(D2)`

**Reduction in impurity (ΔGini)**:
`ΔGini(A) = Gini(D) − Gini_A(D)`

**Choose attribute with MAXIMUM ΔGini (≡ MINIMUM Gini_A(D))!**

---

### Gini Index for Income (All Possible Binary Splits)

Income has 3 levels: low, medium, high → try all 3 binary groupings:

| Split | D1 | D2 | Gini_A(D) |
|-------|----|----|-----------|
| {low, medium} vs {high} | D1:10 tuples (7Y,3N); D2:4 tuples (2Y,2N) | | **0.443** ← minimum! |
| {high, medium} vs {low} | D1:10 tuples (6Y,4N); D2:4 tuples (3Y,1N) | | 0.450 |
| {high, low} vs {medium} | D1:8 tuples (5Y,3N); D2:6 tuples (4Y,2N) | | 0.458 |

**Best split for Income**: {low, medium} vs {high} (Gini = 0.443)

**Gini values for all attributes** (at root):

| Attribute | Min Gini_A(D) | ΔGini |
|-----------|-----------|-------|
| **Age** | **0.357** | 0.102 ← Winner (min Gini! = max reduction) |
| Student | 0.367 | 0.092 |
| Income | 0.443 | 0.016 |
| Credit Rating | 0.428 | 0.031 |

→ **Age** chosen as root (minimum Gini index = 0.357)

---

## 📌 LECTURE 60 — CART III: Python Implementation & Output Interpretation

### Python: Building the CART Model

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import LabelEncoder
from sklearn.tree import DecisionTreeClassifier, export_graphviz
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# 1. Load data
data = pd.read_excel('buys_computer.xlsx')

# 2. Encode categorical → numerical
le_age = LabelEncoder(); le_income = LabelEncoder()
le_student = LabelEncoder(); le_credit = LabelEncoder()
le_buy = LabelEncoder()

data['age_n'] = le_age.fit_transform(data['age'])
data['income_n'] = le_income.fit_transform(data['income'])
data['student_n'] = le_student.fit_transform(data['student'])
data['credit_n'] = le_credit.fit_transform(data['credit_rating'])
data['buys_n'] = le_buy.fit_transform(data['buys_computer'])

# 3. Drop original text columns
data = data.drop(columns=['age','income','student','credit_rating','buys_computer'])

# 4. Define X and y
X = data[['age_n', 'income_n', 'student_n', 'credit_n']]
y = data['buys_n']

# 5. Build CART (without split) — train on ALL data
clf = DecisionTreeClassifier()
dt = clf.fit(X, y)

# 6. With train/test split (75/25)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=0)
clf2 = DecisionTreeClassifier()
clf2.fit(X_train, y_train)
accuracy = accuracy_score(y_test, clf2.predict(X_test))
print(f'Accuracy: {accuracy}')  # 0.75 (75% accurate)
```

> 🔑 `LabelEncoder().fit_transform(column)` → converts text labels to integers
> 🔑 `DecisionTreeClassifier()` builds CART using Gini index by default!
> 🔑 Train/test split: 75% training, 25% testing → accuracy = 0.75

---

### Label Encoding Scheme (Critical for Interpreting Output!)

| Variable | Original | Encoded |
|----------|----------|---------|
| Age | youth=2, middle-aged=0, senior=1 | |
| Income | high=0, low=1, medium=2 | |
| Student | no=0, yes=1 | |
| Credit Rating | excellent=0, fair=1 | |
| Buys Computer | no=0, yes=1 | |

**How to interpret CART output nodes**:
- `age_n <= 0.5` → True = middle-aged (coded 0); False = youth/senior
- `student_n <= 0.5` → True = No; False = Yes
- `age_n <= 1.5` → True = senior (coded 1); False = youth (coded 2)
- `credit_n <= 0.5` → True = excellent (coded 0); False = fair (coded 1)
- `income_n <= 1.5` → True = high or low (0 or 1 ≤ 1.5); False = medium (2)

**Node color coding**:
- 🔵 **Blue box** = favorable → class = 1 (buys computer = **Yes**)
- 🟠 **Orange box** = unfavorable → class = 0 (does NOT buy)
- ⬜ **White box** = intermediate → need further splitting

**Values in each box** (e.g., [5, 9]):
- First number = count of class 0 (No)
- Second number = count of class 1 (Yes)

---

### Decision Tree Interpretation (Full Tree Without Split)

Start at Root:
```
Age_n <= 0.5 (is it middle-aged?)
├── TRUE (middle-aged): ALL say Yes → LEAF = YES ✅
└── FALSE (youth or senior):
    └── Student_n <= 0.5 (is student = No?)
        ├── TRUE (student = No):
        │   └── Age_n <= 1.5 (is senior?)
        │       ├── TRUE (senior): look at credit_rating
        │       └── FALSE (youth): LEAF = No ❌
        └── FALSE (student = Yes):
            └── Credit_n <= 0.5 (is credit = excellent?)
                ├── TRUE (excellent): look at income
                │   └── Income_n <= 1.5 (high or low?)
                │       ├── TRUE (high/low): LEAF = No ❌
                │       └── FALSE (medium): LEAF = Yes ✅
                └── FALSE (fair): LEAF = Yes ✅
```

---

## 🎯 Week 12 Assignment — Key Questions & Answers

| Q | Answer | Explanation |
|---|--------|-------------|
| Q1 | Attribute selection method chooses best split | Determines attribute (variable) best separating classes using info gain / gain ratio / Gini |
| Q2 | CART uses top-down greedy approach | Divide-and-conquer; non-backtracking; once split → permanent |
| Q3 | Gini index used for classification | Measures impurity; minimum Gini → best split |
| Q4 | Info gain biased toward many-valued attributes | ID-like attributes give many pure partitions → spuriously high gain |
| Q5 | Gain ratio removes info-gain bias | Normalizes by SplitInfo: GainRatio = Gain / SplitInfo |
| Q6 | Gini index ensures binary splits | CART with Gini → always 2 branches (binary classif.) |
| Q7 | HAC = Hierarchical Agglomerative Clustering | Bottom-up; merges step by step; uses distance matrix |
| Q8 | Euclidean distance measures dissimilarity | Smaller = more similar; larger = more different |
| Q9 | LabelEncoder encodes categorical variables | Converts text labels (yes/no) → integers (0/1) for ML |
| Q10 | fit_transform() fits and transforms in one step | Learns encoding rules (fit) + applies them (transform) simultaneously |

---

## 💡 Final Quick-Fire Review Checklist

### Hierarchical Clustering (AGNES)
- [ ] Bottom-up: each object → own cluster → merge → 1 final cluster
- [ ] Single linkage: d(Ci,Cj) = **min** distance (nearest neighbour; elongated clusters)
- [ ] Complete linkage: d(Ci,Cj) = **max** distance (farthest neighbour; compact clusters)
- [ ] Average linkage: **compromise** (less sensitive to outliers)
- [ ] HAC algorithm: compute distance matrix → find minimum → merge → update matrix → repeat
- [ ] Dendrogram: tree diagram; cut to get desired k; no need to pre-specify k
- [ ] Python: `linkage(data, 'single')` → `dendrogram(linked)` → `AgglomerativeClustering(n_clusters=k)`

### CART — Decision Tree
- [ ] Supervised; categorical Y → Classification; continuous Y → Regression
- [ ] Root (top) → internal nodes (splits) → leaf nodes (final class)
- [ ] Attribute selected via: Info Gain (max), Gain Ratio (max), Gini Index (min)
- [ ] CART uses **Gini Index** → always **binary splits**
- [ ] Info gain chooses **highest gain**; biased toward many-valued attributes (e.g., ID)
- [ ] Gain ratio = Gain / SplitInfo → corrects info gain bias; choose **highest**
- [ ] Gini(D) = 1 − Σpᵢ² ; Gini_A(D) = weighted average of sub-partition Ginis
- [ ] Gini reduces to 0 when partition is PURE (same class, max entropy!)
- [ ] Termination: all same class; no more attributes; empty partition → majority vote
- [ ] **Pre-pruning**: stop early; **Post-pruning**: remove branches → leaf = majority class

### Formulas Summary

| Formula | Use |
|---------|-----|
| `Info(D) = −Σpᵢ log₂(pᵢ)` | Entropy of dataset (→ lower is purer) |
| `Info_A(D) = Σ(|Dⱼ|/|D|) × Info(Dⱼ)` | Expected info after splitting on A |
| `Gain(A) = Info(D) − Info_A(D)` | Information gain; choose attribute with **MAX** |
| `SplitInfo_A = −Σ(|Dⱼ|/|D|) log₂(|Dⱼ|/|D|)` | Penalty for many-valued attributes |
| `GainRatio(A) = Gain(A) / SplitInfo(A)` | Gain ratio; choose attribute with **MAX** |
| `Gini(D) = 1 − Σpᵢ²` | Gini impurity; 0=pure, 0.5=max impurity (binary) |
| `Gini_A(D) = Σ(|Dⱼ|/|D|) × Gini(Dⱼ)` | Weighted Gini after split; choose attribute with **MIN** |
| `ΔGini(A) = Gini(D) − Gini_A(D)` | Reduction in impurity; choose **MAX ΔGini** |

### Python Quick Reference
```python
# Hierarchical Clustering
from scipy.cluster.hierarchy import linkage, dendrogram
from sklearn.cluster import AgglomerativeClustering
z = linkage(data, method='single')  # or 'complete', 'average', 'ward'
AgglomerativeClustering(n_clusters=2, metric='euclidean', linkage='single').fit_predict(data)

# CART
from sklearn.preprocessing import LabelEncoder
from sklearn.tree import DecisionTreeClassifier
le = LabelEncoder(); col_encoded = le.fit_transform(col)
clf = DecisionTreeClassifier()  # Gini by default; criterion='entropy' for info gain
clf.fit(X_train, y_train)
accuracy = clf.score(X_test, y_test)  # or accuracy_score(y_test, clf.predict(X_test))

# Train-test split
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=0)
```

---

## 🎓 Course Complete! — All 12 Weeks Summary Map

| Week | Topics |
|------|--------|
| **1** | Intro to data analytics, types of data, scales of measurement |
| **2** | Probability, distributions (Binomial, Poisson, Normal), Central Limit Theorem |
| **3** | Sampling distributions, confidence intervals (Z, T, proportion) |
| **4** | Hypothesis testing, Type I/II errors, 2-sample tests (Z, T, paired T) |
| **5** | One-Way ANOVA, F-test, post-hoc (Tukey, Bonferroni), sample size |
| **6** | RBD (Randomized Block Design), Two-Way ANOVA, Simple Linear Regression |
| **7** | Regression (CI/PI, residuals, multiple regression, dummy variables) |
| **8** | MLE, Logistic Regression (odds ratio, logit, sigmoid function) |
| **9** | Confusion matrix, ROC/AUC, curvilinear regression, interaction terms, log Y |
| **10** | Chi-Square (independence, goodness of fit), Cluster Analysis intro, distances |
| **11** | Similarity/dissimilarity, variable types for clustering, K-Means, Hierarchical intro |
| **12** | HAC (agglomerative example + Python), CART (info gain, gain ratio, Gini, Python) |
