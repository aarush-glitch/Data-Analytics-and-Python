# 📊 Week 11 — Data Analytics with Python (NPTEL)
### Lectures 51–55 | Topics: Similarity/Dissimilarity, Variable Types, K-Means, Hierarchical Clustering

---

## 📌 LECTURE 51 — Cluster Analysis III: Missing Data, Similarity & Dissimilarity

### Handling Missing Data in Clustering

**Missing value strategies**:

| Situation | Action |
|-----------|--------|
| ALL measurements missing for ONE object | **Delete the object** |
| ALL measurements missing for ONE variable | **Delete the variable** |
| Partial missing values | Compute MAD/mean using **only present values**; replace n with non-missing count |

**Distance computation with missing values**:
When calculating d(i, j), only consider features where BOTH objects have measurements.
Adjust by: `d(i,j) = (p / actual_terms) × Σ(x_if − x_jf)²` (scale by p/actual count before √)

**Imputation option**: Replace all missing xif with the column mean mf → then all distances computable.

---

### Dissimilarity vs Similarity

| | Dissimilarity d(i,j) | Similarity s(i,j) |
|--|---|---|
| **Range** | d ≥ 0 | 0 ≤ s ≤ 1 |
| **Same object** | d(i,i) = 0 | s(i,i) = 1 |
| **Symmetry** | d(i,j) = d(j,i) | s(i,j) = s(j,i) |
| **Triangle inequality** | May NOT hold | Not required |
| **Low value means** | Objects are SIMILAR | Objects are NOT similar |
| **High value means** | Objects are DIFFERENT | Objects are similar |

**Conversion formula**:
`d(i,j) = 1 − s(i,j)` (larger similarity → smaller dissimilarity)

---

### Using Correlation as Dissimilarity/Similarity

**Pearson correlation** rₚ (parametric) — linear relationships
**Spearman correlation** rₛ (non-parametric) — monotone relationships; for ordinal data

Both range: −1 to +1 (independent of units!)

**Convert correlation → dissimilarity**:
`d(f,g) = (1 − R(f,g)) / 2`

**Convert correlation → similarity** (two options):

| Formula | When to use |
|---------|-------------|
| `s(f,g) = (1 + R(f,g)) / 2` | Negatively correlated variables are DIFFERENT (e.g., mileage vs weight) |
| `s(f,g) = |R(f,g)|` | Negatively correlated variables are SIMILAR (direction irrelevant; use when selecting 1 variable per cluster for regression) |

> 🔑 Example: dissimilarity matrix → math and CS most similar (value = 1.43); psychology and astronomy most different

---

### Binary Variables: Symmetric vs Asymmetric

**Contingency table for binary object pairs**:

|  | j = 1 | j = 0 |
|--|-------|-------|
| **i = 1** | q (both 1) | r (i=1, j=0) |
| **i = 0** | s (i=0, j=1) | t (both 0) |

Total: p = q + r + s + t

| Type | Example | Dissimilarity Formula |
|------|---------|----------------------|
| **Symmetric** | Gender (0=M, 1=F) — both states equally important | `d(i,j) = (r + s) / (q + r + s + t)` |
| **Asymmetric** | HIV test (1=positive, rare) — 1 is more meaningful | `d(i,j) = (r + s) / (q + r + s)` ← **no t in denominator!** |

**Jaccard Coefficient** (similarity for asymmetric binary):
`s(i,j) = q / (q + r + s) = 1 − d(i,j)`

> 🔑 Asymmetric: t (both=0) is EXCLUDED because "double negatives" are NOT informative!
> 🔑 Symmetric: t (both=0) IS included — both states are equally important

---

### Worked Example: Jack, Mary, Jim (Asymmetric Binary)

Variables: fever, cough, test1, test2, test3, test4 (Y=1, N=0; gender excluded = symmetric → not used)

| Pair | q | r | s | t | d = (r+s)/(q+r+s) |
|------|---|---|---|---|--------------------|
| **Jack & Mary** | 2 | 1 | 0 | 3 | (1+0)/(2+1+0) = **0.33** |
| **Jack & Jim** | 1 | 1 | 1 | 3 | (1+1)/(1+1+1) = **0.67** |
| **Jim & Mary** | 1 | 2 | 1 | 2 | (2+1)/(1+2+1) = **0.75** |

