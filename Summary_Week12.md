# ⚡ Week 12 SUMMARY — Data Analytics with Python
## Lectures 56–60 | HAC Example, CART (Decision Trees)

---

### 📍 HAC (AGNES) Algorithm
1. Compute n×n distance matrix
2. Find **minimum** distance → merge those 2 objects
3. Update matrix (single linkage: new d = **min** of two old distances)
4. Repeat until 1 cluster

**7-Object Example (single linkage, key merges):**
- Step 1: {1}+{4} at d=**0.71**
- Step 2: {2}+{3} at d=**1.12**
- Step 3: {1,4}+{5} at d=**1.41**
- Step 4: {2,3}+{7} at d=**1.68**
- Cut at d=4 → 2 clusters: {1,4,5} and {2,3,6,7}

---

### 📍 CART Introduction
| | Classification | Regression |
|-|---------------|-----------|
| **Y** | Categorical | Continuous |

- **Supervised**; top-down; greedy; non-backtracking
- Root node → Internal nodes → Leaf nodes

**Termination:** all same class | no attributes left | empty partition → majority vote

**Splits:**
- Gini index → **binary splits only!**
- Information gain → multiway splits allowed

---

### 📍 Attribute Selection Measures
| Measure | Choose | Biased? |
|---------|--------|---------|
| **Information Gain** | Maximum gain | ✅ Biased toward many-valued attributes |
| **Gain Ratio** | Maximum ratio | ✅ Corrects info-gain bias |
| **Gini Index** | Minimum Gini | Used by CART; binary splits |

> CART uses **Gini Index** by default!

---

### 📍 Formulas
| Formula | Use |
|---------|-----|
| `Info(D) = −Σpᵢ log₂(pᵢ)` | Entropy (0=pure, 1=max impurity binary) |
| `Info_A(D) = Σ(|Dⱼ|/|D|)×Info(Dⱼ)` | Weighted entropy after split |
| `Gain(A) = Info(D) − Info_A(D)` | **MAX** → best attribute |
| `SplitInfo_A = −Σ(|Dⱼ|/|D|)log₂(|Dⱼ|/|D|)` | Penalty for large # of values |
| `GainRatio = Gain/SplitInfo` | **MAX** → best attribute |
| `Gini(D) = 1 − Σpᵢ²` | **MIN** → best attribute |
| `Gini_A(D) = Σ(|Dⱼ|/|D|)×Gini(Dⱼ)` | Weighted Gini |

**Worked Numbers (14 tuples: 9Y, 5N):**
- Info(D) = **0.940** bits
- Gain(Age)=**0.246** wins over Income=0.029, Student=0.151, Credit=0.048
- Gini(D) = 1−(9/14)²−(5/14)² = **0.459**
- Min Gini_A(D) for Age = **0.357** → Age is root

---

### 📍 Pruning
| | Pre-pruning | Post-pruning |
|-|-------------|-------------|
| When | During build | After build |
| How | Stop early | Replace subtree with leaf |
| Leaf class | — | majority class in subtree |

---

### 📍 Python Quick Ref
```python
from sklearn.preprocessing import LabelEncoder
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import linkage, dendrogram

# HAC
linked = linkage(data, method='single')  # 'complete', 'average', 'ward'
dendrogram(linked)
AgglomerativeClustering(n_clusters=2, metric='euclidean', linkage='single').fit_predict(data)

# CART
le = LabelEncoder()
col_encoded = le.fit_transform(col)    # text → int
clf = DecisionTreeClassifier()          # Gini by default!
# criterion='entropy' for Info Gain
clf.fit(X_train, y_train)
accuracy_score(y_test, clf.predict(X_test))

# Label encoding scheme (Buys Computer):
# age: middle-aged=0, senior=1, youth=2
# income: high=0, low=1, medium=2
# student: no=0, yes=1; credit: excellent=0, fair=1
```
