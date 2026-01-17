# Unsupervised Learning Fundamentals

## Q1: What is unsupervised learning and how does it differ from supervised learning?

**Answer:**
Unsupervised learning is a type of machine learning where the algorithm learns patterns from unlabeled data without explicit target outputs.

| Aspect | Supervised Learning | Unsupervised Learning |
|--------|--------------------|-----------------------|
| Labels | Requires labeled data | No labels needed |
| Goal | Predict output for new inputs | Discover hidden patterns/structure |
| Feedback | Direct error signal | No direct feedback |
| Examples | Classification, Regression | Clustering, Dimensionality Reduction |

**Follow-up Questions:**
- Why is unsupervised learning considered harder to evaluate?
  - Answer: No ground truth labels exist to measure accuracy; evaluation relies on internal metrics (silhouette score, inertia) or domain expertise
- Can unsupervised learning be used as a preprocessing step for supervised learning?
  - Answer: Yes, dimensionality reduction (PCA) can reduce features, and clustering can create new features or help with data understanding

---

## Q2: What are the main types of unsupervised learning tasks?

**Answer:**
1. **Clustering**: Grouping similar data points (K-Means, DBSCAN, Hierarchical)
2. **Dimensionality Reduction**: Reducing features while preserving information (PCA, t-SNE, UMAP)
3. **Anomaly Detection**: Identifying unusual data points (Isolation Forest, LOF)
4. **Association Rule Mining**: Finding relationships between items (Apriori, FP-Growth)
5. **Density Estimation**: Estimating probability distributions (GMM, KDE)

**Follow-up Questions:**
- Which type would you use for customer segmentation?
  - Answer: Clustering - it groups customers with similar behaviors/characteristics
- Which type helps with the curse of dimensionality?
  - Answer: Dimensionality reduction - it reduces feature space while retaining important information

---

## Q3: What are the key challenges in unsupervised learning?

**Answer:**
1. **No ground truth**: Cannot directly measure if the model is "correct"
2. **Choosing the right algorithm**: Different algorithms make different assumptions
3. **Hyperparameter selection**: Choosing k in K-Means, epsilon in DBSCAN without labels
4. **Interpretability**: Results may not have clear real-world meaning
5. **Scalability**: Many algorithms are computationally expensive on large datasets
6. **Evaluation**: Metrics like silhouette score may not align with actual usefulness

**Follow-up Questions:**
- How do you validate clustering results without labels?
  - Answer: Use internal metrics (silhouette, Davies-Bouldin), stability analysis, or domain expert evaluation
- What if different algorithms give different results?
  - Answer: This is common; choose based on data characteristics, domain knowledge, and which assumptions fit best

---

## Q4: When would you prefer unsupervised learning over supervised learning?

**Answer:**
Use unsupervised learning when:
- **Labels are unavailable or expensive** to obtain
- **Exploring data** to understand structure before modeling
- **Finding anomalies** in data (fraud, defects)
- **Reducing dimensionality** for visualization or preprocessing
- **Discovering natural groupings** without predefined categories
- **Data compression** while preserving important information

**Follow-up Questions:**
- Can you use both approaches together?
  - Answer: Yes, semi-supervised learning uses small labeled + large unlabeled data; unsupervised preprocessing can improve supervised models
- What's an example where labeling is prohibitively expensive?
  - Answer: Medical imaging (requires expert radiologists), genomics data, or large-scale web content categorization

---

## Q5: What assumptions do most clustering algorithms make?

**Answer:**
Common assumptions include:
- **Cluster shape**: K-Means assumes spherical clusters; DBSCAN assumes density-connected regions
- **Cluster size**: Some assume roughly equal-sized clusters
- **Feature scale**: Most are sensitive to feature scaling (need normalization)
- **Separability**: Clusters should be distinguishable in the feature space
- **Dimensionality**: Performance often degrades in very high dimensions (curse of dimensionality)

**Follow-up Questions:**
- What happens if your data violates these assumptions?
  - Answer: Algorithm may produce poor results; e.g., K-Means fails on non-spherical clusters, DBSCAN fails on varying density
- Why is feature scaling important?
  - Answer: Distance-based algorithms are dominated by features with larger scales; normalization ensures equal contribution

---

## Q6: Explain the curse of dimensionality in the context of unsupervised learning.

**Answer:**
The curse of dimensionality refers to problems that arise when working with high-dimensional data:

1. **Distance concentration**: All pairwise distances become similar, making distance-based methods ineffective
2. **Sparsity**: Data points become increasingly sparse; need exponentially more data to maintain density
3. **Computational cost**: Algorithms become slower as dimensions increase
4. **Overfitting**: Models may find spurious patterns in noise

**Mathematical insight**: In d dimensions, volume of a hypersphere approaches 0 relative to the enclosing hypercube as d→∞

**Follow-up Questions:**
- How can you mitigate the curse of dimensionality?
  - Answer: Use dimensionality reduction (PCA, feature selection), domain knowledge to select relevant features, or algorithms robust to high dimensions
- At what dimensionality does this become a problem?
  - Answer: Rule of thumb: problems often start when d > √n (n = sample size), but depends on data structure

---

## Q7: What is the difference between hard and soft clustering?

**Answer:**
| Hard Clustering | Soft Clustering |
|----------------|-----------------|
| Each point belongs to exactly one cluster | Each point has probability/degree of belonging to each cluster |
| Binary membership (0 or 1) | Continuous membership (0 to 1) |
| Example: K-Means | Example: GMM, Fuzzy C-Means |
| Simpler to interpret | Captures uncertainty |
| Can't handle overlapping clusters well | Better for overlapping clusters |

**Follow-up Questions:**
- When would you prefer soft clustering?
  - Answer: When clusters overlap, when uncertainty matters (e.g., medical diagnosis), or when a point could legitimately belong to multiple groups
- Can you convert soft clustering to hard clustering?
  - Answer: Yes, assign each point to the cluster with highest probability/membership

---

## Q8: How do you evaluate unsupervised learning models?

**Answer:**
**Internal Metrics** (no labels needed):
- **Silhouette Score**: Measures cohesion vs separation (-1 to 1, higher is better)
- **Davies-Bouldin Index**: Ratio of within-cluster to between-cluster distances (lower is better)
- **Inertia/SSE**: Sum of squared distances to cluster centers (lower is better, but decreases with more clusters)

**External Metrics** (if labels available):
- **Adjusted Rand Index (ARI)**: Similarity between predicted and true clusters
- **Normalized Mutual Information (NMI)**: Information shared between clusterings

**Practical Evaluation**:
- Domain expert review
- Downstream task performance
- Stability across runs/subsamples

**Follow-up Questions:**
- Why can't we just minimize inertia?
  - Answer: Inertia always decreases with more clusters; with k=n, inertia=0 but clustering is useless
- What's a good silhouette score?
  - Answer: >0.5 is reasonable, >0.7 is strong structure; but depends on data complexity
# Principal Component Analysis (PCA)

## Q1: What is PCA and what is its main purpose?

**Answer:**
PCA (Principal Component Analysis) is a linear dimensionality reduction technique that transforms data into a new coordinate system where:
- The first axis (PC1) captures maximum variance
- Each subsequent axis captures maximum remaining variance while being orthogonal to previous axes

**Main purposes:**
- Reduce dimensionality while preserving information
- Remove multicollinearity
- Visualization of high-dimensional data
- Noise reduction
- Feature extraction

**Follow-up Questions:**
- Why do we want to maximize variance?
  - Answer: Variance represents information; directions with low variance are likely noise or redundant
- Is PCA supervised or unsupervised?
  - Answer: Unsupervised - it doesn't use labels, only finds directions of maximum variance in features

---

## Q2: Explain the geometric intuition behind PCA.

**Answer:**
Geometrically, PCA finds the directions (axes) along which the data varies the most:

1. **First PC**: Line through the data cloud that minimizes perpendicular distances (equivalently, maximizes projected variance)
2. **Second PC**: Perpendicular to first, captures next most variance
3. **Data transformation**: Rotating the coordinate system to align with these principal directions

**Visual analogy**: If data forms an ellipsoid, PCA finds the axes of that ellipsoid, with PC1 along the longest axis.

**Follow-up Questions:**
- What does it mean if PC1 captures 90% of variance?
  - Answer: The data is mostly spread along one direction; a 1D representation captures most information
- Why must principal components be orthogonal?
  - Answer: Orthogonality ensures components are uncorrelated and capture independent aspects of variance

---

## Q3: Walk through the mathematical steps of PCA.

**Answer:**
1. **Center the data**: Subtract mean from each feature (X̃ = X - μ)
2. **Compute covariance matrix**: C = (1/n) X̃ᵀX̃
3. **Eigendecomposition**: Find eigenvalues λᵢ and eigenvectors vᵢ of C
4. **Sort**: Order eigenvectors by decreasing eigenvalues
5. **Select k components**: Choose top k eigenvectors
6. **Transform**: Project data onto selected eigenvectors: Z = X̃V_k

**Follow-up Questions:**
- Why do we center the data?
  - Answer: Centering ensures PCA finds directions of variance, not directions toward the mean
- Can we use SVD instead of eigendecomposition?
  - Answer: Yes, SVD of centered data X̃ = UΣVᵀ gives principal components directly; often more numerically stable

---

## Q4: What is the role of eigenvalues and eigenvectors in PCA?

**Answer:**
- **Eigenvectors**: Define the directions of principal components (the new axes)
- **Eigenvalues**: Indicate the amount of variance captured along each eigenvector

**Relationship**:
- Eigenvalue λᵢ = variance of data projected onto eigenvector vᵢ
- Proportion of variance explained by PCᵢ = λᵢ / Σλⱼ

**Follow-up Questions:**
- What if two eigenvalues are equal?
  - Answer: The corresponding eigenvectors span a 2D subspace of equal variance; any orthogonal basis in that subspace works
- What does a zero eigenvalue mean?
  - Answer: That direction has no variance - data lies in a lower-dimensional subspace

---

## Q5: How do you decide how many principal components to keep?

**Answer:**
**Methods:**
1. **Variance threshold**: Keep components explaining X% of total variance (e.g., 95%)
2. **Scree plot**: Plot eigenvalues vs component number; look for "elbow"
3. **Kaiser criterion**: Keep components with eigenvalue > 1 (for standardized data)
4. **Cross-validation**: If used for downstream task, validate performance

**Formula for cumulative variance:**
Cumulative variance = Σᵢ₌₁ᵏ λᵢ / Σⱼ₌₁ᵈ λⱼ

**Follow-up Questions:**
- What's the trade-off in choosing k?
  - Answer: More components = more information retained but less compression; fewer = more compression but potential information loss
- Why 95% variance as a common threshold?
  - Answer: Empirical rule; balances dimensionality reduction with information preservation

---

## Q6: What is a scree plot and how do you interpret it?

**Answer:**
A scree plot shows eigenvalues (or variance explained) on y-axis vs component number on x-axis.

**Interpretation:**
- **Steep decline**: Early components capture most variance
- **Elbow point**: Where curve flattens; suggests optimal number of components
- **Flat region**: Remaining components capture mostly noise

**Follow-up Questions:**
- What if there's no clear elbow?
  - Answer: Variance is spread more evenly; may need domain knowledge or cross-validation to choose k
- Can the scree plot be misleading?
  - Answer: Yes, if important information is in lower-variance directions (e.g., rare but important patterns)

---

## Q7: What is reconstruction error in PCA?

**Answer:**
Reconstruction error measures information lost when projecting to lower dimensions and back:

**Formula:** 
- Reconstruction: X̂ = ZVᵀ + μ (where Z is reduced representation)
- Error: ||X - X̂||² = Σᵢ₌ₖ₊₁ᵈ λᵢ (sum of discarded eigenvalues)

**Key insight**: PCA minimizes squared reconstruction error among all linear dimensionality reduction methods.

**Follow-up Questions:**
- How does reconstruction error relate to variance explained?
  - Answer: They're complementary: variance explained + reconstruction error (normalized) = 100%
- Can reconstruction error ever be zero?
  - Answer: Only if k = rank of data, meaning no dimensionality reduction

---

## Q8: What assumptions does PCA make?

**Answer:**
1. **Linearity**: Relationships between variables are linear; PCs are linear combinations
2. **Variance = importance**: High variance directions are most informative
3. **Orthogonality**: Principal components are uncorrelated
4. **Large variance = signal**: Low variance components are noise
5. **Mean and covariance are sufficient**: Data structure captured by first two moments

**Follow-up Questions:**
- What if relationships are nonlinear?
  - Answer: Use Kernel PCA or nonlinear methods like t-SNE/UMAP
- When does variance ≠ importance?
  - Answer: When signal has low variance (e.g., rare events) or noise has high variance

---

## Q9: Should you standardize data before PCA?

**Answer:**
**Standardize (z-score) when:**
- Features have different scales/units (e.g., age in years, income in dollars)
- You want each feature to contribute equally

**Don't standardize when:**
- Features are already on same scale
- Variance differences are meaningful (e.g., all measurements in same unit)

**Effect of not standardizing:** Features with larger variance dominate the principal components.

**Follow-up Questions:**
- What's the difference between centering and standardizing?
  - Answer: Centering subtracts mean (required for PCA); standardizing also divides by std dev (optional)
- Does standardization affect the number of components?
  - Answer: No, but it changes which directions are selected as principal components

---

## Q10: When does PCA fail or perform poorly?

**Answer:**
PCA performs poorly when:
1. **Nonlinear relationships**: Can't capture curved manifolds
2. **Variance ≠ importance**: Important patterns have low variance
3. **Non-Gaussian data**: PCA is optimal for Gaussian; may miss structure in other distributions
4. **Discrete/categorical features**: Correlations don't capture categorical relationships
5. **Outliers**: Heavily influence covariance matrix
6. **When interpretability needed**: PCs are often hard to interpret

**Follow-up Questions:**
- How do you handle nonlinear data?
  - Answer: Kernel PCA, autoencoders, or manifold learning methods (t-SNE, UMAP)
- How do outliers affect PCA?
  - Answer: Outliers inflate variance in their direction, potentially making noise the first PC

---

## Q11: What is Kernel PCA and when would you use it?

**Answer:**
Kernel PCA applies the kernel trick to perform PCA in a high-dimensional feature space without explicitly computing that space.

**Process:**
1. Compute kernel matrix K (e.g., RBF: K(x,y) = exp(-γ||x-y||²))
2. Center the kernel matrix
3. Eigendecomposition of centered kernel matrix
4. Project data using kernel evaluations

**Use when:** Data has nonlinear structure that linear PCA can't capture

**Follow-up Questions:**
- Can you reconstruct data from Kernel PCA?
  - Answer: Not directly; the mapping to feature space is implicit. Pre-image problem is difficult.
- How do you choose the kernel?
  - Answer: Cross-validation on downstream task, or domain knowledge about data structure

---

## Q12: How does PCA relate to SVD?

**Answer:**
For centered data matrix X (n×d):
- SVD: X = UΣVᵀ
- PCA eigenvectors = columns of V
- PCA eigenvalues = σᵢ²/n (where σᵢ are singular values)
- Principal components (scores) = UΣ = XV

**Advantages of SVD:**
- Numerically more stable
- Can handle n < d (more features than samples)
- Doesn't require computing covariance matrix explicitly

**Follow-up Questions:**
- When is SVD preferred over eigendecomposition?
  - Answer: When d is large (avoids d×d covariance matrix) or for numerical stability