→ Jack & Mary most similar (d=0.33); Jim & Mary most dissimilar (d=0.75)

---

## 📌 LECTURE 52 — Cluster Analysis IV: Categorical, Ordinal, Ratio & Mixed Data

### Categorical Variables

**Definition**: More than 2 states; no ordering (e.g., map colors: red, yellow, green, purple, blue)

**Dissimilarity formula**: `d(i,j) = (p − m) / p`
- p = total number of variables (features)
- m = number of **matches** (variables where i and j have same state)

**Example** (p=1, one categorical var, test-1):
- Objects 1 and 2: codes A vs B → m=0 → d(2,1) = (1−0)/1 = **1** (mismatch)
- Objects 1 and 4: both code A → m=1 → d(4,1) = (1−1)/1 = **0** (match)

---

### Ordinal Variables

**Definition**: Multiple ordered states (e.g., rank 1=fair, 2=good, 3=excellent); order matters but magnitude doesn't

**Steps to compute dissimilarity**:

**Step 1**: Assign rank rᵢf (1 to Mf where Mf = max number of states)
- fair → 1, good → 2, excellent → 3

**Step 2**: Standardize (map to 0–1 scale):
`zᵢf = (rᵢf − 1) / (Mf − 1)`

| Rank | Formula | z-value |
|------|---------|---------|
| 1 | (1−1)/(3−1) | **0** |
| 2 | (2−1)/(3−1) | **0.5** |
| 3 | (3−1)/(3−1) | **1.0** |

**Step 3**: Use Euclidean distance on the **z-values**

---

### Ratio-Scaled Variables

**Definition**: Positive measurements on nonlinear (exponential) scale; absolute zero exists
Examples: bacterial growth (Ae^(Bt)), radioactive decay, population growth

**Three methods for dissimilarity**:

| Method | Approach | When |
|--------|----------|------|
| 1 | Treat as interval-scaled | Quick but may distort scale |
| **2** | **Log transform**: yᵢf = log(xᵢf), then treat as interval | **Most common and effective** |
| 3 | Treat as ordinal → rank → standardize | Alternative |

**Example**:
- Raw values: 445, 22, 164, 1210 → After log: 2.65, 1.34, 2.21, 3.08
- Use Euclidean distance on log-transformed values: d(2,1) = |2.65 − 1.34| = 1.31

---

### Mixed Type Variables: Combined Dissimilarity

When dataset has multiple types (interval + binary + categorical + ordinal + ratio) together:

**Combined formula**:
`d(i,j) = [Σf δ(f)ᵢⱼ × d(f)ᵢⱼ] / [Σf δ(f)ᵢⱼ]`

where δ(f)ᵢⱼ = **weight** for feature f:
- δ = 0 if: value is missing, both values are 0 AND variable is asymmetric binary
- δ = 1 otherwise

**Per-feature contribution d(f)ᵢⱼ**:

| Variable type | d(f)ᵢⱼ formula |
|--------------|---------------|
| Interval-scaled | `|xᵢf − xⱼf| / (max − min)` (normalize to 0–1!) |
| Binary/Categorical | 0 if same, 1 if different |
| Ordinal | Convert to z (0–1 scale), use interval formula |
| Ratio | Log-transform, then use interval formula |

> 🔑 Interval variables in mixed type MUST be normalized to 0–1 using (max−min) — different from pure interval clustering!
> 🔑 Final matrix d(i,j) = weighted average of all per-feature dissimilarities

**Example** (p=3: categorical + ordinal + ratio):
- d(2,1): categorical=1, ordinal=1, ratio=0.75 → combined = (1+1+0.75)/3 = **0.92**
- d(4,1): lowest combined → objects 1 and 4 most similar
- d(2,4): highest combined = 1 → most dissimilar

---

## 📌 LECTURE 53 — Cluster Analysis V: Mixed Type Example + Python Distance Demo

### Python: Computing Distances

```python
from scipy.spatial import distance
import numpy as np
import pandas as pd

# Two points
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

# Euclidean distance
dst_e = distance.euclidean(a, b)      # = 5.196

# Minkowski distance (p=1 → Manhattan, p=2 → Euclidean)
dst_m1 = distance.minkowski(a, b, 1)  # Manhattan = 9
dst_m2 = distance.minkowski(a, b, 2)  # Euclidean = 5.196 (same!)
dst_m3 = distance.minkowski(a, b, 3)  # Minkowski p=3 = 4.32
```

