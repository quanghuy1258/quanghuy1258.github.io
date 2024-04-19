---
title: "Notes of Graph Neural Networks (Part 01)"
categories:
  - Artificial intelligence
tags:
  - Graph theory
  - Linear algebra
toc: true
toc_label: "Table of Contents"
mathjax: true
issue_id: 6
---

## 1. Introduction

Before getting started, I would like to wander a bit about the title.
This post is part of a series of notes on Graph Neural Networks.
Honestly, I don't have much interest in topics related to Artificial Intelligence (AI) or Data Science (DS), simply because I don't like to deal with data issues, such as collecting, cleaning, and preprocessing.
However, in the near future, I will have a half-year internship at National Institute of Informatics (NII), Japan, and my work will be associated with this topic.
For this very reason, I need to prepare from now, and take note of it as a series of posts like this.
So, here is the first post.

## 2. Preliminaries

Today's problem involves the knowledge of graph theory and linear algebra.
Therefore, I present it before stating the problem.

### 2.1. Graph theory

For the sake of simplicity, I only talk about graphs where multiple edges are not permitted, but loops are allowed.

**Definition 2.1.1. \[[Ref.](https://en.wikipedia.org/wiki/Graph_theory#Graph)\]**
Graph $G$ is an ordered pair $(V, E)$ consisting of:
- $V$, a set of vertices (also called nodes or points).
- $E \subseteq V^2 = \left\\{ (x, y) \| x, y \in V \right\\}$, a set of edges (also called links or lines), which are ordered pairs of vertices.

Considering $E$ as a relation $R$ (see [more](https://en.wikipedia.org/wiki/Relation_(mathematics))) over $V$, I have the following properties:
- $R$ is irreflexive iff there are no loops in $G$. Then, $G$ is called a simple graph.
- If $R$ is symmetric, then $G$ is called an undirected graph.
- If $R$ is not symmetric, then $G$ is called a directed graph.

However, in this post, I won't use the above notations to represent graphs.
Instead, matrices are used, namely adjacency, degree, and Laplacian ones.

**Definition 2.1.2. \[[Ref.](https://en.wikipedia.org/wiki/Adjacency_matrix)\]**
Given a simple graph $G = (V, E)$ with the vertex set $V = \\{v_1, \ldots, v_n\\}$, the adjacency matrix $A$ for $G$ is a square $n \times n$ matrix such that:

$$A_{i,j} = \begin{cases}
  1, & \text{if } (v_i, v_j) \in E \\
  0, & \text{otherwise } \\
\end{cases}$$

**Definition 2.1.3. \[[Ref.](https://en.wikipedia.org/wiki/Degree_matrix)\]**
Given an undirected simple graph $G = (V, E)$ with the vertex set $V = \\{v_1, \ldots, v_n\\}$, the degree matrix $D$ for $G$ is a square $n \times n$ matrix such that:

$$D_{i,j} = \begin{cases}
  deg(v_i), & \text{if } i = j \\
  0, & \text{otherwise } \\
\end{cases}$$

where the degree $deg(v_i)$ of the vertex $v_i$ is defined by $\left\|\left\\{ v_j \in V \| (v_i, v_j) \in E \right\\}\right\|$.

**Definition 2.1.4. \[[Ref.](https://en.wikipedia.org/wiki/Laplacian_matrix)\]**
Given an undirected simple graph $G = (V, E)$ with the vertex set $V = \\{v_1, \ldots, v_n\\}$, the Laplacian matrix $L$ for $G$ is a square $n \times n$ matrix such that:

$$L_{i,j} = \begin{cases}
  deg(v_i), & \text{if } i = j \\
  -1, & \text{if } i \neq j \wedge (v_i, v_j) \in E \\
  0, & \text{otherwise } \\
\end{cases}$$

From the above definitions, I have the following properties:
- $A$ is symmetric if $G$ is undirected.
- $D$ is a diagonal matrix.
- $L = D - A$.

Before ending this section, I want to introduce a special matrix, normalized Laplacian matrix.
It is defined by $L^\text{norm} = D^{-\frac{1}{2}} L D^{-\frac{1}{2}} = I - D^{-\frac{1}{2}} A D^{-\frac{1}{2}}$, i.e.

$$L^\text{norm}_{i,j} = \begin{cases}
  1, & \text{if } i = j \\
  -\frac{1}{\sqrt{deg(v_i)deg(v_j)}}, & \text{if } i \neq j \wedge (v_i, v_j) \in E \\
  0, & \text{otherwise }\\
\end{cases}$$

Of course, the degrees of all vertices need to be greater than $0$ so that $L^\text{norm}$ exists.
We see it again in the main problem mentioned in later sections.

### 2.2. Linear algebra

One of the topics which often appear in courses and books on Linear Algebra is eigenvalues and eigenvectors.
It plays the central role of many important concepts, algorithms, and methods, such as diagonalization, principal component analysis (PCA), singular value decomposition (SVD), optimization of quadratic functions.
Similarly, it also appears in the main problem I discuss later.

**Definition 2.2.1. \[[Ref.](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)\]**
Let $A$ be a square $n \times n$ matrix over $\mathbb{R}$.
Then, a nonzero vector $\mathbf{v} \in \mathbb{R}^n$ is called an eigenvector if there is one scalar $\lambda \in \mathbb{R}$ such that $A\mathbf{v} = \lambda\mathbf{v}$, where $\lambda$ is called the eigenvalue, characteristic value, or characteristic root associated with $\mathbf{v}$.

Besides, another concept which is also related, and needs mentioning is diagonalization.
It is defined as follows:

**Definition 2.2.2. \[[Ref.](https://en.wikipedia.org/wiki/Diagonalizable_matrix)\]**
Let $A$ be a square $n \times n$ matrix over $\mathbb{R}$.
If there exists an $n \times n$ invertible matrix $P$ such that $D = P^{-1}AP$ is a diagonal matrix, then $A$ is called diagonalizable or non-defective.
The process of finding the above $P$ and $D$ is called diagonalization.

From the above definitions, I easily infer the following results:
- The diagonal entries of $D$ are the eigenvalues of $A$.
- The column vectors of $P$ are the eigenvectors of $A$. Moreover, they are linearly independent because $P$ is invertible.

By extending the above results, I have a theorem on necessary and sufficient conditions for the diagonalizability of $A$.

**Theorem 2.2.3. \[[Ref.](https://en.wikipedia.org/wiki/Diagonalizable_matrix)\]**
A square $n \times n$ real matrix $A$ is diagonalizable if and only if $A$ have $n$ different linearly independent eigenvectors.

For symmetric matrices, I even have a more beautiful results than the above one.

**Theorem 2.2.4. \[[Ref.](https://en.wikipedia.org/wiki/Diagonalizable_matrix)\]**
If $A$ is a $n \times n$ real symmetric matrix, then there exists an orthogonal matrix $P$ such that $P^\top A P$ is a diagonal matrix, thus $A$ is diagonalizable.

It is noted that matrix representations of undirected graphs are symmetric matrices.
That is the reason why I introduce this theorem, and also the last part of related knowledge.
In the next section, I state the main problem of this post.

## 3. Problem statement

Given an undirected graphs $G$ with no vertices of degree $0$, prove that:
1. The minimum eigenvalue of the normalized Laplacian matrix of $G$ is $0$.
2. The maximum eigenvalue of the normalized Laplacian matrix of $G$ is not larger than $2$.

## 4. Proof of problem

For the sake of simplicity, I keep all denotations unchanged.
In addition, I only consider the eigenvectors of length 1 because if $\mathbf{v}$ is an eigenvector then $\frac{\mathbf{v}}{\|\mathbf{v}\|}$ is also an eigenvector associated with the same eigenvalue.

Assuming that $\mathbf{v}$ is an eigenvector and $\lambda$ is the corresponding eigenvalue, we have:

$$\begin{aligned}
     & L^\text{norm} \mathbf{v} = \lambda \mathbf{v} \\
\iff & \left(I - D^{-\frac{1}{2}} A D^{-\frac{1}{2}}\right) \mathbf{v} = \lambda \mathbf{v} \\
\iff & D^{-\frac{1}{2}} A D^{-\frac{1}{2}} \mathbf{v} = (1 - \lambda) \mathbf{v}
\end{aligned}$$

From that, we infer that:

$$\begin{aligned}
         |1 - \lambda| &= \left|\mathbf{v}^\top (1 - \lambda) \mathbf{v}\right| \\
                       &= \left|\mathbf{v}^\top D^{-\frac{1}{2}} A D^{-\frac{1}{2}} \mathbf{v}\right| \\
                       &= \left|\left(D^{-\frac{1}{2}} \mathbf{v}\right)^\top A \left(D^{-\frac{1}{2}} \mathbf{v}\right)\right| \\
                       &= \left|\sum_{(v_i, v_j) \in E} \frac{\mathbf{v}_i \mathbf{v}_j}{\sqrt{deg(v_i) deg(v_j)}}\right| \\
\implies |1 - \lambda| &\leq \sum_{(v_i, v_j) \in E} \frac{\left|\mathbf{v}_i \mathbf{v}_j\right|}{\sqrt{deg(v_i) deg(v_j)}} \\
\implies |1 - \lambda| &\leq \frac{1}{2} \sum_{(v_i, v_j) \in E} \left(\frac{\mathbf{v}_i^2}{deg(v_i)} + \frac{\mathbf{v}_j^2}{deg(v_j)}\right) \\
\end{aligned}$$

However, we also have:

$$\sum_{(v_i, v_j) \in E} \left(\frac{\mathbf{v}_i^2}{deg(v_i)} + \frac{\mathbf{v}_j^2}{deg(v_j)}\right) = 2 \sum_{i=1}^n \frac{\mathbf{v}_i^2}{deg(v_i)} deg(v_i) = 2 \sum_{i=1}^n \mathbf{v}_i^2 = 2$$

Therefore, we infer that:

$$|1 - \lambda| \leq 1 \iff 0 \leq \lambda \leq 2$$

The last thing we need to prove is that $0$ is an eigenvalue of $L^\text{norm}$.
That can be done by using the vector $\mathbf{v}^* = \left(\sqrt{deg(v_1)}, \ldots, \sqrt{deg(v_n)}\right)$.
Because of its simplicity, I leave it as an exercise for readers.

## 5. Conclusion

After solving the problem, here is the only remark I come up with.
All eigenvalues of normalized Laplacian matrices are bounded.
Moreover, the matrices are symmetric, so they are also diagonalizable.
Therefore, with an appropriate positive real multiplier, the power series of the matrices are convergent.
Xhonneux et al \[[Ref.](https://dl.acm.org/doi/suppl/10.5555/3524938.3525904)\] utilized this convergence to propose continuous graph neural networks.
Actually, I just read part of their work, so I will certainly come back with this topic, of course, after I completely understand it.
LOL :v
