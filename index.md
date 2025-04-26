# 🧠 Gromov-Wasserstein (GW) Distance Cheat Sheet

## 📖 Motivation
- **When**: Classical Optimal Transport (OT) assumes two distributions live in the **same space** (shared metric).
- **Problem**: When the distributions are on **different metric spaces**, OT is not directly applicable.
- **GW idea**: Match distributions by comparing **internal relational structures** (the geometry inside each space), not their absolute coordinates.

---

## 📐 Formal Definition
Given two metric measure spaces:
- \((X, d_X, \mu)\)
- \((Y, d_Y, \nu)\)

the **Gromov-Wasserstein distance** between \(\mu\) and \(\nu\) is:

\[
\text{GW}^2(\mu, \nu) = \min_{\pi \in \Pi(\mu, \nu)} \iint_{X \times Y} \iint_{X \times Y} |d_X(x,x') - d_Y(y,y')|^2 \, d\pi(x,y) \, d\pi(x',y')
\]

- \(\Pi(\mu, \nu)\) = set of all couplings between \(\mu\) and \(\nu\).
- \(d_X\), \(d_Y\) = distances in the respective spaces.

✅ **Matches pairwise distances**, not points directly.

---

## 🧮 Key Components
- **Coupling \(\pi\)**: Joint probability distribution matching marginals \(\mu\) and \(\nu\).
- **Loss function** \(L(a,b) = (a-b)^2\): measures mismatch between distances.
- **Relational Matching**: Not looking at points but **how points relate to each other**.

---

## 🔥 Computational Version (Discrete GW)

Given:
- \(C_X \in \mathbb{R}^{n \times n}\): distance matrix for source
- \(C_Y \in \mathbb{R}^{m \times m}\): distance matrix for target
- \(p \in \Delta_n\), \(q \in \Delta_m\): source and target histograms (probabilities)

The discrete GW problem:

\[
\min_{T \in U(p,q)} \sum_{i,j,k,l} |C_X(i,j) - C_Y(k,l)|^2 \, T_{ik} T_{jl}
\]

where:
- \(U(p,q)\) = set of matrices with row/column sums \(p\) and \(q\).

---

## ⚙️ Solvers
- **Gradient Descent** on \(T\).
- **Entropic Regularization**:
  - Add entropy penalty to smooth optimization.
  - Leads to **Sinkhorn-like algorithms**.

---

## 🧩 Entropic GW (Fast Variant)

Regularized problem:

\[
\text{GW}_\epsilon^2(\mu, \nu) = \min_{\pi \in \Pi(\mu,\nu)} \iint |d_X(x,x') - d_Y(y,y')|^2 d\pi(x,y)d\pi(x',y') - \epsilon H(\pi)
\]

with entropy:

\[
H(\pi) = -\int \log(\pi(x,y)) \, d\pi(x,y)
\]

✅ Faster and smoother convergence.

---

## ✏️ Algorithm Sketch
1. **Initialize** random coupling \(T\).
2. **Compute loss tensor**:
   \[
   L_{i,j,k,l} = (C_X(i,j) - C_Y(k,l))^2
   \]
3. **Update T** iteratively (Sinkhorn, gradient descent, etc.).
4. **Stop** when convergence (loss stabilizes).

---

## ⚡ Applications
- **Shape Matching** (matching surfaces in computer graphics)
- **Graph Matching** (comparing graphs as metric spaces)
- **Domain Adaptation** (aligning datasets across domains)
- **Manifold Alignment** (unsupervised learning)

---

## 🔑 Key Properties
| Property            | GW Distance |
|---------------------|--------------|
| Same Space Needed?   | ❌ No        |
| Focus                | Relations between points |
| Symmetric            | ✅            |
| Non-negative         | ✅            |
| Triangle Inequality  | ❓ (pseudo-distance; not a full metric in all cases) |

---

## 🧠 Intuition
- **OT** matches **points**.
- **GW** matches **shapes of spaces**.

Think of it like comparing **the "skeleton" or "internal geometry"** of two datasets!

---

# Bonus: 📚 Reference formulas to remember
- Classical OT:

  \[
  \text{OT}_c(\mu, \nu) = \min_{\pi \in \Pi(\mu,\nu)} \int c(x,y) d\pi(x,y)
  \]

- GW (structure matching):

  \[
  \text{GW}(\mu,\nu) = \min_{\pi \in \Pi(\mu,\nu)} \text{Mismatched internal distances}
  \]