### Python: Distance Matrix for 8 Objects

```python
from scipy.spatial import distance_matrix
import pandas as pd

# Height-Weight data for 8 people
data = [[15,95], [49,156], [49,154], [50,161], [85,178], [66,176], [49,155], [70,175]]
labels = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
df = pd.DataFrame(data, columns=['Weight', 'Height'], index=labels)

# Compute full n×n distance matrix
dm = pd.DataFrame(distance_matrix(df.values, df.values), index=labels, columns=labels)
dm_rounded = dm.round(decimals=1)
# Diagonal = 0; symmetric (upper triangle = lower triangle)
# d(B,A) = d(A,B) = 69.83; d(C,A) = 2.0 (closest pair!)
```

> 🔑 `distance_matrix(df.values, df.values)` → computes all pairwise Euclidean distances
> 🔑 Minkowski with p=1 → Manhattan; p=2 → Euclidean; same function, different p!

---

## 📌 LECTURE 54 — K-Means Clustering

### Classification of Clustering Methods

```
Clustering Methods
├── Partitioning Methods
│   ├── K-Means  ← centroid-based
│   └── K-Medoids
└── Hierarchical Methods
    ├── Agglomerative (bottom-up)
    └── Divisive (top-down)
```

| Method | Use when |
|--------|----------|
| **K-Means** | Know k in advance; large datasets; spherical clusters |
| **Hierarchical** | Don't know k; want a dendrogram; small-medium datasets |

---

### K-Means Algorithm: Steps

**Input**: k (number of clusters), dataset of n objects
**Output**: k clusters with centroids

**Algorithm**:
1. **Initialize**: Randomly choose k objects as initial centroids (C1, C2, ..., Ck)
2. **Assign**: For each object, compute distance to ALL centroids → assign to **nearest centroid** (min distance)
3. **Update**: Recompute centroid of each cluster = mean of all objects in that cluster
4. **Repeat** steps 2–3 until no object changes cluster (convergence)

**Criterion function (SSE — Sum of Squared Errors)**:
`E = Σᵢ₌₁ᵏ Σₚ∈Cᵢ |p − mᵢ|²`
- p = data point; mᵢ = centroid of cluster i
- **Minimize E** → compact, well-separated clusters

---

### K-Means: Numerical Example (k=2, 7 individuals)

| Individual | Var1 | Var2 |
|-----------|------|------|
| 1 | 1.0 | 1.0 |
| 2 | 1.5 | 2.0 |
| 3 | 3.0 | 4.0 |
| 4 | 5.0 | 7.0 |
| 5 | 3.5 | 5.0 |
| 6 | 4.5 | 5.0 |
| 7 | 3.5 | 4.5 |

**Initial centroids** (randomly pick individuals 1 and 3):
- K1 = (1.0, 1.0); K2 = (3.0, 4.0)

**Euclidean distance**: `d = √[(x₂−x₁)² + (y₂−y₁)²]`

**Iteration process** (abbreviated):

| Individual | d to K1 | d to K2 | Assigned to |
|-----------|---------|---------|-------------|
| 2 (1.5,2) | 1.12 | 2.50 | **K1** |
| Update K1 centroid → (1.25, 1.5) | | | |
| 4 (5,7) | 6.66 | 3.61 | **K2** |
| Update K2 centroid → (4.0, 5.5) | | | |
| 5 (3.5,5) | 4.16 | 0.71 | **K2** |
| ... and so on | | | |

**Final result**:
- **Cluster 1** (K1): Individuals 1, 2 → Centroid = **(1.25, 1.5)**
- **Cluster 2** (K2): Individuals 3, 4, 5, 6, 7 → Centroid = **(3.9, 5.1)**

---

### K-Means Python

