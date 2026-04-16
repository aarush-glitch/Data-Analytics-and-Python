# ⚡ Week 11 SUMMARY — Data Analytics with Python
## Lectures 51–55 | Similarity/Dissimilarity, Variable Types, K-Means & HAC

---

### 📍 Dissimilarity vs Similarity
| | d(i,j) | s(i,j) |
|-|--------|--------|
| Range | ≥ 0 | 0 to 1 |
| Same object | 0 | 1 |
| Symmetry | Yes | Yes |
| Conversion | d = 1 − s | s = 1 − d |

- Correlation → dissimilarity: `d = (1 − R) / 2`
- Correlation → similarity: `s = (1 + R) / 2` OR `s = |R|`

---

### 📍 Binary Variables Table
|  | j=1 | j=0 |
|--|-----|-----|
| **i=1** | q | r |
| **i=0** | s | t |

| Type | d formula | Note |
|------|-----------|------|
| **Symmetric** (both 0/1 equally important) | (r+s)/(q+r+s+t) | Include t |
| **Asymmetric** (1 is rare/important) | **(r+s)/(q+r+s)** | **Exclude t!** |
| **Jaccard** (similarity) | q/(q+r+s) | Asymmetric |

---

### 📍 Variable Types for Clustering
| Type | Method |
|------|--------|
| **Categorical** | d = (p−m)/p; p=vars, m=matches |
| **Ordinal** | Rank → z=(rank−1)/(Mmax−1) → Euclidean on z |
| **Ratio** | Log-transform → treat as interval |
| **Mixed** | d = Σ(δ×d(f)) / Σδ; normalize interval to [0,1] using (max−min) |

---

### 📍 K-Means Clustering
**Algorithm:** Init k centroids → Assign (min distance) → Update (new mean) → Repeat until stable

- Criterion: Minimize E = Σ|p − mᵢ|² (SSE within clusters)
- Need to **pre-specify k**
- NOT reproducible (random initialization)
- Best for: large datasets, spherical clusters
- Groups **individuals (rows)** — vs Factor Analysis which groups variables!

**Fails when:** clusters of different sizes/densities, non-spherical shapes, outliers

---

### 📍 Hierarchical Clustering
| | AGNES (Agglomerative) | DIANA (Divisive) |
|-|-----------------------|-----------------|
| Direction | **Bottom-up** | Top-down |
| Start | n clusters | 1 cluster |
| End | 1 cluster | n clusters |
| Common? | **More common** | Less |

- Dendrogram: cut at any level → get k clusters (no need to pre-specify!)
- Once merged → CANNOT undo!

**Linkage Methods:**
| Method | Formula | Shape |
|--------|---------|-------|
| **Single (Min)** | min distance | Elongated/chain |
| **Complete (Max)** | max distance | Compact/globular |
| **Average** | compromise | — |

---

### 📍 K-Means vs Hierarchical
| Feature | K-Means | Hierarchical |
|---------|---------|-------------|
| k needed? | **YES** | NO (choose from dendrogram) |
| Reproducible? | **NO** | YES |
| Dataset size | Large | Small-medium |
| Mistakes fixable? | YES | **NO** |
| Diagram | Flat clusters | Dendrogram |

---

### 📍 Python Quick Ref
```python
from sklearn.cluster import KMeans, AgglomerativeClustering
from scipy.spatial import distance_matrix

# K-Means
km = KMeans(n_clusters=2, random_state=0).fit(data)
km.labels_; km.cluster_centers_

# Distance matrix (7×7)
dm = pd.DataFrame(distance_matrix(df.values, df.values), index=labels, columns=labels)

# HAC
from scipy.cluster.hierarchy import linkage, dendrogram
linked = linkage(data, method='single')  # 'complete', 'average', 'ward'
dendrogram(linked)
```
