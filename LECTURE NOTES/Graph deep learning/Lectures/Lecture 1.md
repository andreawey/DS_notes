# Graph Deep Learning Cheatsheet

## Course Overview

- **Professor**: Cesare Alippi (Università della Svizzera italiana)
- **Evaluation**: 80% Project + 20% Quiz
- **Prerequisites**: Machine Learning, Deep Learning Lab

---

## Why Graphs?

### Natural Applications

- Social networks, molecular structures, knowledge graphs
- **Latent graphs**: Derived from timeseries/signals using correlation
- **Inductive bias**: Graphs provide structural assumptions for learning

### Graph Representation

- **Adjacency Matrix (A)**: Binary matrix where $A_{i,j} = 1$ if edge exists
- **Node Features (X)**: $x_i \in \mathbb{R}^{d_v}$ for node $i$
- **Edge Features (E)**: $e_{i,j} \in \mathbb{R}^{d_e}$ for edge $(i,j)$

---

## Linear Algebra Foundations

### Eigenvalues & Eigenvectors

$$\lambda x = Ax$$ $$\det(A - \lambda I) = 0$$

### Spectral Theorem

For symmetric matrix $A$: $$A = \sum_{i=1}^n \lambda_i u_{\lambda_i} u_{\lambda_i}^T$$

### Graph Laplacian

$$L = D - A$$ where $D$ is degree matrix: $d_i = \sum_j A_{i,j}$

**Properties**:

- Symmetric and positive semidefinite
- Number of zero eigenvalues = number of connected components
- Eigenvectors represent graph frequencies

---

## Graph Convolutions

### Graph Shift Operator (GSO)

Matrix $\tilde{A}$ where $\tilde{a}_{ij} = 0$ for $(i,j) \notin E$ and $i \neq j$

**Examples**:

- Laplacian: $\tilde{A} = L = D - A$
- Random-walk: $\tilde{A} = D^{-1}A$

### Basic Graph Convolution

$$H = \sigma(\tilde{A}X\Theta)$$ where:

- $\tilde{A}$: Graph Shift Operator
- $X$: Node features
- $\Theta$: Learnable parameters
- $\sigma$: Activation function

### K-th Order Filters

**Polynomial filters**: $$H^{(K)} = \sum_{k=0}^K \tilde{A}^k X\Theta^{(k)}$$

**Sequential filters**: $$H^{(0)} = X, \quad H^{(k)} = \tilde{A}H^{(k-1)}\Theta^{(k)}$$

---

## Popular GNN Variants

### GCN (Graph Convolutional Network)

$$\tilde{A} = D^{-1/2}(I_N + A)D^{-1/2}$$

### GIN (Graph Isomorphism Network)

$$\tilde{A} = A + (1 + \epsilon) \cdot I_N$$

### Diffusion Convolution

$$\tilde{A} = D^{-1}A$$

---

## Message Passing Framework

### General Form

$$h_i = \gamma\left(x_i, \text{Aggr}_{j \in \mathcal{N}(i)} {\phi(x_i, x_j, e_{ji})}\right)$$

where:

- $\phi$: Message function
- $\text{Aggr}$: Aggregation (sum, mean, max)
- $\gamma$: Update function

### Types

- **Isotropic**: Message depends only on sender
- **Anisotropic**: Message depends on sender, receiver, and/or edge features

---

## Graph Attention Networks (GAT)

1. **Transform**: $x'_i = x_i \Theta_1$
2. **Attention scores**: $\alpha_{ji} = \sigma([x'_i | x'_j]\theta_2)$
3. **Normalize**: $\tilde{\alpha}_{ji} = \frac{\exp(\alpha_{ji})}{\sum_{k \in \mathcal{N}(i)} \exp(\alpha_{ki})}$
4. **Aggregate**: $h_i = \sum_{j \in \mathcal{N}(i)} \tilde{\alpha}_{ji} x'_j$

---

## Edge-Conditioned Convolution

$$\Theta_{ji} = \rho(e_{ji})$$ $$h_i = x_i \Theta_i + \sum_{j \in \mathcal{N}(i)} x_j \Theta_{ji}$$

where $\rho: \mathbb{R}^{d_e} \to \mathbb{R}^{d_x \times d_h}$ is an MLP.

---

## GNN Architecture Recipe

### Good Practices

- **Pre/post-processing**: 2-layer MLPs
- **Depth**: 4-6 message-passing steps
- **Message**: $m^l_{ji} = \text{PReLU}(\text{BatchNorm}(h^l_j \Theta^l + b^l))$
- **Aggregation**: Sum
- **Update**: $h^{l+1}_i = h^l_i | m^l_i$ (concatenation)

---

## Graph Pooling

### Select-Reduce-Connect Framework

1. **Select**: $\text{SEL}: G \mapsto S = {S_1, \ldots, S_K}$
2. **Reduce**: $X' = SX$
3. **Connect**: $A' = SAS^T$

### Properties

- **Dense vs Sparse**: How many nodes selected
- **Fixed vs Adaptive**: Number of supernodes
- **Trainable vs Non-trainable**: Learn from data or not

### Spectral Clustering

Use low-frequency eigenvectors of Laplacian for clustering:

- Run k-means on first few eigenvectors
- **Problem**: $O(N^3)$ complexity, ignores node features

---

## Global Pooling

### Purpose

Convert graph to fixed-size vector for graph-level tasks

### Methods

- **Simple**: Sum, mean, max, product
- **Attention-based**: Weighted sum with learned weights
- **Neural**: Apply MLP after aggregation

**Requirement**: Must be permutation-invariant

---

## Key Challenges

### Over-smoothing

- Graph convolutions act as low-pass filters
- Features become too similar after many layers
- Limits effective depth of GNNs

### Applications

- **Node-level**: Social networks, citation networks
- **Graph-level**: Molecular property prediction, graph classification

---

## Tools & Libraries

### PyTorch Geometric (PyG)

- Popular library for GNN implementation
- Supports various GNN layers and utilities

### Torch Spatiotemporal (TSL)

- Specialized for spatiotemporal graph neural networks
- Used for traffic forecasting and similar tasks

---

## Mathematical Notation Summary

| Symbol           | Meaning                               |
| ---------------- | ------------------------------------- |
| $G = (V, E)$     | Graph with vertices $V$ and edges $E$ |
| $A$              | Adjacency matrix                      |
| $X$              | Node feature matrix                   |
| $E$              | Edge feature matrix                   |
| $D$              | Degree matrix                         |
| $L$              | Laplacian matrix                      |
| $\tilde{A}$      | Graph Shift Operator                  |
| $\Theta$         | Learnable parameters                  |
| $\mathcal{N}(i)$ | Neighborhood of node $i$              |
| $\sigma$         | Activation function                   |
| $\|$             | Concatenation                         |