```python
from sklearn.cluster import KMeans
import pandas as pd
import matplotlib.pyplot as plt

data = pd.DataFrame({
    'Var1': [1, 1.5, 3, 5, 3.5, 4.5, 3.5],
    'Var2': [1, 2, 4, 7, 5, 5, 4.5]
})

# K-Means with k=2
kmeans = KMeans(n_clusters=2, random_state=0)
kmeans.fit(data)

# Labels (which cluster each point belongs to)
labels = kmeans.labels_          # e.g., [0, 0, 1, 1, 1, 1, 1]

# Centroids
centroids = kmeans.cluster_centers_
# Cluster 0 centroid: [1.25, 1.5]
# Cluster 1 centroid: [3.9, 5.1]

# For k=3:
kmeans3 = KMeans(n_clusters=3)
kmeans3.fit(data2)
# Centroids: (7, 4.3), (3.6, 9), (1.5, 3.5)

# Visualize
plt.scatter(data['Var1'], data['Var2'], c=labels)
plt.scatter(centroids[:, 0], centroids[:, 1], marker='*', s=200)
plt.show()
```

> 🔑 K-Means vs Factor Analysis — K-Means groups **individuals (rows)**; Factor Analysis groups **variables (columns)**!
> 🔑 K-Means results may differ on re-runs (random initialization) — not reproducible!
> 🔑 K-Means works best with **spherical/globular** clusters

---

## 📌 LECTURE 55 — Hierarchical Clustering I

### Types of Hierarchical Methods

| | Agglomerative (AGNES) | Divisive (DIANA) |
|--|---|---|
| **Direction** | **Bottom-up** | Top-down |
| **Start** | Each object = its own cluster (n clusters) | All objects in 1 cluster |
| **Process** | Merge smallest-distance pairs step by step | Split into smaller clusters step by step |
| **End** | 1 mega-cluster | n individual clusters |
| **Common?** | **More common** | Less common |

**Once merged/split → CANNOT undo!** (biggest limitation of hierarchical methods)

---

### Dendrogram

A **tree diagram** showing hierarchical clustering process:
- **X-axis**: Objects/clusters
- **Y-axis**: Similarity (or distance) scale at which merge occurred
- At level 0 → each object = separate cluster
- At level 4 (for 5 objects) → all merged into 1

```
Level 0:  a   b   c   d   e       ← 5 separate clusters
Level 1: [a,b]  c   d   e         ← a and b merged
Level 2: [a,b]  c  [d,e]          ← d and e merged
Level 3: [a,b] [c,d,e]            ← c joins {d,e}
Level 4: [a,b,c,d,e]              ← single cluster
```

> 🔑 User can CUT the dendrogram at any level to get desired k clusters
> 🔑 No need to pre-specify k (unlike K-Means)

---

### Distance Measures Between Clusters (Linkage Methods)

| Linkage | Formula | Also called | Best cluster shape |
|---------|---------|-------------|---------------------|
| **Single (Min)** | d(Cᵢ,Cⱼ) = min distance between any pair | Nearest neighbour | Elongated/chain |
| **Complete (Max)** | d(Cᵢ,Cⱼ) = max distance between any pair | Farthest neighbour | Compact, globular |
| **Mean** | d(Cᵢ,Cⱼ) = distance between cluster means | — | — |
| **Average** | d(Cᵢ,Cⱼ) = (1/nᵢnⱼ) Σ Σ |p − p'| | — | Compromise |

> 🔑 Single linkage → minimal spanning tree algorithm
> 🔑 Complete linkage → complete linkage algorithm
> 🔑 Single & Complete: robust to distance metric changes; Average: sensitive to metric
> 🔑 **Average linkage** = compromise; less sensitive to outliers than single/complete

---

### K-Means vs Hierarchical Clustering: Full Comparison

| Feature | K-Means | Hierarchical |
|---------|---------|-------------|
| **k required?** | YES — pre-specify k | NO — choose after seeing dendrogram |
| **Type** | Partitioning (non-hierarchical) | Hierarchical (nested clusters) |
| **Cluster shape** | Spherical/globular | Any shape (depends on linkage) |
| **Results reproducible?** | NO (random init) | YES |
| **Works with...** | Large datasets | Small-medium datasets |
| **Computation** | O(n) — efficient | O(n²) — stores n×n matrix |
| **Mistakes fixable?** | YES (reassign at each iteration) | NO (irreversible merges/splits) |
| **Diagram** | Flat clusters (non-overlapping) | Dendrogram (nested) |
| **Algorithm type** | Centroid-based | Linkage-based |

**Similarity** (both need):
- Distance/dissimilarity between 2 objects
- Distance/dissimilarity between 2 clusters
- Variety of metrics can be used