- What if n < d?
  - Answer: Covariance matrix is rank-deficient; SVD handles this naturally

---

## Q13: Can PCA be used for feature selection?

**Answer:**
PCA does **feature extraction**, not feature selection:
- **Feature selection**: Choose subset of original features
- **Feature extraction** (PCA): Create new features as combinations of originals

**Using PCA for selection (indirect):**
- Look at loadings (weights) in top PCs
- Features with high absolute loadings in important PCs are influential

**Follow-up Questions:**
- Why might you prefer feature selection over PCA?
  - Answer: When interpretability matters, when you need to reduce data collection costs, or when original features have domain meaning
- What are the loadings in PCA?
  - Answer: Loadings are the coefficients (eigenvector elements) showing how much each original feature contributes to each PC

---

## Q14: What is the difference between PCA on covariance vs correlation matrix?

**Answer:**
| Covariance Matrix PCA | Correlation Matrix PCA |
|----------------------|------------------------|
| Uses raw centered data | Uses standardized data |
| Affected by scale | Scale-independent |
| Preserves variance structure | Treats all variables equally |
| Use when units are same | Use when units differ |

**Mathematical difference:**
- Covariance: C = X̃ᵀX̃/n
- Correlation: R = (X_std)ᵀ(X_std)/n (where X_std is standardized)

**Follow-up Questions:**
- Which is more common in practice?
  - Answer: Correlation matrix PCA (standardized) because real-world data often has different scales
- Does the choice affect reconstruction?
  - Answer: Yes; correlation PCA reconstructs standardized data, not original scale

---

## Q15: How do you interpret principal components?

**Answer:**
**Methods:**
1. **Examine loadings**: High positive/negative loadings indicate which features contribute most
2. **Biplot**: Visualize both scores and loadings together
3. **Domain knowledge**: Map loadings to meaningful combinations

**Example:** If PC1 has high loadings for all size-related features (height, weight, width), interpret as "size factor"

**Challenges:**
- PCs are often combinations that lack clear meaning
- Sign is arbitrary (PC and -PC are equivalent)

**Follow-up Questions:**
- Why is interpretation often difficult?
  - Answer: PCs maximize variance, not interpretability; features combine in mathematically optimal but not necessarily meaningful ways
- What's a biplot?
  - Answer: Scatterplot of first 2 PC scores with arrows showing feature loadings; shows both observations and variable relationships

---

## Q16: What is the time and space complexity of PCA?

**Answer:**
**Time complexity:**
- Covariance matrix: O(nd²)
- Eigendecomposition: O(d³)
- Total: O(nd² + d³)

**Space complexity:** O(d²) for covariance matrix

**For large d (SVD approach):**
- Time: O(min(nd², n²d))
- Can use randomized/incremental PCA for very large data

**Follow-up Questions:**
- How do you scale PCA to big data?
  - Answer: Randomized PCA, incremental PCA (mini-batches), or approximate methods
- What if d >> n?
  - Answer: Compute d×d covariance is expensive; use SVD on n×d matrix or kernel PCA on n×n kernel matrix

---

## Q17: What is Incremental PCA?

**Answer:**
Incremental PCA updates the PCA model with batches of data without loading entire dataset into memory.

**Key features:**
- Process data in chunks/mini-batches
- Memory efficient: O(batch_size × d) instead of O(n × d)
- Approximate solution that converges to batch PCA

**Use cases:**
- Data too large for memory
- Streaming data
- Online learning scenarios

**Follow-up Questions:**
- Is Incremental PCA exact?
  - Answer: No, it's an approximation; quality depends on batch size and number of passes
- How does it differ from Online PCA?
  - Answer: Similar concepts; Online PCA typically updates with single samples, Incremental uses mini-batches

---

## Q18: How does PCA help with multicollinearity?

**Answer:**
**Problem:** Multicollinearity (highly correlated features) causes:
- Unstable regression coefficients
- Inflated variance in estimates

**PCA solution:**
- Transforms to uncorrelated principal components
- Can use PC scores as regression features (Principal Component Regression)
- Removes redundancy by combining correlated features

**Follow-up Questions:**
- Is using all PCs the same as using original features?
  - Answer: Mathematically equivalent for linear models; benefit comes from removing low-variance PCs
- What's Principal Component Regression (PCR)?
  - Answer: Regress target on top k principal components instead of original features

---

## Q19: Compare PCA with other dimensionality reduction methods.

**Answer:**
| Method | Linear? | Preserves | Good for |
|--------|---------|-----------|----------|
| PCA | Yes | Global variance | Compression, denoising |
| t-SNE | No | Local structure | Visualization |
| UMAP | No | Local + some global | Visualization, faster than t-SNE |
| LDA | Yes | Class separation | Supervised reduction |
| Autoencoders | No | Learned features | Complex nonlinear patterns |

**Follow-up Questions:**
- When would you choose PCA over t-SNE?
  - Answer: When you need linear transformation, interpretability, ability to transform new data, or for preprocessing
- Can you combine PCA with t-SNE?
  - Answer: Yes, common practice: PCA to ~50 dimensions, then t-SNE for visualization

---

## Q20: What is Sparse PCA and when is it useful?

**Answer:**
Sparse PCA adds a sparsity constraint (L1 regularization) to make loadings exactly zero for some features.

**Optimization:** Maximize variance subject to:
- Orthogonality constraint
- Sparsity constraint (||loadings||₁ ≤ λ)

**Benefits:**
- More interpretable components (fewer non-zero loadings)
- Automatic feature selection within PCs
- Useful when original features have meaning

**Follow-up Questions:**
- What's the trade-off with sparsity?
  - Answer: Sparsity improves interpretability but captures less variance than regular PCA
- How do you choose the sparsity parameter?
  - Answer: Cross-validation, or target a specific number of non-zero loadings based on domain needs
# t-SNE (t-Distributed Stochastic Neighbor Embedding)

## Q1: What is t-SNE and what is it primarily used for?

**Answer:**
t-SNE is a nonlinear dimensionality reduction technique designed specifically for visualization of high-dimensional data in 2D or 3D.

**Key characteristics:**
- Preserves local structure (nearby points stay nearby)
- Reveals clusters and patterns invisible in linear methods
- Non-parametric (no explicit mapping function)
- Primarily used for visualization, not preprocessing

**Follow-up Questions:**
- Why is t-SNE mainly used for visualization and not general dimensionality reduction?
  - Answer: It's computationally expensive, non-deterministic, and can't easily embed new points
- Can t-SNE be used for more than 3 dimensions?
  - Answer: Yes, but loses visualization benefit; also becomes computationally heavier

---

## Q2: Explain the intuition behind how t-SNE works.

**Answer:**
t-SNE works in two main steps:

1. **High-dimensional space**: Convert distances to probabilities
   - Similar points get high probability of being "neighbors"
   - Uses Gaussian distribution centered on each point

2. **Low-dimensional space**: Find arrangement that matches these probabilities
   - Uses t-distribution (heavier tails) for low-dimensional probabilities
   - Minimizes difference between high-D and low-D probability distributions

**Intuition:** "Arrange points in 2D such that neighbors in high-D remain neighbors in 2D"

**Follow-up Questions:**
- Why convert distances to probabilities?
  - Answer: Makes the similarity measure scale-invariant and focuses on relative, not absolute, distances
- What does "minimize difference" mean specifically?
  - Answer: Minimize KL divergence between high-D and low-D probability distributions

---

## Q3: What is the mathematical formulation of t-SNE?

**Answer:**
**High-dimensional similarities (Gaussian):**
p(j|i) = exp(-||xᵢ - xⱼ||² / 2σᵢ²) / Σₖ≠ᵢ exp(-||xᵢ - xₖ||² / 2σᵢ²)

Symmetrized: pᵢⱼ = (p(j|i) + p(i|j)) / 2n

**Low-dimensional similarities (t-distribution with 1 df):**
qᵢⱼ = (1 + ||yᵢ - yⱼ||²)⁻¹ / Σₖ≠ₗ(1 + ||yₖ - yₗ||²)⁻¹

**Objective:** Minimize KL divergence
C = KL(P||Q) = Σᵢⱼ pᵢⱼ log(pᵢⱼ/qᵢⱼ)

**Follow-up Questions:**
- Why symmetrize the probabilities?
  - Answer: Ensures pᵢⱼ = pⱼᵢ, making the similarity measure symmetric and handling outliers better
- How is the optimization done?
  - Answer: Gradient descent on the KL divergence with respect to low-dimensional coordinates yᵢ

---

## Q4: Why does t-SNE use t-distribution in the low-dimensional space?

**Answer:**
The t-distribution (Student-t with 1 degree of freedom = Cauchy) has heavier tails than Gaussian, which solves the **crowding problem**:

**Crowding problem:** In high dimensions, a point can have many equidistant neighbors. In 2D, there's not enough "room" to place all neighbors equally close.

**Solution:** Heavy tails of t-distribution allow moderate distances in low-D to represent small distances in high-D, giving clusters room to spread.

**Follow-up Questions:**
- What happens if you use Gaussian in both spaces?
  - Answer: Points crowd into the center; clusters merge and structure is lost (original SNE problem)
- Why specifically 1 degree of freedom?
  - Answer: Provides sufficient heavy tails; more degrees would approach Gaussian and lose the benefit

---

## Q5: What is perplexity in t-SNE and how do you choose it?

**Answer:**
Perplexity is a hyperparameter that controls the effective number of neighbors considered for each point.

**Mathematical definition:**
Perplexity = 2^H(P) where H(P) is the entropy of the probability distribution

**Practical interpretation:**
- Perplexity ≈ expected number of neighbors
- Typical values: 5-50
- Controls σᵢ (bandwidth) for each point

**Choosing perplexity:**
- Small perplexity (5-10): Focus on very local structure
- Large perplexity (30-50): Consider more global structure
- Try multiple values and compare

**Follow-up Questions:**
- What happens with very small perplexity?
  - Answer: May see spurious small clusters; noise affects results more
- What happens with very large perplexity?
  - Answer: Clusters may merge; loses fine local structure; if perplexity ≈ n, approximates PCA

---

## Q6: What are the main limitations of t-SNE?

**Answer:**
1. **Non-deterministic**: Different runs give different results (random initialization)
2. **No out-of-sample extension**: Can't embed new points without re-running
3. **Computationally expensive**: O(n²) naive, O(n log n) with approximations
4. **Hyperparameter sensitive**: Results vary significantly with perplexity
5. **Distances not meaningful**: Global distances and cluster sizes can be misleading
6. **Non-convex optimization**: Can get stuck in local minima

**Follow-up Questions:**
- How can you get reproducible results?
  - Answer: Set random seed, use same initialization (e.g., PCA initialization)
- What's the memory complexity?
  - Answer: O(n²) for storing pairwise similarities; can be reduced with approximations

---

## Q7: How do you interpret t-SNE plots correctly?

**Answer:**
**DO interpret:**
- Presence of clusters (points that group together)
- Relative proximity of points within clusters
- General separation between distinct groups

