---
layout: default
title: Project 4 - Lithium Migration Barriers
---

# Data-Driven Prediction of Lithium Migration Barriers

## Motivation

Lithium-ion migration barriers determine ionic conductivity in battery materials.  
Accurate estimation requires expensive DFT-NEB calculations.

We developed a machine learning framework that:

1. Learns migration physics from large approximate datasets.
2. Transfers this knowledge to high-fidelity DFT.
3. Achieves strong generalization in small-data regimes.

---

## Datasets

### nebBVSE122k (122,421 samples)

Approximate migration barriers computed using BVSE-NEB.

### nebDFT2k (1,681 samples)

High-fidelity DFT-NEB migration barriers.

---

## Model Architecture

Each migration hop is represented as a graph:

- Nodes: Atoms in supercell
- Special centroid atom "X" marks bottleneck
- Edge features: Interatomic distances
- Node features: Atomic number + distance to centroid

We use an NNConv geometry-aware GNN:

$$
\mathcal{G} \rightarrow \text{Message Passing} \rightarrow \text{Pooling} \rightarrow E_a
$$

---

## Experimental Design

1. BVSE Pretraining (122k samples)
2. DFT Fine-Tuning (80%)
3. DFT Scratch Training (80%)
4. Evaluation on identical 20% held-out DFT test set

---

## Results

| Model | MAE (eV) | RMSE (eV) |
|-------|----------|-----------|
| DFT Scratch | 0.534 | 0.734 |
| BVSE → DFT Fine-Tuned | 0.260 | 0.366 |

### Transfer Learning Effect

$$
\text{MAE Reduction} \approx 51\%
$$

---

## Parity Plot

![Parity Comparison](assets/parity_comparison.png)

---

## Error vs Barrier Analysis

![Error vs Barrier](assets/error_vs_barrier.png)

---

## Scientific Implications

- Large-scale approximate physics is an effective pretraining corpus.
- Structural embeddings transfer to quantum-accurate DFT barriers.
- Transfer learning reduces DFT error by ~50%.

This provides a scalable framework for accelerating battery materials screening.