**K-Means fails when**:
- Clusters of different sizes
- Clusters of different densities
- Non-spherical cluster shapes
- Data contains outliers

**Hierarchical fails when**:
- Very large datasets (n×n matrix too big)
- Outliers present
- Data ordering changes results (low stability)

---

## 🎯 Week 11 Assignment — Key Questions & Answers

| Q | Question | Answer | Formula/Concept |
|---|----------|--------|----------------|
| Q1 | Property of dissimilarity | **Non-negative; increases with difference** | d ≥ 0; grows as objects differ |
| Q2 | If ALL values missing for a variable | **Remove the variable** | No info → delete |
| Q3 | Dissimilarity for categorical variables | **Ratio of mismatches: (p−m)/p** | p=vars, m=matches |
| Q4 | If all variables match (categorical) | **d = 0** | (p−p)/p = 0 |
| Q5 | Why normalize for mixed variables? | **Different variable scales need normalization** | Equal weight across all types |
| Q6 | Range of log values for ratio data? | **1.74** | max−min = 3.08−1.34 = 1.74 |
| Q7 | K-Means criterion function | **Sum of squared distances within clusters** | SSE = Σ|p − mᵢ|² |
| Q8 | d(i,j) for 4 vars, 3 matches (categorical) | **(4−3)/4 = 0.25** | d = (p−m)/p |
| Q9 | Normalized ratio dissimilarity (2,1) | **≈ 0.75** | |2.65−1.34|/1.74 = 1.31/1.74 ≈ 0.75 |
| Q10 | Point (2,3) closer to C1=(1,1) or C2=(5,5)? | **Cluster 1** | √5 ≈ 2.24 vs √13 ≈ 3.61 |

---

## 💡 Quick-Fire Review Checklist

**Dissimilarity/Similarity:**
- [ ] d(i,j) ≥ 0; d(i,i) = 0; symmetric; triangle inequality may NOT hold
- [ ] s(i,j): 0 to 1; s(i,i) = 1; symmetric
- [ ] Conversion: d(i,j) = 1 − s(i,j)
- [ ] Pearson = parametric (linear); Spearman = non-parametric (monotone, ordinal)
- [ ] Correlation → dissimilarity: d = (1 − R) / 2
- [ ] Correlation → similarity: s = (1 + R) / 2 OR s = |R|

**Binary Variables:**
- [ ] Symmetric (e.g., gender): d = (r+s)/(q+r+s+t) — include t (double negatives count!)
- [ ] Asymmetric (e.g., HIV test): d = (r+s)/(q+r+s) — EXCLUDE t!
- [ ] Jaccard coefficient: s = q/(q+r+s) for asymmetric
- [ ] q=both 1; r=i=1,j=0; s=i=0,j=1; t=both 0

**Variable Types:**
- [ ] Categorical: d = (p−m)/p; 0=perfect match, 1=total mismatch
- [ ] Ordinal: rank → z = (rank−1)/(Mmax−1) → Euclidean on z values
- [ ] Ratio: log-transform → treat as interval; or rank as ordinal
- [ ] Mixed: weighted average of per-feature dissimilarities; normalize interval to 0–1 with (max−min)!

**K-Means:**
- [ ] Pre-specify k; iterative; centroid = mean of cluster; assign to nearest centroid
- [ ] SSE criterion: E = Σ|p − mᵢ|² → minimize
- [ ] Results NOT reproducible (random init); best for spherical clusters
- [ ] Python: `KMeans(n_clusters=k).fit(data)` → `.labels_`, `.cluster_centers_`
- [ ] Groups INDIVIDUALS (rows), not variables! (vs factor analysis)

**Hierarchical Clustering:**
- [ ] Agglomerative (AGNES) = bottom-up; Divisive (DIANA) = top-down
- [ ] AGNES more common; start with n clusters, end with 1
- [ ] Dendrogram = tree diagram; cut at any level to get k clusters; no need to pre-specify k
- [ ] Linkage: Single=min distance (elongated); Complete=max (compact); Average=compromise
- [ ] Once merged → CANNOT undo! → may propagate errors
- [ ] Need n×n distance matrix → expensive for large datasets
- [ ] Reproducible results (unlike K-Means)
- [ ] Python: `distance_matrix(df.values, df.values)` → n×n symmetric matrix