**DON'T over-interpret:**
- Distances between clusters (can be arbitrary)
- Cluster sizes (don't reflect actual data density)
- Cluster shapes (can be artifacts of the algorithm)

**Best practices:**
- Run multiple times with different seeds
- Try different perplexity values
- Validate clusters with other methods

**Follow-up Questions:**
- Why can't we trust inter-cluster distances?
  - Answer: t-SNE optimizes local structure; global distances are sacrificed to achieve good local arrangement
- Two clusters appear far apart - does that mean they're very different?
  - Answer: Not necessarily; t-SNE may place them far for visual clarity, not because of actual distance

---

## Q8: Compare t-SNE with PCA.

**Answer:**
| Aspect | PCA | t-SNE |
|--------|-----|-------|
| Type | Linear | Nonlinear |
| Preserves | Global structure, variance | Local structure |
| Speed | Fast O(min(nd², n²d)) | Slow O(n²) or O(n log n) |
| Deterministic | Yes | No (random initialization) |
| New points | Easy (matrix multiply) | Difficult (re-run needed) |
| Interpretable | Loadings have meaning | No direct interpretation |
| Best for | Preprocessing, compression | Visualization |

**Follow-up Questions:**
- Should you run PCA before t-SNE?
  - Answer: Yes, often recommended to reduce to 50-100 dimensions first; speeds up t-SNE and removes noise
- When would PCA visualization be sufficient?
  - Answer: When data has linear structure or when first 2-3 PCs capture most variance

---

## Q9: What is the crowding problem and how does t-SNE address it?

**Answer:**
**Problem:** In high dimensions, the volume of space increases exponentially. A point can have many neighbors at similar distances. When mapping to 2D, there's insufficient area to place all these neighbors at appropriate distances - they "crowd" together.

**Mathematical insight:** Volume of d-dimensional sphere ∝ r^d; as d increases, most volume is near the surface.

**t-SNE solution:**
- Uses heavy-tailed t-distribution in low dimensions
- Allows moderately distant points in 2D to represent close neighbors in high-D
- Effectively "stretches" the low-D space to accommodate more neighbors

**Follow-up Questions:**
- Does the crowding problem affect all dimensionality reduction methods?
  - Answer: Yes, but linear methods like PCA suffer differently (lose structure rather than crowd)
- Why doesn't this create false separations?
  - Answer: The KL divergence penalizes placing true neighbors far apart more than bringing distant points closer

---

## Q10: Explain the optimization process in t-SNE.

**Answer:**
t-SNE uses gradient descent to minimize KL divergence:

**Gradient:** 
∂C/∂yᵢ = 4 Σⱼ (pᵢⱼ - qᵢⱼ)(yᵢ - yⱼ)(1 + ||yᵢ - yⱼ||²)⁻¹

**Optimization tricks:**
1. **Early exaggeration**: Multiply pᵢⱼ by 4 initially to form tight clusters
2. **Momentum**: Use momentum to escape local minima
3. **Adaptive learning rate**: Adjust step size per point
4. **Early compression**: Add L2 penalty initially to keep points together

**Follow-up Questions:**
- Why use early exaggeration?
  - Answer: Forces clusters to form early; easier to separate later than to merge already-spread points
- How many iterations are typically needed?
  - Answer: 1000-5000 iterations usually; should monitor convergence

---

## Q11: What is Barnes-Hut t-SNE?

**Answer:**
Barnes-Hut t-SNE is an approximation that reduces complexity from O(n²) to O(n log n).

**Key ideas:**
1. **Sparse similarities**: Only compute pᵢⱼ for nearest neighbors (using efficient KNN)
2. **Barnes-Hut approximation**: For gradient computation, group distant points using space-partitioning tree (quadtree/octree)
3. **Approximation parameter θ**: Controls accuracy vs speed trade-off

**Practical impact:**
- Enables t-SNE on datasets with millions of points
- Slight loss in quality for large θ

**Follow-up Questions:**
- What's a typical value for θ?
  - Answer: θ = 0.5 is common; lower = more accurate but slower
- Are there even faster variants?
  - Answer: Yes, FFT-accelerated t-SNE (FIt-SNE) and GPU implementations

---

## Q12: Can you embed new points into an existing t-SNE visualization?

**Answer:**
**Direct embedding: Not possible** - t-SNE doesn't learn a parametric mapping.

**Workarounds:**
1. **Re-run t-SNE**: Include new points and run again (positions of old points will change)
2. **Parametric t-SNE**: Train a neural network to approximate the mapping
3. **Approximate embedding**: Use KNN in original space to position new point near its neighbors
4. **Use UMAP instead**: UMAP can transform new points more easily

**Follow-up Questions:**
- Why doesn't t-SNE have a transform function?
  - Answer: It directly optimizes positions, not a mapping function; there's no formula y = f(x)
- When would this limitation be a serious problem?
  - Answer: Real-time applications, streaming data, or when you need to embed many new points frequently
# UMAP (Uniform Manifold Approximation and Projection)

## Q1: What is UMAP and how does it differ from t-SNE?

**Answer:**
UMAP is a nonlinear dimensionality reduction technique based on manifold learning and topological data analysis.

**Key differences from t-SNE:**
| Aspect | UMAP | t-SNE |
|--------|------|-------|
| Speed | Faster, scales better | Slower |
| Global structure | Better preserved | Often lost |
| Theory | Based on Riemannian geometry | Based on probability distributions |
| New points | Can transform new data | Cannot easily |
| Hyperparameters | n_neighbors, min_dist | Perplexity |

**Follow-up Questions:**
- Why is UMAP faster than t-SNE?
  - Answer: UMAP uses approximate nearest neighbor algorithms and doesn't require computing all pairwise distances
- When would you still choose t-SNE over UMAP?
  - Answer: When you need very fine local structure, or for reproducibility in published results where t-SNE is standard

---

## Q2: Explain the theoretical foundation of UMAP.

**Answer:**
UMAP is based on three mathematical foundations:

1. **Manifold assumption**: High-dimensional data lies on a low-dimensional manifold
2. **Fuzzy topology**: Represents the manifold as a fuzzy simplicial complex (weighted graph)
3. **Cross-entropy optimization**: Minimizes cross-entropy between high-D and low-D fuzzy representations

**Intuition:**
- Build a weighted graph connecting nearby points in high-D
- Find a low-D layout that preserves this graph structure
- "Fuzzy" because edge weights represent probability of connection

**Follow-up Questions:**
- What's a simplicial complex?
  - Answer: A mathematical structure made of vertices, edges, triangles, and higher-dimensional analogues; UMAP uses 1-skeleton (just vertices and edges)
- Why "fuzzy"?
  - Answer: Edge weights are continuous [0,1] rather than binary, representing uncertainty in connectivity

---

## Q3: What are the main hyperparameters in UMAP?

**Answer:**
**1. n_neighbors** (default: 15)
- Controls local vs global structure trade-off
- Small values: Focus on very local structure, may fragment
- Large values: More global structure, may lose fine details

**2. min_dist** (default: 0.1)
- Minimum distance between points in low-D
- Small values: Tight clusters, may overlap
- Large values: More spread out, preserves topology

**3. n_components** (default: 2)
- Output dimensionality

**4. metric** (default: 'euclidean')
- Distance metric for computing neighbors

**Follow-up Questions:**
- How is n_neighbors different from perplexity in t-SNE?
  - Answer: Similar concept (effective neighborhood size), but UMAP uses hard cutoff while t-SNE uses soft Gaussian weighting
- What's a good starting point for hyperparameters?
  - Answer: n_neighbors=15, min_dist=0.1 is reasonable; adjust based on whether you want more local (lower) or global (higher n_neighbors) structure

---

## Q4: How does UMAP handle the manifold assumption?

**Answer:**
UMAP assumes data lies on a locally connected Riemannian manifold:

**Construction:**
1. For each point, find k nearest neighbors
2. Estimate local metric (distance scale) based on distance to kth neighbor
3. Build fuzzy edges: weight = exp(-(d - ρ)/σ) where ρ is distance to nearest neighbor
4. Symmetrize: combine edges from both directions

**Key insight:** Local distance scales (σ) adapt to data density - sparse regions have larger scales.

**Follow-up Questions:**
- What if data doesn't lie on a manifold?
  - Answer: UMAP still works but may produce misleading results; random noise will create artificial structure
- Why adapt distance scales locally?
  - Answer: Ensures each point has roughly the same number of "effective" neighbors, handling varying density

---

## Q5: Compare UMAP and t-SNE in preserving global structure.

**Answer:**
**t-SNE:**
- Focuses almost entirely on local structure
- KL divergence penalizes placing neighbors far apart but not placing distant points close
- Global structure (cluster distances, positions) often arbitrary

**UMAP:**
- Uses cross-entropy which is symmetric
- Penalizes both: neighbors far apart AND non-neighbors close together
- Better preserves relative positions of clusters

**Practical implication:** In UMAP, if clusters A and B appear far from cluster C, this likely reflects actual high-dimensional distances.

**Follow-up Questions:**
- Is UMAP's global structure fully reliable?
  - Answer: Better than t-SNE but still approximate; verify with other methods if global relationships matter
- How can you improve global structure preservation in t-SNE?
  - Answer: Use higher perplexity, or try variants like large-vis

---

## Q6: When does UMAP fail or produce misleading results?

**Answer:**
**Failure cases:**
1. **No manifold structure**: Random/noisy data will show spurious clusters
2. **Very high dimensions**: May need PCA preprocessing
3. **Wrong distance metric**: Using Euclidean when data needs different metric
4. **Extreme hyperparameters**: Too small n_neighbors creates disconnected fragments
5. **Insufficient data**: Need enough points to estimate local structure

**Misleading results:**
- Cluster sizes don't reflect actual sizes
- Connected clusters in high-D might appear separate
- Very different data configurations can produce similar plots

**Follow-up Questions:**
- How many points are needed for reliable UMAP?
  - Answer: Hundreds minimum; thousands preferred for complex structures
- How do you validate UMAP results?
  - Answer: Try multiple hyperparameters, compare with t-SNE, validate clusters with other methods

---

## Q7: How does UMAP handle new/unseen data points?

**Answer:**
Unlike t-SNE, UMAP can embed new points:

**Process:**
1. Find nearest neighbors of new point in original training data
2. Use the learned transformation to estimate low-D position
3. Optionally optimize position to match neighborhood structure

**Implementation:** `umap_model.transform(new_data)`

**Caveats:**
- Works best when new data is similar to training data
- Extrapolation to very different regions may be poor
- Positions are approximate

**Follow-up Questions:**
- Why can UMAP do this but t-SNE can't?
  - Answer: UMAP learns a parametric approximation of the mapping during training; t-SNE only optimizes point positions
- Is the transform exact?
  - Answer: No, it's an approximation; for exact embedding, would need to re-run UMAP with new points included

---

## Q8: What is the role of the min_dist parameter?

**Answer:**
min_dist controls how tightly points can pack in low-dimensional space:

**Effects:**
- **min_dist ≈ 0**: Points can be very close; creates tight, compact clusters
- **min_dist large (0.5-1)**: Points spread out; preserves more topological structure

**Trade-offs:**
| Small min_dist | Large min_dist |
|----------------|----------------|
| Tight clusters | Spread out points |
| May have overlapping clusters | Better separation |
| Shows fine structure | Shows broad structure |
| Good for clustering | Good for topology |

**Follow-up Questions:**
- What's the maximum useful value for min_dist?
  - Answer: Usually < 1; values approaching 1 spread points uniformly
- How does min_dist interact with n_neighbors?
  - Answer: n_neighbors affects graph structure; min_dist affects how that structure is rendered in low-D

---

## Q9: Explain the optimization objective in UMAP.

**Answer:**
UMAP minimizes cross-entropy between high-D and low-D fuzzy set representations:

**Objective:**
CE = Σᵢⱼ [μᵢⱼ log(μᵢⱼ/νᵢⱼ) + (1-μᵢⱼ) log((1-μᵢⱼ)/(1-νᵢⱼ))]

Where:
- μᵢⱼ = high-dimensional edge weight (fuzzy membership)
- νᵢⱼ = low-dimensional edge weight

**Two terms:**
1. **Attraction**: μᵢⱼ log(μᵢⱼ/νᵢⱼ) - pull connected points together
2. **Repulsion**: (1-μᵢⱼ) log(...) - push disconnected points apart

**Follow-up Questions:**
- How does this differ from t-SNE's KL divergence?
  - Answer: KL divergence is asymmetric (only penalizes neighbors far apart); cross-entropy is symmetric
- How is optimization performed?
  - Answer: Stochastic gradient descent with negative sampling for efficiency

---

## Q10: What are the computational complexity and scalability of UMAP?

**Answer:**
**Time complexity:**
- Building k-NN graph: O(n^1.14) with approximate methods
- Optimization: O(n × epochs × n_neighbors)
- Overall: Much faster than O(n²) t-SNE

**Memory complexity:**
- O(n × n_neighbors) for sparse graph
- Much better than O(n²) for dense similarity matrix

**Scalability:**
- Can handle millions of points
- GPU-accelerated implementations available (cuML)
- Parallelizable graph construction

**Follow-up Questions:**
- How does UMAP achieve O(n^1.14) for k-NN?
  - Answer: Uses approximate nearest neighbor algorithms like Annoy or NN-Descent
- What's the limiting factor for very large datasets?
  - Answer: Memory for storing graph; can use out-of-core or sampling approaches
# K-Means Clustering

## Q1: Explain the K-Means algorithm step by step.

**Answer:**
**Algorithm:**
1. **Initialize**: Choose k initial centroids (randomly or using K-Means++)
2. **Assign**: Assign each point to nearest centroid → forms k clusters
3. **Update**: Recalculate centroids as mean of assigned points
4. **Repeat**: Steps 2-3 until convergence (centroids don't change)

**Convergence criteria:**
- Centroids stop moving
- Assignments don't change
- Maximum iterations reached

**Follow-up Questions:**
- Is K-Means guaranteed to converge?
  - Answer: Yes, but only to a local minimum; inertia decreases monotonically with each iteration
- How many iterations are typically needed?
  - Answer: Usually 10-300 iterations; depends on data and initialization

---

## Q2: What is the objective function that K-Means minimizes?

**Answer:**
K-Means minimizes **within-cluster sum of squares (WCSS)** or **inertia**:

J = Σᵢ₌₁ᵏ Σₓ∈Cᵢ ||x - μᵢ||²

Where:
- k = number of clusters
- Cᵢ = set of points in cluster i
- μᵢ = centroid of cluster i

**Interpretation:** Sum of squared distances from each point to its cluster center.

**Follow-up Questions:**
- Why squared distances instead of absolute distances?
  - Answer: Squared distances are differentiable (enables gradient-based optimization) and penalize large errors more
- What algorithm minimizes absolute distances?
  - Answer: K-Medians; uses median instead of mean, more robust to outliers

---

## Q3: What is the time complexity of K-Means?

**Answer:**
**Per iteration:** O(n × k × d)
- n = number of points
- k = number of clusters  
- d = number of dimensions

**Total:** O(i × n × k × d)
- i = number of iterations

**Space complexity:** O(n × d + k × d)
- Store data points and centroids

**Follow-up Questions:**
- How does K-Means scale with number of clusters?
  - Answer: Linear in k; but more clusters typically need more iterations to converge
- Is K-Means suitable for big data?
  - Answer: Yes, it's relatively efficient; Mini-batch K-Means scales even better

---

## Q4: How do you choose the optimal number of clusters (k)?

**Answer:**
**1. Elbow Method:**
- Plot inertia vs k
- Look for "elbow" where marginal improvement decreases sharply

**2. Silhouette Score:**
- Measures cohesion vs separation
- s(i) = (b(i) - a(i)) / max(a(i), b(i))
- Choose k with highest average silhouette

**3. Gap Statistic:**
- Compares inertia to expected under null (uniform) distribution
- Choose k where gap is largest

**4. Domain Knowledge:**
- Sometimes k is determined by business requirements

**Follow-up Questions:**
- What if there's no clear elbow?
  - Answer: Data may not have clear cluster structure; try silhouette score or other methods
- Can silhouette score be negative?
  - Answer: Yes, indicates point is probably in wrong cluster (closer to another cluster's points)

---

## Q5: What are the problems with random initialization in K-Means?

**Answer:**
**Problems:**
1. **Local minima**: Different initializations give different results
2. **Poor convergence**: Bad initialization leads to suboptimal clustering
3. **Empty clusters**: A centroid may have no points assigned
4. **Slow convergence**: Points may need many reassignments

**Example:** If two initial centroids are in the same actual cluster, one cluster will be split and another merged.

**Follow-up Questions:**
- How can you mitigate initialization problems?
  - Answer: Run K-Means multiple times with different seeds; use K-Means++ initialization
- What happens if a cluster becomes empty?
  - Answer: Implementations typically reinitialize that centroid randomly or split largest cluster

---

## Q6: Explain K-Means++ initialization.

**Answer:**
K-Means++ is a smart initialization that spreads initial centroids:

**Algorithm:**
1. Choose first centroid uniformly at random
2. For each remaining centroid:
   - Calculate D(x)² = distance to nearest existing centroid for each point
   - Choose next centroid with probability proportional to D(x)²
3. Repeat until k centroids chosen

**Benefit:** Points far from existing centroids are more likely to be chosen → better spread.

**Guarantees:** Expected inertia is O(log k) competitive with optimal.

**Follow-up Questions:**
- Why use D(x)² instead of D(x)?
  - Answer: Squared distance gives even more preference to distant points; empirically works better
- Does K-Means++ guarantee global optimum?
  - Answer: No, but it significantly reduces the chance of very poor local minima

---

## Q7: What assumptions does K-Means make about data?

**Answer:**
1. **Spherical clusters**: Assumes clusters are roughly spherical/isotropic
2. **Similar sizes**: Works best when clusters have similar number of points
3. **Similar density**: Assumes clusters have similar spread
4. **Linear separability**: Cluster boundaries are linear (Voronoi cells)
5. **Euclidean distance meaningful**: Features should be numeric and scaled

**Follow-up Questions:**
- What happens with non-spherical clusters?
  - Answer: K-Means may split one cluster or merge different clusters; use DBSCAN or GMM instead
- Why is scaling important?
  - Answer: Euclidean distance is dominated by features with larger scale; normalize features first

---

## Q8: How do outliers affect K-Means?

**Answer:**
**Effects:**
1. **Centroid bias**: Outliers pull centroids away from cluster centers
2. **Separate clusters**: Outliers may form their own clusters
3. **Increased inertia**: Large distances inflate the objective function

**Why K-Means is sensitive:**
- Uses mean (affected by outliers) not median
- Squared distance gives outliers disproportionate influence

**Solutions:**
- Remove outliers before clustering
- Use K-Medoids (uses actual data points as centers)
- Use robust distance metrics

**Follow-up Questions:**
- What is K-Medoids?
  - Answer: Similar to K-Means but uses medoids (actual data points) as cluster centers; more robust but O(n²)
- Can you detect outliers using K-Means?
  - Answer: Yes, points with very large distances to their centroid may be outliers

---

## Q9: What is Mini-Batch K-Means?

**Answer:**
Mini-Batch K-Means uses random subsets (mini-batches) for centroid updates:

**Algorithm:**
1. Initialize centroids
2. For each iteration:
   - Sample mini-batch of size b
   - Assign batch points to nearest centroids
   - Update centroids using only batch points (with learning rate)
3. Repeat until convergence

**Advantages:**
- Much faster for large datasets
- Memory efficient
- Often similar quality to full K-Means

**Trade-off:** Slightly less accurate; centroids may be noisier.

**Follow-up Questions:**
- How do you choose batch size?
  - Answer: Typically 100-1000; larger batches are more accurate but slower
- When is Mini-Batch K-Means preferred?
  - Answer: Large datasets (n > 10,000), streaming data, or when speed matters more than perfect accuracy

---

## Q10: Compare K-Means with hierarchical clustering.

**Answer:**
| Aspect | K-Means | Hierarchical |
|--------|---------|--------------|
| Requires k upfront | Yes | No (can cut at any level) |
| Complexity | O(nki) | O(n²) or O(n² log n) |
| Cluster shape | Spherical | Any shape |
| Result | Flat partition | Dendrogram (tree) |
| Scalability | Better | Poor for large n |
| Deterministic | No (random init) | Yes |

**Follow-up Questions:**
- When would you prefer hierarchical clustering?
  - Answer: When you want to explore different numbers of clusters, need a hierarchy, or have small data
- Can you use hierarchical clustering to initialize K-Means?
  - Answer: Yes, cut dendrogram at k clusters and use those centroids

---

## Q11: What is the silhouette score and how do you interpret it?

**Answer:**
**Formula for point i:**
s(i) = (b(i) - a(i)) / max(a(i), b(i))

Where:
- a(i) = average distance to points in same cluster (cohesion)
- b(i) = average distance to points in nearest other cluster (separation)

**Interpretation:**
- s(i) ≈ 1: Point is well-clustered
- s(i) ≈ 0: Point is on boundary between clusters
- s(i) < 0: Point may be in wrong cluster

**Overall score:** Average s(i) across all points; ranges from -1 to 1.

**Follow-up Questions:**
- What's a good silhouette score?
  - Answer: > 0.5 indicates reasonable structure; > 0.7 is strong; but depends on data
- Can you use silhouette score to choose k?
  - Answer: Yes, try different k values and choose the one with highest average silhouette

---

## Q12: How does K-Means perform in high dimensions?

**Answer:**
K-Means struggles in high dimensions due to:

1. **Curse of dimensionality**: All pairwise distances become similar
2. **Sparse data**: Need exponentially more data to maintain density
3. **Irrelevant features**: Noise dimensions distort distances
4. **Computation**: O(n × k × d) scales linearly with dimensions

**Solutions:**
- Dimensionality reduction (PCA) before clustering
- Feature selection to remove irrelevant features
- Use subspace clustering methods

**Follow-up Questions:**
- At what dimensionality does K-Means start failing?
  - Answer: Depends on data, but often d > 20-50 starts showing issues; d > 100 is problematic
- Can you use K-Means on text data?
  - Answer: Yes, but typically after dimensionality reduction (TF-IDF + SVD) or using specialized text clustering

---

## Q13: Explain the connection between K-Means and GMM.

**Answer:**
K-Means is a special case of GMM (Gaussian Mixture Model):

**GMM with constraints:**
- All covariances are spherical (σ²I)
- All covariances are equal
- Hard assignment (each point belongs to one cluster with probability 1)

**EM algorithm for constrained GMM:**
- E-step becomes hard assignment (K-Means assignment step)
- M-step becomes centroid update (K-Means update step)

**Follow-up Questions:**
- What does GMM provide that K-Means doesn't?
  - Answer: Soft assignments (probabilities), elliptical clusters (different shapes), cluster covariances
- When is K-Means preferred over GMM?
  - Answer: When you need speed, have clear spherical clusters, or want hard assignments

---

## Q14: What is the Voronoi diagram and how does it relate to K-Means?

**Answer:**
A Voronoi diagram partitions space into regions where each region contains points closest to one centroid.

**Relation to K-Means:**
- K-Means assignment step creates a Voronoi partition
- Each cluster is a Voronoi cell
- Boundaries are linear (hyperplanes equidistant from two centroids)

**Implication:** K-Means can only create convex, linearly-bounded clusters.

**Follow-up Questions:**
- Why can't K-Means find non-convex clusters?
  - Answer: Voronoi cells are always convex; points between two parts of a non-convex cluster will be assigned elsewhere
- How do you handle non-convex clusters?
  - Answer: Use DBSCAN, spectral clustering, or kernel K-Means

---

## Q15: What are some variants and improvements of K-Means?

**Answer:**
**1. K-Means++:** Better initialization (discussed earlier)

**2. K-Medoids (PAM):** Uses actual points as centers; robust to outliers

**3. Bisecting K-Means:** Hierarchical approach; recursively split clusters

**4. Kernel K-Means:** Apply kernel trick for non-linear boundaries

**5. Fuzzy C-Means:** Soft assignments; points belong to multiple clusters with weights

**6. X-Means:** Automatically determines k using BIC

**Follow-up Questions:**
- When would you use Kernel K-Means?
  - Answer: When clusters are non-linearly separable; maps data to higher dimensions implicitly
- What's the advantage of Bisecting K-Means?
  - Answer: Can give hierarchy of clusters; often more consistent results than regular K-Means
# Gaussian Mixture Models (GMM) and Expectation-Maximization (EM)

## Q1: What is a Gaussian Mixture Model?

**Answer:**
A GMM assumes data is generated from a mixture of K Gaussian distributions:

p(x) = Σₖ₌₁ᴷ πₖ N(x | μₖ, Σₖ)

**Components:**
- πₖ = mixing coefficient (weight) for cluster k, Σπₖ = 1
- μₖ = mean of cluster k
- Σₖ = covariance matrix of cluster k
- N(x | μ, Σ) = multivariate Gaussian density

**Interpretation:** Each data point is generated by first selecting a cluster (with probability πₖ), then sampling from that cluster's Gaussian.

**Follow-up Questions:**
- How many parameters does a GMM have?
  - Answer: K-1 mixing weights + K×d means + K×d(d+1)/2 covariance params (for full covariance)
- Why is it called a "mixture" model?
  - Answer: The overall density is a weighted combination (mixture) of component densities

---

## Q2: What is the difference between hard and soft clustering in the context of GMM?

**Answer:**
**Hard clustering (K-Means style):**
- Each point belongs to exactly one cluster
- Assignment: arg maxₖ p(k|x)
- Binary membership

**Soft clustering (GMM):**
- Each point has probability of belonging to each cluster
- Responsibility: γₖ(x) = p(k|x) = πₖN(x|μₖ,Σₖ) / Σⱼπⱼ N(x|μⱼ,Σⱼ)
- Continuous membership [0,1]

**Advantages of soft clustering:**
- Captures uncertainty
- Better for overlapping clusters
- Provides richer information

**Follow-up Questions:**
- When would you prefer hard assignments?
  - Answer: When you need a clear partition, or for downstream tasks requiring discrete labels
- Can you convert soft to hard assignments?
  - Answer: Yes, assign to cluster with highest responsibility (posterior probability)

---

## Q3: Explain the 1D Gaussian distribution.

**Answer:**
**Formula:**
N(x | μ, σ²) = (1/√(2πσ²)) exp(-(x-μ)²/(2σ²))

**Parameters:**
- μ = mean (center of distribution)
- σ² = variance (spread)
- σ = standard deviation

**Properties:**
- Bell-shaped, symmetric around μ
- 68% of data within μ ± σ
- 95% within μ ± 2σ
- 99.7% within μ ± 3σ

**Follow-up Questions:**
- What does higher variance mean geometrically?
  - Answer: Wider, flatter bell curve; more spread out data
- What's the maximum value of the PDF?
  - Answer: 1/√(2πσ²) at x = μ; PDF can exceed 1 for small σ

---

## Q4: Explain the multivariate (2D+) Gaussian distribution.

**Answer:**
**Formula:**
N(x | μ, Σ) = (1/((2π)^(d/2)|Σ|^(1/2))) exp(-½(x-μ)ᵀΣ⁻¹(x-μ))

**Parameters:**
- μ = mean vector (d×1)
- Σ = covariance matrix (d×d, symmetric positive definite)

**Covariance matrix controls:**
- Diagonal elements: variance in each dimension
- Off-diagonal elements: correlation between dimensions
- Shape and orientation of probability contours (ellipsoids)

**Follow-up Questions:**
- What do equal-probability contours look like?
  - Answer: Ellipsoids; axes aligned with eigenvectors of Σ, lengths proportional to √eigenvalues
- What if Σ is diagonal?
  - Answer: Features are independent; ellipsoid axes aligned with coordinate axes

---

## Q5: What is the EM algorithm and why is it needed for GMM?

**Answer:**
**Problem:** GMM has latent (hidden) variables - which cluster each point belongs to. Direct maximum likelihood is intractable.

**EM alternates between:**
1. **E-step**: Estimate latent variables (cluster responsibilities) given current parameters
2. **M-step**: Maximize likelihood given responsibilities to update parameters

**Why needed:**
- If we knew cluster assignments → easy to fit Gaussians
- If we knew Gaussian parameters → easy to compute assignments
- EM iteratively improves both

**Follow-up Questions:**
- Does EM guarantee global optimum?
  - Answer: No, only local optimum; sensitive to initialization
- What other models use EM?
  - Answer: Hidden Markov Models, Latent Dirichlet Allocation, many mixture models

---

## Q6: Walk through the E-step and M-step of EM for GMM.

**Answer:**
**E-step (Expectation):**
Compute responsibilities (posterior probabilities):
γₖ(xᵢ) = πₖN(xᵢ|μₖ,Σₖ) / Σⱼπⱼ N(xᵢ|μⱼ,Σⱼ)

**M-step (Maximization):**
Update parameters using responsibilities:
- Nₖ = Σᵢ γₖ(xᵢ)  (effective number of points in cluster k)
- πₖ = Nₖ/n  (new mixing coefficient)
- μₖ = (1/Nₖ) Σᵢ γₖ(xᵢ)xᵢ  (weighted mean)
- Σₖ = (1/Nₖ) Σᵢ γₖ(xᵢ)(xᵢ-μₖ)(xᵢ-μₖ)ᵀ  (weighted covariance)

**Follow-up Questions:**
- Why are these called "soft" counts?
  - Answer: Nₖ uses fractional responsibilities, not hard 0/1 assignments
- How do you know when EM has converged?
  - Answer: Monitor log-likelihood; stop when change is below threshold

---

## Q7: How does GMM compare to K-Means?

**Answer:**
| Aspect | K-Means | GMM |
|--------|---------|-----|
| Cluster shape | Spherical | Elliptical (any covariance) |
| Assignment | Hard | Soft (probabilities) |
| Output | Cluster labels | Labels + probabilities |
| Parameters | Centroids only | Means, covariances, weights |
| Speed | Faster | Slower |
| Initialization | K-Means++ | Random or K-Means |

**K-Means as special case of GMM:**
- All covariances = σ²I (spherical, equal)
- Hard assignments (responsibilities → 0 or 1)

**Follow-up Questions:**
- When should you use GMM over K-Means?
  - Answer: When clusters have different shapes/sizes, when you need probabilities, or when clusters overlap
- Can you initialize GMM with K-Means?
  - Answer: Yes, common practice; use K-Means centroids as initial means

---

## Q8: What are the different covariance types in GMM?

**Answer:**
**1. Full:** Each cluster has its own full covariance matrix
- Most flexible; K×d×(d+1)/2 covariance parameters
- Can model any elliptical shape

**2. Tied:** All clusters share the same full covariance
- d×(d+1)/2 covariance parameters
- Same shape, different centers

**3. Diagonal:** Each cluster has diagonal covariance (axis-aligned ellipses)
- K×d parameters
- No correlation between features

**4. Spherical:** Each cluster has single variance (σₖ²I)
- K parameters
- Like K-Means with soft assignments

**Follow-up Questions:**
- Which type is most prone to overfitting?
  - Answer: Full covariance; has most parameters, especially problematic in high dimensions
- When would you use tied covariance?
  - Answer: When clusters differ mainly in location, not shape; reduces parameters significantly

---

## Q9: What is the log-likelihood for GMM and why do we maximize it?

**Answer:**
**Log-likelihood:**
L(θ) = Σᵢ log(Σₖ πₖ N(xᵢ|μₖ,Σₖ))

**Why maximize:**
- Higher likelihood = parameters that make observed data more probable
- Equivalent to maximum likelihood estimation (MLE)
- Log converts product to sum (numerically stable)

**EM property:** Each iteration increases or maintains log-likelihood (monotonic convergence).

**Follow-up Questions:**
- Why is direct maximization difficult?
  - Answer: Sum inside log prevents closed-form solution; involves latent variables
- Can log-likelihood decrease during EM?
  - Answer: No (except for numerical errors); this is a key property of EM

---

## Q10: How do you choose the number of components (K) in GMM?

**Answer:**
**1. BIC (Bayesian Information Criterion):**
BIC = -2L + p×log(n)
- L = maximized log-likelihood
- p = number of parameters
- Choose K with lowest BIC

**2. AIC (Akaike Information Criterion):**
AIC = -2L + 2p
- Less penalty than BIC; may favor more components

**3. Cross-validation:**
- Split data, fit on train, evaluate likelihood on validation

**4. Silhouette score:**
- Same as K-Means; use hard assignments

**Follow-up Questions:**
- Why does BIC include a penalty term?
  - Answer: More components always fit better; penalty prevents overfitting
- Which criterion is more conservative?
  - Answer: BIC penalizes complexity more (especially for large n); tends to select simpler models

---

## Q11: What problems can occur during GMM fitting?

**Answer:**
**1. Singularity:** Covariance matrix becomes singular
- Happens when a Gaussian collapses onto a single point
- Solution: Add regularization (covariance floor)

**2. Local minima:** EM converges to poor solution
- Solution: Multiple random restarts, K-Means initialization

**3. Slow convergence:** Many iterations needed
- Solution: Better initialization, early stopping

**4. Overfitting:** Too many components or full covariance with little data
- Solution: Regularization, simpler covariance types, BIC for model selection

**Follow-up Questions:**
- What is covariance regularization?
  - Answer: Add small value to diagonal (Σ + εI) to ensure positive definiteness
- How do you detect singularity?
  - Answer: Determinant becomes very small/zero, or likelihood explodes to infinity

---

## Q12: What is the relationship between EM and gradient descent?

**Answer:**
**Similarities:**
- Both iteratively optimize an objective
- Both can get stuck in local optima
- Both require initialization

**Differences:**
| EM | Gradient Descent |
|----|------------------|
| Closed-form updates | Requires learning rate |
| Specific to latent variable models | General optimization |
| Monotonically increases likelihood | May oscillate |
| Naturally handles constraints | Need to enforce constraints |

**Connection:** EM can be viewed as a form of coordinate ascent on a lower bound of the log-likelihood.

**Follow-up Questions:**
- Can you use gradient descent for GMM?
  - Answer: Yes, but EM is preferred because it has closed-form updates and guarantees improvement
- What is the ELBO?
  - Answer: Evidence Lower Bound; EM maximizes a lower bound on log-likelihood, which equals log-likelihood at the true posterior

---

## Q13: How does GMM handle outliers?

**Answer:**
**Behavior:**
- Outliers inflate covariance estimates
- May cause additional components to explain outliers
- Can pull means away from true cluster centers

**Solutions:**
1. **Robust estimation:** Use t-distribution instead of Gaussian (heavier tails)
2. **Outlier component:** Add a uniform or wide Gaussian component to capture outliers
3. **Pre-processing:** Remove outliers before fitting
4. **Regularization:** Constrain covariance estimates

**Follow-up Questions:**
- What is a Student-t mixture model?
  - Answer: Like GMM but uses t-distributions; extra degree of freedom parameter controls tail heaviness
- Why are t-distributions more robust?
  - Answer: Heavier tails assign higher probability to outliers, reducing their influence on mean/covariance

---

## Q14: Can GMM be used for density estimation?

**Answer:**
Yes, GMM is a powerful density estimator:

**Advantages:**
- Flexible: can approximate any smooth density with enough components
- Parametric: gives explicit probability p(x)
- Generative: can sample new data points

**Process:**
1. Fit GMM with appropriate K
2. Use fitted model to compute p(x) for any new x
3. Can sample by: pick component k with prob πₖ, then sample from N(μₖ, Σₖ)

**Applications:** Anomaly detection, data augmentation, likelihood-based classification

**Follow-up Questions:**
- How does this compare to kernel density estimation?
  - Answer: GMM is parametric (fixed # of params); KDE is non-parametric (grows with data); GMM generalizes better
- Can GMM estimate any distribution?
  - Answer: In theory yes (universal approximator); in practice limited by number of components and data

---

## Q15: Explain the intuition behind why EM works.

**Answer:**
**Intuition:** EM handles the chicken-and-egg problem of latent variables:
- To find parameters, need to know which cluster each point belongs to
- To know clusters, need to know parameters

**EM solution:**
1. Start with a guess
2. E-step: "If these parameters were true, what would cluster assignments probably be?"
3. M-step: "If these assignments were true, what would best parameters be?"
4. Repeat: Each step improves the other

**Mathematical justification:**
- EM maximizes a lower bound (ELBO) on log-likelihood
- E-step tightens the bound; M-step improves parameters
- Guaranteed non-decreasing likelihood

**Follow-up Questions:**
- Why does EM guarantee improvement?
  - Answer: E-step makes bound tight at current params; M-step improves params; bound can only increase
- Can EM ever decrease likelihood?
  - Answer: No (theoretically); numerical issues might cause tiny decreases in practice
# DBSCAN (Density-Based Spatial Clustering of Applications with Noise)

## Q1: What is DBSCAN and what problem does it solve?

**Answer:**
DBSCAN is a density-based clustering algorithm that groups together points in high-density regions and marks points in low-density regions as outliers.

**Problems it solves:**
- Finding clusters of arbitrary shape (not just spherical)
- Automatically determining number of clusters
- Identifying noise/outliers naturally
- Handling clusters of different sizes

**Key idea:** Clusters are dense regions separated by sparse regions.

**Follow-up Questions:**
- Why can't K-Means handle arbitrary shapes?
  - Answer: K-Means uses distance to centroids, creating Voronoi cells which are always convex
- What does "density" mean in DBSCAN?
  - Answer: Number of points within a specified radius ε of a point

---

## Q2: Explain the concepts of core points, border points, and noise points.

**Answer:**
**Core Point:** Has at least MinPts neighbors within radius ε (including itself)
- Forms the "backbone" of clusters
- Can reach other core points

**Border Point:** Within ε of a core point but has fewer than MinPts neighbors
- On the edge of clusters
- Not dense enough to be core

**Noise Point:** Neither core nor border
- Not within ε of any core point
- Marked as outliers (label = -1)

**Follow-up Questions:**
- Can a border point belong to multiple clusters?
  - Answer: Technically could be within ε of cores from different clusters; typically assigned to first encountered
- What percentage of points are typically noise?
  - Answer: Depends on data and parameters; 5-15% is common for real-world data with outliers

---

## Q3: Walk through the DBSCAN algorithm step by step.

**Answer:**
**Algorithm:**
1. For each point, find all points within ε distance (ε-neighborhood)
2. Identify core points (those with ≥ MinPts neighbors)
3. For each unvisited core point:
   - Start a new cluster
   - Add all density-reachable points to this cluster
4. Repeat until all core points are visited
5. Assign border points to nearby clusters
6. Label remaining points as noise

**Density reachability:** Point A is density-reachable from B if there's a chain of core points connecting them.

**Follow-up Questions:**
- What order are points processed in?
  - Answer: Arbitrary; different orders can give slightly different border point assignments
- Is DBSCAN deterministic?
  - Answer: Core point assignments are deterministic; border points may vary based on processing order

---

## Q4: What are the hyperparameters of DBSCAN and how do you tune them?

**Answer:**
**1. ε (epsilon):** Radius of neighborhood
- Too small → many noise points, clusters fragment
- Too large → clusters merge, less noise detected

**2. MinPts:** Minimum points to form dense region
- Rule of thumb: MinPts ≥ d + 1 (d = dimensions)
- Common default: MinPts = 4 or 5
- Larger MinPts → smoother clusters, more noise

**Tuning methods:**
- **k-distance plot:** Plot sorted distance to k-th nearest neighbor; look for "elbow" to set ε
- **Domain knowledge:** Set based on expected cluster density
- **Grid search:** Try combinations and evaluate with silhouette score

**Follow-up Questions:**
- What is the k-distance plot method?
  - Answer: For each point, find distance to k-th neighbor (k=MinPts); sort and plot; elbow suggests good ε
- Why MinPts ≥ d + 1?
  - Answer: In d dimensions, need at least d+1 points to define a volume; prevents clusters in low-density projections

---

## Q5: What is the time and space complexity of DBSCAN?

**Answer:**
**Without spatial indexing:**
- Time: O(n²) - computing all pairwise distances
- Space: O(n²) - storing distance matrix

**With spatial indexing (KD-tree, Ball tree):**
- Time: O(n log n) average case
- Space: O(n) - just the index structure

**Note:** Spatial indexing effectiveness degrades in high dimensions.

**Follow-up Questions:**
- When does the O(n log n) complexity hold?
  - Answer: When data has structure that spatial indices can exploit; fails in very high dimensions
- What's the worst case with indexing?
  - Answer: O(n²) if data distribution defeats the index (e.g., all points equidistant)

---

## Q6: What are the advantages of DBSCAN over K-Means?

**Answer:**
| Advantage | DBSCAN | K-Means |
|-----------|--------|---------|
| Cluster shape | Arbitrary | Only spherical |
| Number of clusters | Automatic | Must specify k |
| Outliers | Detects naturally | No outlier handling |
| Cluster sizes | Can vary | Tends toward equal |
| Density | Adapts locally | Assumes uniform |

**Best for DBSCAN:**
- Clusters with complex shapes
- Data with noise/outliers
- Unknown number of clusters
- Varying density clusters

**Follow-up Questions:**
- When would K-Means be better?
  - Answer: When clusters are spherical, similar sized, no outliers, and you know k
- Can DBSCAN find clusters within clusters?
  - Answer: No, standard DBSCAN finds flat partitions; hierarchical DBSCAN (HDBSCAN) can

---

## Q7: What are the limitations of DBSCAN?

**Answer:**
**1. Varying density clusters:** Cannot handle clusters with very different densities
- Dense cluster may be split
- Sparse cluster may be marked as noise

**2. High dimensions:** Distance becomes less meaningful; all points appear equidistant

**3. Parameter sensitivity:** Results depend heavily on ε and MinPts

**4. Border point ambiguity:** Border points may be assigned arbitrarily

**5. Not suitable for:** Completely connected data, very high-dimensional data

**Follow-up Questions:**
- How do you handle varying density?
  - Answer: Use OPTICS or HDBSCAN which adapt density threshold
- At what dimensionality does DBSCAN struggle?
  - Answer: Typically d > 10-20; curse of dimensionality makes density estimation unreliable

---

## Q8: Compare DBSCAN with hierarchical clustering.

**Answer:**
| Aspect | DBSCAN | Hierarchical |
|--------|--------|--------------|
| Result | Flat partition + noise | Dendrogram |
| Complexity | O(n²) or O(n log n) | O(n²) to O(n² log n) |
| Cluster shape | Arbitrary | Depends on linkage |
| Parameters | ε, MinPts | Linkage method, cut height |
| Noise handling | Built-in | No explicit handling |
| Memory | O(n) with indexing | O(n²) for distance matrix |

**Follow-up Questions:**
- Can hierarchical clustering find arbitrary shapes?
  - Answer: Single linkage can, but is sensitive to noise; complete/average linkage tend toward spherical
- Which is more interpretable?
  - Answer: Hierarchical gives dendrogram showing relationships; DBSCAN gives simpler partition with outliers

---

## Q9: What is OPTICS and how does it relate to DBSCAN?

**Answer:**
OPTICS (Ordering Points To Identify the Clustering Structure) is an extension of DBSCAN that handles varying density:

**Key differences:**
- Produces a reachability plot instead of fixed clustering
- Can extract clusters at multiple density levels
- No single ε needed (uses range: ε_max)

**Reachability distance:** min(core-distance, distance to point)
- Creates ordering where nearby points are adjacent
- Valleys in reachability plot indicate clusters

**Follow-up Questions:**
- How do you extract clusters from OPTICS?
  - Answer: Use ξ (steepness) threshold to detect cluster boundaries in reachability plot
- What is HDBSCAN?
  - Answer: Hierarchical DBSCAN; combines DBSCAN with hierarchical approach; extracts optimal flat clustering

---

## Q10: How does DBSCAN handle different distance metrics?

**Answer:**
DBSCAN can use any distance metric:

**Common metrics:**
- **Euclidean:** Default; works for continuous numerical data
- **Manhattan:** Better when features are independent
- **Cosine:** Good for text/high-dimensional sparse data
- **Haversine:** For geographical (lat/lon) data

**Considerations:**
- ε interpretation changes with metric
- Some metrics require different tuning strategies
- Spatial indexing may not work with all metrics

**Follow-up Questions:**
- How do you choose ε for cosine distance?
  - Answer: Cosine distance range is [0, 2]; typical ε values are much smaller (0.1-0.5)
- Can you use custom distance functions?
  - Answer: Yes, but may lose spatial indexing benefits

---

## Q11: How do you evaluate DBSCAN clustering results?

**Answer:**
**Internal metrics (no labels):**
- **Silhouette score:** Exclude noise points; measure cluster quality
- **Davies-Bouldin Index:** Ratio of within to between cluster distances
- **Number of noise points:** Too many suggests poor ε

**External metrics (if labels available):**
- **Adjusted Rand Index (ARI)**
- **Normalized Mutual Information (NMI)**
- **Homogeneity/Completeness**

**Visual inspection:**
- 2D/3D plots (after dimensionality reduction if needed)
- Verify clusters make domain sense

**Follow-up Questions:**
- Should noise points be included in silhouette calculation?
  - Answer: Typically excluded; they're not part of any cluster
- What if DBSCAN finds only noise?
  - Answer: ε too small or MinPts too large; adjust parameters using k-distance plot

---

## Q12: Can DBSCAN be used for anomaly/outlier detection?

**Answer:**
Yes, DBSCAN naturally identifies outliers as noise points (label = -1):

**Advantages for anomaly detection:**
- No need to specify number of normal clusters
- Outliers detected based on density, not distance to center
- Works for complex-shaped normal regions

**Process:**
1. Run DBSCAN on data
2. Points labeled as noise are potential anomalies
3. Adjust ε and MinPts to control sensitivity

**Limitations:**
- Requires tuning parameters appropriately
- May not detect local outliers (anomalies within dense regions)
- Binary output (outlier or not); no anomaly score

**Follow-up Questions:**
- How does this compare to Isolation Forest?
  - Answer: DBSCAN is density-based; Isolation Forest uses tree isolation. IF provides scores; DBSCAN is binary
- Can you get anomaly scores from DBSCAN?
  - Answer: Not directly; but can use distance to nearest core point or Local Outlier Factor for scores
# Anomaly Detection

## Q1: What is the difference between anomalies, outliers, and novelties?

**Answer:**
**Anomaly:** Data point that deviates significantly from expected pattern
- General term for unusual observations
- May indicate errors, fraud, or rare events

**Outlier:** Point far from other observations statistically
- Often used interchangeably with anomaly
- Typically detected using statistical methods

**Novelty:** Previously unseen pattern in new data
- Model trained only on "normal" data
- New point is novel if it differs from training distribution

**Key distinction:**
- Outlier detection: Anomalies exist in training data
- Novelty detection: Training data is "clean"; detect anomalies in new data

**Follow-up Questions:**
- Can novelty detection be used for outlier detection?
  - Answer: If you can identify clean data for training, yes; otherwise outlier detection methods are needed
- Which is more common in practice?
  - Answer: Outlier detection; getting perfectly clean training data is often difficult

---

## Q2: What are the main approaches to anomaly detection?

**Answer:**
**1. Statistical Methods:**
- Z-score, IQR, Mahalanobis distance
- Assume data follows known distribution

**2. Distance-Based:**
- KNN-based, LOF
- Anomalies are far from normal points

**3. Density-Based:**
- DBSCAN noise points, LOF
- Anomalies in low-density regions

**4. Model-Based:**
- Isolation Forest, One-Class SVM
- Build model of normal behavior

**5. Reconstruction-Based:**
- Autoencoders
- Anomalies have high reconstruction error

**Follow-up Questions:**
- Which approach works best for high-dimensional data?
  - Answer: Isolation Forest and Autoencoders; distance-based methods suffer from curse of dimensionality
- Can you combine multiple approaches?
  - Answer: Yes, ensemble methods often improve robustness

---

## Q3: Explain the Z-score method for outlier detection.

**Answer:**
**Formula:**
z = (x - μ) / σ

**Method:**
1. Compute mean (μ) and standard deviation (σ) of data
2. Calculate z-score for each point
3. Flag points with |z| > threshold (typically 2 or 3)

**Interpretation:**
- |z| > 2: Beyond ~95% of data (if normal)
- |z| > 3: Beyond ~99.7% of data

**Limitations:**
- Assumes normal distribution
- Sensitive to outliers (they affect μ and σ)
- Only works for univariate data (or each dimension separately)

**Follow-up Questions:**
- What if data is not normally distributed?
  - Answer: Z-score thresholds may not correspond to expected percentiles; use IQR or transform data
- How do you handle multivariate data?
  - Answer: Use Mahalanobis distance which accounts for correlations

---

## Q4: Explain the IQR (Interquartile Range) method.

**Answer:**
**Formula:**
- IQR = Q3 - Q1 (75th percentile - 25th percentile)
- Lower bound = Q1 - 1.5 × IQR
- Upper bound = Q3 + 1.5 × IQR
- Points outside bounds are outliers

**Advantages:**
- Robust to outliers (uses median-based statistics)
- No distribution assumption
- Simple to compute and interpret

**Limitations:**
- Univariate only
- Fixed threshold (1.5×IQR) may not suit all data
- Assumes symmetric distribution for the 1.5 multiplier

**Follow-up Questions:**
- Why is 1.5 used as the multiplier?
  - Answer: For normal data, 1.5×IQR captures ~99.3% of data; can adjust (e.g., 3×IQR for extreme outliers)
- How does IQR compare to Z-score?
  - Answer: IQR is more robust because quartiles aren't affected by extreme outliers

---

## Q5: What is Mahalanobis distance and when is it useful?

**Answer:**
**Formula:**
D_M(x) = √[(x - μ)ᵀ Σ⁻¹ (x - μ)]

Where Σ is the covariance matrix.

**Interpretation:**
- Measures distance from the mean in units of standard deviation
- Accounts for correlations between features
- Follows chi-squared distribution with d degrees of freedom (for multivariate normal)

**Use cases:**
- Multivariate outlier detection
- When features are correlated
- Assuming data is roughly multivariate normal

**Follow-up Questions:**
- How does it differ from Euclidean distance?
  - Answer: Mahalanobis scales by variance and accounts for correlations; Euclidean treats all directions equally
- What if covariance matrix is singular?
  - Answer: Use regularization (Σ + εI) or dimensionality reduction first

---

## Q6: Explain how Isolation Forest works.

**Answer:**
**Key insight:** Anomalies are "few and different" → easier to isolate.

**Algorithm:**
1. Build random trees by recursively splitting data:
   - Select random feature
   - Select random split value between min and max
   - Partition data
2. Repeat to create forest of isolation trees
3. For each point, compute average path length to isolation
4. Anomaly score based on path length:
   - Short path → easy to isolate → anomaly
   - Long path → hard to isolate → normal

**Score formula:**
s(x, n) = 2^(-E[h(x)]/c(n))

Where h(x) is path length, c(n) is average path length in random tree.

**Follow-up Questions:**
- Why are anomalies isolated quickly?
  - Answer: Random splits are more likely to separate points far from others; anomalies need fewer splits
- What are typical hyperparameters?
  - Answer: Number of trees (100), sample size (256), contamination rate for threshold

---

## Q7: What are the advantages and disadvantages of Isolation Forest?

**Answer:**
**Advantages:**
- Fast: O(n log n) for training
- Scales well to large datasets
- Works in high dimensions (doesn't compute distances)
- Few hyperparameters
- No assumption about data distribution

**Disadvantages:**
- Axis-parallel splits may miss some anomalies
- Performance depends on contamination estimate
- May struggle with local anomalies in dense regions
- Random nature means results vary slightly

**Follow-up Questions:**
- How do you set the contamination parameter?
  - Answer: Estimate proportion of anomalies (e.g., 0.01 for 1%); affects threshold for anomaly/normal classification
- What is Extended Isolation Forest?
  - Answer: Uses hyperplanes instead of axis-parallel splits; better for detecting anomalies in correlated features

---

## Q8: Explain One-Class SVM for anomaly detection.

**Answer:**
**Concept:** Learn a decision boundary around "normal" data; points outside are anomalies.

**Method:**
1. Map data to high-dimensional feature space (using kernel)
2. Find hyperplane that separates data from origin with maximum margin
3. Points on wrong side of hyperplane (or outside boundary) are anomalies

**Key parameter:** ν (nu)
- Controls fraction of outliers and support vectors
- ν is upper bound on fraction of outliers
- ν is lower bound on fraction of support vectors

**Follow-up Questions:**
- What kernel is commonly used?
  - Answer: RBF (Gaussian) kernel; creates flexible boundary around normal data
- When is One-Class SVM preferred?
  - Answer: When you have only normal data for training (novelty detection); works well with moderate dimensions

---

## Q9: What is Local Outlier Factor (LOF)?

**Answer:**
LOF measures local density deviation of a point compared to its neighbors.

**Calculation:**
1. For each point, find k nearest neighbors
2. Compute local reachability density (LRD):
   - LRD(x) = 1 / average reachability-distance to neighbors
3. Compute LOF:
   - LOF(x) = average(LRD(neighbors)) / LRD(x)

**Interpretation:**
- LOF ≈ 1: Similar density to neighbors (normal)
- LOF >> 1: Lower density than neighbors (anomaly)
- LOF << 1: Higher density than neighbors (possible cluster center)

**Follow-up Questions:**
- Why is LOF "local"?
  - Answer: Compares density only to k nearest neighbors, not global density
- When does LOF outperform global methods?
  - Answer: When data has clusters of varying density; global methods may miss local anomalies

---

## Q10: Compare Isolation Forest, One-Class SVM, and LOF.

**Answer:**
| Aspect | Isolation Forest | One-Class SVM | LOF |
|--------|------------------|---------------|-----|
| Type | Model-based | Kernel-based | Distance-based |
| Speed | Fast O(n log n) | Slow O(n² or n³) | O(n² or n log n) |
| Scalability | Excellent | Poor | Moderate |
| High dimensions | Good | Moderate | Poor |
| Local anomalies | Moderate | Poor | Excellent |
| Hyperparameters | Few | Kernel, ν | k neighbors |

**Follow-up Questions:**
- Which should you try first?
  - Answer: Isolation Forest is often good default; fast and works well in many cases
- Can you ensemble these methods?
  - Answer: Yes, majority voting or averaging scores can improve robustness

---

## Q11: How do you evaluate anomaly detection models?

**Answer:**
**With labels (ground truth):**
- Precision, Recall, F1-score
- PR curve (Precision-Recall) - preferred for imbalanced data
- ROC-AUC

**Without labels:**
- Visual inspection (2D/3D plots)
- Domain expert review
- Stability analysis (consistent across subsets?)

**Challenges:**
- Labels often unavailable
- Extreme class imbalance (1% anomalies)
- Accuracy is misleading (99% accuracy by predicting all normal)

**Follow-up Questions:**
- Why is PR curve preferred over ROC for anomaly detection?
  - Answer: ROC can look good even with many false positives when negatives vastly outnumber positives
- What's a good F1 score for anomaly detection?
  - Answer: Depends on domain; 0.5-0.8 is often acceptable given the difficulty

---

## Q12: How do you handle class imbalance in anomaly detection?

**Answer:**
**Problem:** Anomalies are rare (often <1% of data)

**Strategies:**
1. **Use appropriate metrics:** F1, PR-AUC, not accuracy
2. **Adjust threshold:** Lower threshold to catch more anomalies
3. **Use unsupervised methods:** Isolation Forest, LOF don't need labels
4. **Cost-sensitive learning:** Weight misclassified anomalies higher
5. **Sampling:** SMOTE for oversampling (if using supervised methods)

**Follow-up Questions:**
- If 99% normal, what's baseline accuracy?
  - Answer: 99% by predicting all normal; shows why accuracy is misleading
- Should you balance classes for training?
  - Answer: For unsupervised methods, no; for supervised, balancing can help but may introduce bias

---

## Q13: What is the reconstruction error approach to anomaly detection?

**Answer:**
**Concept:** Train model to reconstruct normal data; anomalies have high reconstruction error.

**Using Autoencoders:**
1. Train autoencoder on (assumed) normal data
2. For new data, compute reconstruction: x̂ = decoder(encoder(x))
3. Compute error: ||x - x̂||²
4. High error → anomaly

**Why it works:**
- Autoencoder learns to compress/decompress normal patterns
- Anomalies have patterns not learned → poor reconstruction

**Follow-up Questions:**
- What if training data contains anomalies?
  - Answer: Autoencoder may learn to reconstruct some anomalies; use robust training or pre-filter
- What error threshold to use?
  - Answer: Often percentile-based (e.g., 95th percentile of training errors) or domain-specific

---

## Q14: How do you detect anomalies in time series data?

**Answer:**
**Methods:**
1. **Statistical:** Moving average, exponential smoothing; flag deviations
2. **Decomposition:** Separate trend/seasonality; detect residual anomalies
3. **Prediction-based:** ARIMA, LSTM; high prediction error = anomaly
4. **Distance-based:** DTW distance to historical patterns
5. **Domain-specific:** Define rules for acceptable ranges

**Considerations:**
- Temporal dependencies must be preserved
- Distinguish point anomalies vs collective anomalies (abnormal subsequences)
- Handle seasonality and trends

**Follow-up Questions:**
- What's a point vs collective anomaly?
  - Answer: Point: single unusual value. Collective: normal values that together form unusual pattern
- Can Isolation Forest work on time series?
  - Answer: Not directly; need to create features (rolling stats, lags) first

---

## Q15: How do you choose the right anomaly detection method?

**Answer:**
**Consider:**

| Factor | Recommendation |
|--------|----------------|
| Data size | Large → Isolation Forest; Small → LOF, One-Class SVM |
| Dimensions | High → Isolation Forest; Low → LOF |
| Local anomalies | LOF, DBSCAN |
| Global anomalies | Isolation Forest, statistical |
| Labeled data available | Supervised methods (Random Forest) |
| Need scores | IF, LOF, Autoencoders |
| Speed critical | Isolation Forest |

**General approach:**
1. Start with Isolation Forest (good baseline)
2. If local anomalies matter → try LOF
3. If data is sequential → use time series methods
4. Validate with domain experts

**Follow-up Questions:**
- Should you use multiple methods?
  - Answer: Yes, ensemble or compare; different methods catch different types of anomalies
- How do you explain anomaly predictions?
  - Answer: Look at which features contributed most; use interpretable methods or SHAP values
# Association Rule Mining

## Q1: What is association rule mining and where is it used?

**Answer:**
Association rule mining discovers interesting relationships (associations) between items in large datasets.

**Format of rules:** {A, B} → {C}
- If items A and B are present, then C is likely present

**Applications:**
- **Market basket analysis:** "Customers who buy bread and butter also buy milk"
- **Web usage mining:** Pages frequently visited together
- **Medical diagnosis:** Symptom combinations indicating diseases
- **Recommendation systems:** Product suggestions

**Follow-up Questions:**
- What's the difference between association rules and classification?
  - Answer: Association finds relationships between items (no target); classification predicts a specific target variable
- Can rules have multiple items on both sides?
  - Answer: Yes, {A, B, C} → {D, E} is valid; called multi-consequent rules

---

## Q2: Explain support, confidence, and lift metrics.

**Answer:**
**Support:** How frequently itemset appears
- Support(A) = (Transactions containing A) / (Total transactions)
- Support(A → B) = Support(A ∪ B)

**Confidence:** How often rule is true
- Confidence(A → B) = Support(A ∪ B) / Support(A)
- "Given A, probability of B"

**Lift:** How much more likely B is given A, compared to random
- Lift(A → B) = Confidence(A → B) / Support(B)
- Lift = 1: Independent
- Lift > 1: Positive correlation
- Lift < 1: Negative correlation

**Follow-up Questions:**
- Why isn't confidence enough?
  - Answer: High confidence can be misleading if B is very common; lift normalizes by B's frequency
- What's a "good" lift value?
  - Answer: Depends on domain; lift > 1.5 often considered interesting

---

## Q3: Walk through the Apriori algorithm.

**Answer:**
**Key principle:** If an itemset is infrequent, all its supersets are also infrequent (anti-monotone property).

**Algorithm:**
1. **Generate C1:** Count support of all single items
2. **Prune to L1:** Keep items with support ≥ min_support
3. **Generate C2:** Create candidate pairs from L1
4. **Prune to L2:** Keep pairs with support ≥ min_support
5. **Repeat:** Generate Ck from Lk-1, prune to Lk
6. **Stop:** When no more candidates
7. **Generate rules:** From frequent itemsets, create rules meeting min_confidence

**Follow-up Questions:**
- What does "Apriori" mean?
  - Answer: Uses prior knowledge (if subset is infrequent, superset can't be frequent) to prune search space
- How are candidate itemsets generated?
  - Answer: Join Lk-1 with itself: combine itemsets that share k-2 items

---

## Q4: Explain the anti-monotone (downward closure) property.

**Answer:**
**Property:** If an itemset is infrequent, all its supersets are infrequent.

**Equivalently:** If an itemset is frequent, all its subsets are frequent.

**Example:**
- If {bread} has support 5% (below min_support 10%)
- Then {bread, milk}, {bread, butter}, {bread, milk, butter} are all < 10%
- No need to count these combinations!

**Why it works:** Adding items can only decrease or maintain support, never increase it.

**Follow-up Questions:**
- How much does this property help?
  - Answer: Dramatically reduces search space; without it, would need to check 2^n itemsets
- Are there properties that work upward (for supersets)?
  - Answer: Not for support; but confidence doesn't have anti-monotone property (complicates rule generation)

---

## Q5: What are the limitations of Apriori?

**Answer:**
**1. Multiple database scans:**
- Scans entire database for each level k
- Slow for large databases

**2. Candidate generation overhead:**
- Generates many candidates, many pruned
- Memory intensive for large itemsets

**3. Support threshold sensitivity:**
- Too high → miss interesting rules
- Too low → too many candidates, slow

**4. Only finds frequent patterns:**
- Rare but important associations may be missed

**5. Assumes binary attributes:**
- Items present or absent; doesn't handle quantities

**Follow-up Questions:**
- How does FP-Growth improve on Apriori?
  - Answer: Uses compressed data structure (FP-tree); only 2 database scans; no candidate generation
- What about rare item associations?
  - Answer: Use multiple minimum supports or other algorithms like ECLAT

---

## Q6: How does FP-Growth algorithm work?

**Answer:**
FP-Growth uses a compact data structure (FP-tree) to avoid candidate generation:

**Phase 1: Build FP-tree**
1. Scan database, count item frequencies
2. Remove infrequent items, sort remaining by frequency
3. Second scan: insert transactions into tree (shared prefixes compressed)

**Phase 2: Mine patterns**
1. Start from least frequent item
2. Build conditional pattern base (paths to that item)
3. Construct conditional FP-tree
4. Recursively mine patterns

**Advantages over Apriori:**
- Only 2 database scans
- No candidate generation
- Compressed representation

**Follow-up Questions:**
- What is a conditional pattern base?
  - Answer: All prefix paths to a given item in the FP-tree; represents transactions containing that item
- When does FP-tree not help?
  - Answer: When data is too sparse (tree doesn't compress well) or memory is limited

---

## Q7: How do you generate association rules from frequent itemsets?

**Answer:**
**Process:**
1. For each frequent itemset L with |L| ≥ 2
2. Generate all non-empty subsets S of L
3. Create rule: S → (L - S)
4. Compute confidence = Support(L) / Support(S)
5. Keep rules with confidence ≥ min_confidence

**Example:** Frequent itemset {A, B, C}
- Possible rules: {A} → {B,C}, {B} → {A,C}, {C} → {A,B}, {A,B} → {C}, {A,C} → {B}, {B,C} → {A}
- Each has same support but different confidence

**Follow-up Questions:**
- Do all rules from same itemset have same lift?
  - Answer: No, lift depends on support of both antecedent and consequent
- How many rules can one itemset generate?
  - Answer: 2^k - 2 rules for itemset of size k (exclude empty antecedent and consequent)

---

## Q8: What is the difference between interesting and trivial rules?

**Answer:**
**Trivial rules:** Obvious or expected associations
- Example: {printer} → {printer_ink} (obvious)
- {laptop} → {laptop_charger}

**Interesting rules:** Non-obvious, actionable insights
- Example: {diapers} → {beer} (famous example)
- Requires domain knowledge to evaluate

**Measures of interestingness:**
- **Lift:** Measures deviation from independence
- **Conviction:** Measures dependency strength
- **Leverage:** Difference between observed and expected co-occurrence
- **Domain expertise:** Most important filter

**Follow-up Questions:**
- Can statistical measures identify all interesting rules?
  - Answer: No, interestingness is partly subjective; domain experts must review
- What is the diapers-beer rule?
  - Answer: Famous (possibly apocryphal) finding that young fathers buy diapers and beer together

---

## Q9: How do you choose minimum support and confidence thresholds?

**Answer:**
**Minimum Support:**
- Too high → miss rare but important patterns
- Too low → too many patterns, slow computation
- Start with ~1-5%, adjust based on results

**Minimum Confidence:**
- Too high → only obvious rules
- Too low → unreliable rules
- Start with 50-70%

**Guidelines:**
- Consider item frequency distribution
- Rare items need lower support to be found
- Use lift to filter rules after generation
- Domain knowledge guides final threshold

**Follow-up Questions:**
- Can you use different thresholds for different items?
  - Answer: Yes, "multiple minimum support" allows rare items to have lower thresholds
- What if no rules are found?
  - Answer: Lower thresholds, or data may not have strong associations

---

## Q10: Compare association rules with other recommendation approaches.

**Answer:**
| Aspect | Association Rules | Collaborative Filtering |
|--------|-------------------|------------------------|
| Data needed | Transaction data | User-item ratings |
| Output | If-then rules | Personalized recommendations |
| Interpretability | High (explicit rules) | Lower (latent factors) |
| Cold start | Works for new users | Struggles with new users |
| Personalization | Limited (same rules for all) | High (per-user) |
| Computation | Mining phase then fast | Often real-time computation |

**When to use association rules:**
- Product bundling decisions
- Store layout optimization
- Simple recommendations ("bought together")
- When interpretability matters

**Follow-up Questions:**
- Can association rules personalize recommendations?
  - Answer: Limited; can filter rules based on user's current basket but same rules for all
- How do you handle temporal patterns?
  - Answer: Sequential pattern mining extends association rules to ordered events
# Recommendation Systems

## Q1: What are the main types of recommendation systems?

**Answer:**
**1. Collaborative Filtering:**
- Uses user behavior (ratings, clicks) to find similar users or items
- Types: User-based, Item-based, Matrix Factorization

**2. Content-Based Filtering:**
- Uses item attributes to recommend similar items
- Based on user's past preferences

**3. Hybrid Systems:**
- Combine collaborative and content-based approaches
- Often best performance in practice

**4. Knowledge-Based:**
- Uses explicit knowledge about items and user needs
- Good for complex domains (cars, real estate)

**Follow-up Questions:**
- Which type is most common in practice?
  - Answer: Hybrid systems; they mitigate weaknesses of individual approaches
- Can deep learning be used for recommendations?
  - Answer: Yes, deep learning models can learn complex patterns from user behavior and item features

---

## Q2: Explain user-based collaborative filtering.

**Answer:**
**Concept:** Recommend items liked by similar users.

**Steps:**
1. Build user-item rating matrix
2. Find similar users to target user (using similarity metric)
3. Aggregate ratings from similar users for unrated items
4. Recommend items with highest predicted ratings

**Prediction formula:**
r̂(u,i) = r̄ᵤ + [Σᵥ∈N(u) sim(u,v)(rᵥᵢ - r̄ᵥ)] / [Σᵥ∈N(u) |sim(u,v)|]

**Follow-up Questions:**
- What makes two users "similar"?
  - Answer: High correlation or cosine similarity in their rating patterns
- What's the main disadvantage?
  - Answer: Scalability; computing user similarities is expensive for many users (O(|U|²))

---

## Q3: Explain item-based collaborative filtering.

**Answer:**
**Concept:** Recommend items similar to what user has liked.

**Steps:**
1. Build item-item similarity matrix
2. For target user, find items they've rated highly
3. Find similar items to those (using similarity metric)
4. Recommend most similar items not yet rated

**Prediction formula:**
r̂(u,i) = [Σⱼ∈R(u) sim(i,j) × rᵤⱼ] / [Σⱼ∈R(u) |sim(i,j)|]

**Advantages over user-based:**
- Item similarities are more stable (items don't change)
- Can precompute item similarities
- Often better for many users, fewer items

**Follow-up Questions:**
- Which is better: user-based or item-based?
  - Answer: Depends on domain; item-based often preferred when items < users; Netflix used item-based
- How do you compute item similarity?
  - Answer: Based on users who rated both items; adjusted cosine similarity is common

---

## Q4: What is cosine similarity and how is it used in recommendations?

**Answer:**
**Formula:**
cos(u, v) = (u · v) / (||u|| × ||v||) = Σuᵢvᵢ / (√Σuᵢ² × √Σvᵢ²)

**Range:** -1 to 1 (for ratings centered at 0); 0 to 1 (for non-negative values)

**In recommendations:**
- Vectors are user ratings or item features
- Measures angle between vectors (direction similarity)
- Ignores magnitude (a user who rates everything high vs low)

**Follow-up Questions:**
- Why use cosine over Euclidean distance?
  - Answer: Cosine is invariant to rating scale; users with same preferences but different rating tendencies will be similar
- What is adjusted cosine similarity?
  - Answer: Subtract user's mean rating before computing cosine; handles user bias

---

## Q5: What is Pearson correlation and how does it differ from cosine similarity?

**Answer:**
**Pearson correlation:**
r(u, v) = Σ(uᵢ - ū)(vᵢ - v̄) / (√Σ(uᵢ - ū)² × √Σ(vᵢ - v̄)²)

**Key difference:** Pearson centers data by subtracting means.

**Comparison:**
| Aspect | Cosine | Pearson |
|--------|--------|---------|
| Mean centering | No | Yes |
| Rating bias | Affected | Not affected |
| Interpretation | Vector angle | Linear correlation |
| Range | [-1, 1] or [0, 1] | [-1, 1] |

**Follow-up Questions:**
- When is Pearson preferred?
  - Answer: When users have different rating tendencies (some rate high, some low)
- Are they ever equivalent?
  - Answer: Yes, when data is mean-centered, cosine = Pearson

---

## Q6: What is the cold start problem?

**Answer:**
**Definition:** Difficulty recommending for new users or new items with no history.

**Types:**
1. **New user:** No ratings/interactions to find similar users
2. **New item:** No ratings to determine item similarity
3. **New system:** No data at all initially

**Solutions:**
- **Content-based features:** Use item attributes, user demographics
- **Ask for preferences:** Onboarding questionnaire
- **Popular items:** Recommend trending/popular items
- **Explore/exploit:** Balance between known preferences and discovery
- **Hybrid approaches:** Combine collaborative with content-based

**Follow-up Questions:**
- Which type of cold start is harder to solve?
  - Answer: New user; at least new items have attributes. New users may have nothing
- How do companies like Spotify handle cold start?
  - Answer: Ask for favorite artists/genres during signup; use content features (audio analysis)

---

## Q7: Explain content-based filtering.

**Answer:**
**Concept:** Recommend items similar to what user liked, based on item attributes.

**Steps:**
1. Create item profiles (features/attributes)
2. Build user profile from items they've liked
3. Match user profile with item profiles
4. Recommend items with highest similarity to user profile

**Item features examples:**
- Movies: Genre, director, actors, keywords
- Music: Genre, tempo, artist, audio features
- Products: Category, brand, price range, description

**Follow-up Questions:**
- What are advantages over collaborative filtering?
  - Answer: No cold start for new items (if features available); can explain recommendations; works with single user
- What are limitations?
  - Answer: Limited to item features (can't capture serendipity); needs good feature engineering; overspecialization

---

## Q8: What is the overspecialization problem in content-based filtering?

**Answer:**
**Problem:** System only recommends items very similar to what user has seen before.

**Consequences:**
- No diversity in recommendations
- User stuck in "filter bubble"
- Misses items user might like from different categories
- No serendipitous discovery

**Solutions:**
- Add randomness/exploration
- Hybrid with collaborative filtering
- Diversity constraints in recommendation list
- Periodic re-exploration of user preferences

**Follow-up Questions:**
- How does collaborative filtering help with this?
  - Answer: CF can recommend items liked by similar users even if different from user's typical preferences
- What is a filter bubble?
  - Answer: When recommendations reinforce existing preferences, limiting exposure to diverse content

---

## Q9: What is matrix factorization for recommendations?

**Answer:**
**Concept:** Decompose user-item rating matrix into lower-dimensional user and item factors.

**R ≈ U × V^T**
- R: m×n rating matrix (users × items)
- U: m×k user factor matrix
- V: n×k item factor matrix
- k: number of latent factors (typically 20-200)

**Prediction:**
r̂(u,i) = uᵤ · vᵢ = Σₖ uᵤₖ × vᵢₖ

**Interpretation:** 
- Latent factors capture hidden dimensions (e.g., genres, quality)
- User factors: how much user likes each factor
- Item factors: how much item exhibits each factor

**Follow-up Questions:**
- Why is this better than user/item-based CF?
  - Answer: More scalable; handles sparsity better; captures latent patterns
- How is the factorization learned?
  - Answer: Minimize reconstruction error (often RMSE) using SGD or ALS

---

## Q10: What is SVD and how is it used in recommendations?

**Answer:**
**SVD (Singular Value Decomposition):**
R = UΣV^T

**For recommendations:**
- Can't apply directly (rating matrix has missing values)
- "Funk SVD" approximates by minimizing error on known ratings only

**Optimization:**
min Σ(u,i)∈known (rᵤᵢ - uᵤ·vᵢ)² + λ(||U||² + ||V||²)

- First term: fit known ratings
- Second term: regularization to prevent overfitting

**Follow-up Questions:**
- Why can't we use standard SVD?
  - Answer: Standard SVD requires complete matrix; filling missing values with 0 or mean distorts patterns
- What is ALS (Alternating Least Squares)?
  - Answer: Fix U, optimize V; then fix V, optimize U; alternate until convergence

---

## Q11: How do you evaluate recommendation systems?

**Answer:**
**Rating prediction (accuracy):**
- RMSE: √(Σ(r - r̂)²/n)
- MAE: Σ|r - r̂|/n

**Ranking quality:**
- Precision@k: Relevant items in top k / k
- Recall@k: Relevant items in top k / total relevant
- MAP (Mean Average Precision)
- NDCG (Normalized Discounted Cumulative Gain)

**Business metrics:**
- Click-through rate (CTR)
- Conversion rate
- User engagement

**Follow-up Questions:**
- Why is ranking often more important than rating accuracy?
  - Answer: Users see top recommendations; getting the order right matters more than exact predicted ratings
- What is NDCG?
  - Answer: Measures ranking quality giving more weight to items at top positions; accounts for graded relevance

---

## Q12: What is implicit feedback and how does it differ from explicit?

**Answer:**
**Explicit feedback:** Direct user ratings (1-5 stars, thumbs up/down)
- Clear signal of preference
- Sparse (users rate few items)

**Implicit feedback:** Inferred from behavior (clicks, views, purchases, time spent)
- Abundant data
- Noisy (watching ≠ liking)
- Only positive signals (no "dislike" data)

**Handling implicit feedback:**
- Treat as confidence rather than preference
- Use specialized models (e.g., Implicit Matrix Factorization)
- Sample negative examples (items not interacted with)

**Follow-up Questions:**
- Which is more valuable?
  - Answer: Implicit is more abundant; explicit is cleaner. Best systems use both.
- How do you handle missing implicit feedback?
  - Answer: Missing = negative (with low confidence) or use negative sampling during training

---

## Q13: What is the Netflix Prize problem?

**Answer:**
**Competition (2006-2009):**
- Goal: Improve Netflix's recommendation algorithm by 10%
- Prize: $1 million
- Metric: RMSE on held-out ratings

**Key learnings:**
1. Matrix factorization works well
2. Ensemble of diverse models performs best
3. Temporal dynamics matter (preferences change)
4. Implicit feedback (what users watch) helps
5. 10% improvement is very hard

**Winning solution:**
- Blended 800+ models
- Used SVD++, RBM, k-NN, temporal models
- Final improvement: 10.06%

**Follow-up Questions:**
- Why didn't Netflix deploy the winning solution?
  - Answer: Too complex for production; marginal improvement didn't justify engineering cost
- What is SVD++?
  - Answer: Extension of SVD that incorporates implicit feedback (which items user rated, not just how)

---

## Q14: How do hybrid recommendation systems work?

**Answer:**
**Combination strategies:**

1. **Weighted:** Combine scores from multiple systems
   - r̂ = α × r̂_CF + (1-α) × r̂_content

2. **Switching:** Use different system based on context
   - Use content-based for new users, CF for established users

3. **Feature combination:** Use features from both as input to single model
   - Collaborative features + item features → ML model

4. **Cascade:** Use one system to filter, another to rank
   - Content-based pre-filters, CF ranks

5. **Meta-level:** One system generates features for another
   - Content model creates user profile, used by CF

**Follow-up Questions:**
- Which approach is most common?
  - Answer: Weighted and feature combination are popular; deep learning often does feature combination implicitly
- How do you tune the combination?
  - Answer: Cross-validation; optimize business metrics on validation set

---

## Q15: What are the scalability challenges in recommendation systems?

**Answer:**
**Challenges:**
1. **Number of users/items:** Millions of users, millions of items
2. **Sparsity:** Most user-item pairs have no interaction
3. **Real-time requirements:** Recommendations needed in milliseconds
4. **Model updates:** New ratings arrive constantly

**Solutions:**
1. **Approximate methods:** Locality-sensitive hashing for similar item lookup
2. **Matrix factorization:** Compact representations, O(k) per prediction
3. **Pre-computation:** Compute recommendations offline, serve from cache
4. **Incremental updates:** Update models without full retraining
5. **Distributed computing:** Spark, parameter servers

**Follow-up Questions:**
- How does YouTube handle billions of videos?
  - Answer: Two-stage: candidate generation (retrieve 100s from millions) then ranking (score candidates)
- What is approximate nearest neighbor (ANN)?
  - Answer: Fast similarity search that trades exactness for speed; used to find similar items/users

---

## Q16: How do you handle the popularity bias in recommendations?

**Answer:**
**Problem:** Popular items get recommended more → more interactions → even more recommendations (rich get richer).

**Consequences:**
- Long tail items never recommended
- Reduced diversity
- May not serve niche user preferences

**Solutions:**
1. **Inverse propensity scoring:** Weight rare item interactions higher
2. **Calibration:** Ensure recommendations reflect user's genre distribution
3. **Diversity constraints:** Enforce variety in recommendation list
4. **Multi-objective optimization:** Balance relevance and diversity
5. **Popularity debiasing:** Separate popularity from quality in models

**Follow-up Questions:**
- Is popularity bias always bad?
  - Answer: No, popular items are often good; but over-emphasis hurts discovery and long-tail items
- What is the "long tail"?
  - Answer: Many items with few interactions; collectively significant but individually rare

---

## Q17: What is contextual recommendation?

**Answer:**
**Context:** Additional information beyond user and item
- Time: Morning vs evening, weekday vs weekend
- Location: Home vs work vs traveling
- Device: Mobile vs desktop
- Social: Alone vs with friends
- Mood: Explicit or inferred

**Approaches:**
1. **Pre-filtering:** Filter data by context before modeling
2. **Post-filtering:** Adjust recommendations based on context
3. **Contextual modeling:** Include context as features in model

**Example:** Music recommendations
- Morning: Energetic songs
- Night: Relaxing music
- Workout: High tempo

**Follow-up Questions:**
- How do you incorporate time in recommendations?
  - Answer: Time of day as feature, temporal decay of preferences, session-based modeling
- What is session-based recommendation?
  - Answer: Recommend based on current session behavior, not historical profile; uses RNNs or attention

---

## Q18: What are embedding-based recommendations?

**Answer:**
**Concept:** Learn dense vector representations (embeddings) for users and items.

**Methods:**
1. **Word2Vec style:** Train on interaction sequences (item2vec)
2. **Neural collaborative filtering:** Deep networks learn embeddings
3. **Graph embeddings:** Learn from user-item interaction graph

**Advantages:**
- Captures complex patterns
- Handles sparse data well
- Easy to incorporate side information
- Can use pre-trained embeddings (from content)

**Prediction:**
r̂(u,i) = f(eᵤ, eᵢ) where e is embedding

**Follow-up Questions:**
- How do embeddings differ from matrix factorization factors?
  - Answer: Similar concept; MF factors are embeddings. Deep learning can learn more complex embeddings
- Can you use content embeddings (e.g., BERT)?
  - Answer: Yes, pre-trained embeddings from descriptions/images can address cold start

---

## Q19: Explain the explore-exploit tradeoff in recommendations.

**Answer:**
**Exploitation:** Recommend items likely to be liked (based on current knowledge)
**Exploration:** Recommend items to learn user preferences

**Why exploration matters:**
- User preferences may change
- Model may be wrong about some items
- New items need exposure to gather feedback

**Approaches:**
1. **Epsilon-greedy:** With probability ε, recommend random item
2. **Thompson sampling:** Sample from posterior distribution of item quality
3. **UCB (Upper Confidence Bound):** Recommend items with high uncertainty
4. **Contextual bandits:** Balance based on context

**Follow-up Questions:**
- How do you balance the tradeoff?
  - Answer: Depends on business goals; more exploration early, more exploitation as model improves
- What is a multi-armed bandit?
  - Answer: Framework for sequential decision-making under uncertainty; each item is an "arm" to pull

---

## Q20: How do you explain recommendations to users?

**Answer:**
**Why explainability matters:**
- Builds user trust
- Helps users decide faster
- Provides transparency
- Improves user experience

**Explanation types:**
1. **User-based:** "Users similar to you liked this"
2. **Item-based:** "Because you liked X"
3. **Feature-based:** "This matches your preference for action movies"
4. **Social:** "Your friend liked this"

**Challenges:**
- Latent factor models are hard to explain
- Trade-off between accuracy and explainability
- Explanations may be post-hoc rationalizations

**Follow-up Questions:**
- Do explanations improve recommendation effectiveness?
  - Answer: Studies show yes; users are more likely to engage with explained recommendations
- How do you explain matrix factorization recommendations?
  - Answer: Difficult; can use post-hoc methods like "similar users" or "similar items" explanations
# Matrix Factorization

## Q1: What is matrix factorization and why is it useful?

**Answer:**
Matrix factorization decomposes a matrix into a product of two or more matrices with lower rank.

**R ≈ U × V^T**
- R: Original matrix (m × n)
- U: Factor matrix (m × k)
- V: Factor matrix (n × k)
- k: Rank (k << min(m, n))

**Why useful:**
- **Dimensionality reduction:** Compress data while preserving structure
- **Missing value handling:** Fill in missing entries
- **Latent factor discovery:** Uncover hidden patterns
- **Noise reduction:** Low-rank approximation filters noise

**Applications:** Recommendations, topic modeling, image compression, NLP

**Follow-up Questions:**
- What is the rank of a matrix?
  - Answer: Maximum number of linearly independent rows (or columns); represents the "intrinsic dimensionality"
- Why does low rank help with missing values?
  - Answer: Assumes data has low-dimensional structure; factors learned from observed entries can predict missing ones

---

## Q2: Explain Singular Value Decomposition (SVD).

**Answer:**
SVD decomposes any m×n matrix A into:
**A = UΣV^T**

Where:
- U: m×m orthogonal matrix (left singular vectors)
- Σ: m×n diagonal matrix (singular values, σ₁ ≥ σ₂ ≥ ... ≥ 0)
- V: n×n orthogonal matrix (right singular vectors)

**Properties:**
- Always exists for any matrix
- Singular values indicate importance of components
- U columns: directions in row space
- V columns: directions in column space

**Follow-up Questions:**
- How does SVD relate to eigendecomposition?
  - Answer: A^T A = VΣ²V^T (V contains eigenvectors); AA^T = UΣ²U^T (U contains eigenvectors)
- What does it mean for U and V to be orthogonal?
  - Answer: Columns are unit length and perpendicular; U^T U = I, V^T V = I

---

## Q3: What is Truncated SVD and how does it differ from full SVD?

**Answer:**
**Truncated SVD:** Keep only top k singular values/vectors.

**A ≈ Aₖ = Uₖ Σₖ Vₖ^T**

Where:
- Uₖ: m×k (first k columns of U)
- Σₖ: k×k (top k singular values)
- Vₖ: n×k (first k columns of V)

**Properties:**
- Best rank-k approximation (minimizes ||A - Aₖ||_F)
- Compression: Store mk + k + nk instead of mn
- Error: ||A - Aₖ||_F = √(σ²ₖ₊₁ + ... + σ²ᵣ)

**Follow-up Questions:**
- How do you choose k?
  - Answer: Based on cumulative variance explained (Σᵢ₌₁ᵏ σᵢ² / Σᵢ σᵢ²), scree plot, or downstream task performance
- What is the Eckart-Young theorem?
  - Answer: States truncated SVD gives the best rank-k approximation in both Frobenius and spectral norm

---

## Q4: How is SVD used in recommender systems?

**Answer:**
**Challenge:** User-item rating matrix has many missing values; standard SVD requires complete matrix.

**Approaches:**

1. **Imputation + SVD:** Fill missing values (e.g., with mean), then apply SVD
   - Simple but distorts patterns

2. **Funk SVD (Matrix Factorization):**
   - Factorize R ≈ UV^T
   - Minimize error only on known ratings
   - Learn U, V via gradient descent

**Optimization:**
min Σ(u,i)∈observed (rᵤᵢ - uᵤ · vᵢ)² + λ(||U||² + ||V||²)

**Follow-up Questions:**
- Why not use standard SVD directly?
  - Answer: Standard SVD can't handle missing values; treating them as 0 or mean introduces bias
- What do the factors represent?
  - Answer: Latent features - user factors show preferences, item factors show characteristics

---

## Q5: Explain the optimization methods for matrix factorization.

**Answer:**
**1. Stochastic Gradient Descent (SGD):**
- Update factors for each observed rating
- uᵤ ← uᵤ + η(eᵤᵢ · vᵢ - λuᵤ)
- vᵢ ← vᵢ + η(eᵤᵢ · uᵤ - λvᵢ)
- Where eᵤᵢ = rᵤᵢ - uᵤ · vᵢ

**2. Alternating Least Squares (ALS):**
- Fix U, solve for V (closed form)
- Fix V, solve for U (closed form)
- Alternate until convergence

**Comparison:**
| SGD | ALS |
|-----|-----|
| Online updates | Batch updates |
| Simple implementation | Parallelizable |
| Learning rate tuning | No learning rate |
| Faster per iteration | Better for implicit feedback |

**Follow-up Questions:**
- When is ALS preferred?
  - Answer: When parallelization matters (distributed systems) or for implicit feedback
- What is the closed-form solution in ALS?
  - Answer: Uᵤ = (VᵀV + λI)⁻¹ Vᵀ Rᵤ for each user row

---

## Q6: What is Non-negative Matrix Factorization (NMF)?

**Answer:**
NMF factorizes a non-negative matrix into non-negative factors:

**V ≈ WH** where V, W, H ≥ 0

**Properties:**
- Parts-based representation (additive, no cancellation)
- More interpretable than SVD
- Useful when negatives don't make sense

**Applications:**
- **Topic modeling:** V = documents×words, W = documents×topics, H = topics×words
- **Image analysis:** Parts of faces, objects
- **Audio:** Spectrograms, source separation

**Follow-up Questions:**
- Why does non-negativity help interpretability?
  - Answer: Factors represent additive contributions; in topic modeling, words have positive association with topics
- How does NMF differ from SVD for the same k?
  - Answer: NMF may have higher reconstruction error but more interpretable factors; SVD allows negative values

---

## Q7: What are the different loss functions for matrix factorization?

**Answer:**
**1. Frobenius norm (squared error):**
L = ||R - UV^T||²_F = Σ(rᵤᵢ - uᵤ·vᵢ)²
- Common, easy to optimize
- Assumes Gaussian noise

**2. KL divergence:**
L = Σ(rᵤᵢ log(rᵤᵢ/(uᵤ·vᵢ)) - rᵤᵢ + uᵤ·vᵢ)
- For count data
- Used in NMF for topic modeling

**3. Weighted loss:**
L = Σ wᵤᵢ(rᵤᵢ - uᵤ·vᵢ)²
- Different weights for different entries
- Useful for implicit feedback

**4. Ranking loss (BPR):**
L = -Σ log σ(uᵤ·vᵢ - uᵤ·vⱼ)
- Optimizes pairwise ranking
- i = positive item, j = negative item

**Follow-up Questions:**
- When would you use ranking loss over squared error?
  - Answer: When you care about the order of recommendations, not exact rating prediction
- What is BPR?
  - Answer: Bayesian Personalized Ranking; optimizes that positive items rank higher than negative

---

## Q8: How do you handle implicit feedback in matrix factorization?

**Answer:**
**Implicit feedback:** Binary or count data (clicks, views, purchases) not explicit ratings.

**Challenges:**
- Only positive signals (no explicit negatives)
- Missing = negative or just unknown?
- Varying confidence levels

**Approaches:**

1. **Weighted Matrix Factorization:**
- Treat all entries (observed=1, unobserved=0)
- Weight observed entries higher: cᵤᵢ = 1 + α·rᵤᵢ
- min Σcᵤᵢ(pᵤᵢ - uᵤ·vᵢ)²

2. **BPR (Bayesian Personalized Ranking):**
- Sample positive and negative pairs
- Optimize pairwise ranking

**Follow-up Questions:**
- Why weight observed entries higher?
  - Answer: More confident that interaction means preference; unobserved could be negative or unknown
- What is the iALS algorithm?
  - Answer: Implicit ALS; efficient algorithm for weighted matrix factorization using ALS

---

## Q9: What regularization techniques are used in matrix factorization?

**Answer:**
**1. L2 Regularization (Ridge):**
L = Σ(rᵤᵢ - uᵤ·vᵢ)² + λ(||U||²_F + ||V||²_F)
- Prevents large factor values
- Most common

**2. L1 Regularization (Lasso):**
- Encourages sparsity in factors
- Harder to optimize

**3. Bias terms:**
r̂ᵤᵢ = μ + bᵤ + bᵢ + uᵤ·vᵢ
- μ: global mean
- bᵤ: user bias
- bᵢ: item bias
- Captures baseline effects

**4. Temporal regularization:**
- Smooth changes in factors over time

**Follow-up Questions:**
- Why are bias terms important?
  - Answer: Capture that some users rate high/low overall, some items are generally better/worse
- How do you tune λ?
  - Answer: Cross-validation; grid search or Bayesian optimization

---

## Q10: How does matrix factorization relate to PCA?

**Answer:**
**Similarities:**
- Both find low-rank approximations
- Both use SVD (or similar decomposition)
- Both reduce dimensionality

**Differences:**
| PCA | Matrix Factorization (Recommendations) |
|-----|---------------------------------------|
| Complete data | Missing values |
| Centered data | Raw ratings or biased |
| Minimize reconstruction error | Minimize on observed only |
| Unsupervised | Can incorporate side info |
| Orthogonal factors | Not necessarily orthogonal |

**Connection:**
- PCA on centered complete matrix ≈ SVD
- MF for recommendations is like "PCA for incomplete matrices"

**Follow-up Questions:**
- Can PCA handle missing values?
  - Answer: Not directly; need variants like PPCA (Probabilistic PCA) or EM-based methods
- Which gives more interpretable factors?
  - Answer: Neither is guaranteed interpretable; NMF often more interpretable than both

---

## Q11: What is SVD++ and how does it improve recommendations?

**Answer:**
SVD++ extends basic matrix factorization by incorporating implicit feedback:

**Prediction:**
r̂ᵤᵢ = μ + bᵤ + bᵢ + vᵢ · (pᵤ + |N(u)|^(-1/2) Σⱼ∈N(u) yⱼ)

Where:
- pᵤ: explicit user factor
- yⱼ: implicit factor for item j
- N(u): items user u has interacted with

**Intuition:** 
- User preference = explicit preferences + what they've consumed
- "You are what you eat" - implicit history reveals preferences

**Improvement:** Typically 1-2% RMSE improvement over basic SVD.

**Follow-up Questions:**
- Why does implicit history help?
  - Answer: Even without ratings, which items a user interacted with reveals preferences
- Is SVD++ computationally expensive?
  - Answer: Yes, more complex; slower training but better predictions

---

## Q12: How do you evaluate matrix factorization models?

**Answer:**
**Accuracy metrics:**
- RMSE: √(Σ(r - r̂)²/n) - common for rating prediction
- MAE: Σ|r - r̂|/n

**Ranking metrics:**
- Precision@k, Recall@k
- NDCG, MAP
- AUC (for pairwise ranking)

**Evaluation strategies:**
1. **Random split:** Random train/test
2. **Time-based split:** Train on past, test on future
3. **Leave-one-out:** Hide one rating per user

**Follow-up Questions:**
- Why is time-based split more realistic?
  - Answer: Simulates production; predicting future ratings from past, not random held-out
- What's the difference between RMSE and ranking metrics?
  - Answer: RMSE measures rating accuracy; ranking measures if good items are ranked high (often more relevant)

---

## Q13: What are the scalability challenges in matrix factorization?

**Answer:**
**Challenges:**
1. **Large matrices:** Billions of users × millions of items
2. **Many parameters:** (m + n) × k factors to learn
3. **Sparse data:** Most entries missing
4. **Real-time updates:** New users/ratings continuously

**Solutions:**
1. **Distributed computing:** Parallel ALS (Spark MLlib)
2. **Stochastic methods:** SGD processes one rating at a time
3. **Approximate methods:** Randomized SVD
4. **Incremental updates:** Add new factors without retraining
5. **Dimensionality reduction:** Lower k trades accuracy for speed

**Follow-up Questions:**
- How does Spark MLlib implement matrix factorization?
  - Answer: Distributed ALS; partitions U and V across workers; efficient for large sparse matrices
- What is randomized SVD?
  - Answer: Uses random projections to approximate SVD; O(mnk) instead of O(mn·min(m,n))

---

## Q14: How do you incorporate side information into matrix factorization?

**Answer:**
**Side information:** Additional features about users or items.

**Approaches:**

1. **Feature-augmented MF:**
r̂ᵤᵢ = uᵤ·vᵢ + wᵤ·xᵢ + zᵢ·fᵤ
- xᵢ: item features
- fᵤ: user features
- w, z: learned projection matrices

2. **Factorization Machines:**
- General framework for feature interactions
- Models all pairwise feature interactions

3. **Deep learning approaches:**
- Neural networks combine embeddings and features
- More flexible but complex

**Benefits:**
- Better cold start handling
- Improved accuracy
- More interpretable factors

**Follow-up Questions:**
- How do Factorization Machines generalize MF?
  - Answer: FM treats user and item IDs as features; interaction between user and item features = standard MF
- When is side information most valuable?
  - Answer: Cold start (new users/items), sparse data, or when features are highly informative

---

## Q15: What are recent advances in matrix factorization?

**Answer:**
**1. Neural Collaborative Filtering:**
- Replace dot product with neural network
- r̂ = f(uᵤ, vᵢ) where f is MLP
- More expressive but harder to train

**2. Variational Autoencoders:**
- Probabilistic approach to MF
- Better uncertainty estimation
- Handles missing data naturally

**3. Graph Neural Networks:**
- Model user-item interactions as graph
- Learn embeddings via message passing
- Captures higher-order relationships

**4. Attention mechanisms:**
- Learn which factors are important
- Dynamic weighting based on context

**5. Temporal models:**
- Factors evolve over time
- Better for dynamic preferences

**Follow-up Questions:**
- Do neural methods always beat simple MF?
  - Answer: Not always; simple MF is often competitive with proper tuning; neural methods shine with lots of data
- What is the "embedding layer" in deep recommenders?
  - Answer: Lookup table that converts user/item IDs to dense vectors (equivalent to factor matrices)
