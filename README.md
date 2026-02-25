
# AACO: Alternation-Based Ant Colony Optimization for Large-Scale TSP

![License](https://img.shields.io/badge/license-MIT-blue.svg) ![Status](https://img.shields.io/badge/status-research-orange)

This repository contains the implementation and experimental results of the paper **"Improving Solutions Quality for Large-Scale Datasets of the Traveling Salesman Problem"**.

The project introduces **AACO** (Alternation-Based Ant Colony Optimization), a novel metaheuristic approach designed to solve Large-Scale Traveling Salesman Problems (TSP) ranging from 30,000 to 200,000+ nodes.

## 📖 Abstract

The Traveling Salesman Problem (TSP) is a classic combinatorial optimization problem. While heuristic solvers like LKH dominate small-to-medium instances, Ant Colony Optimization (ACO) has historically struggled with large-scale datasets (>30,000 nodes) due to memory and convergence constraints.

This research analyzes existing improvements in ACO and proposes **AACO**, which integrates two major contributions:
1.  **Forced Path Alternation:** Controlling ant behavior by forcing path deviations to escape local optima.
2.  **Dynamic Ant Population with Smooth Max-Min:** Adjusting the number of active ants over time combined with a Smooth Max-Min Ant System (SMMAS) update rule.

## 🚀 Key Features

*   **Scalability:** Optimized for datasets up to 200,000 nodes (Art TSP instances).
*   **Smooth Max-Min Ant System (SMMAS):** Replaces standard MMAS with a smoothed pheromone update to better balance exploration and exploitation.
*   **Dynamic Population:** Implements an adaptive strategy where the ant population doubles at specific intervals to match the increasing complexity of the search phase.
*   **Forced Diversity:** Forces ants to choose non-optimal edges early in the construction phase to ensure search space diversity.
*   **Performance:** Outperforms state-of-the-art ACO variants (FocusedACO, PartialACO, RPMM) in solution quality.

## 📊 Experimental Results

Experiments were conducted on an Intel(R) Xeon(R) CPU E5-2697 v4 @ 2.30GHz with 64GB RAM. The algorithm was tested against **TSPLIB**, **TSP Test Data**, and **Art TSP**.

### 1. Large-Scale Art TSP (100k - 200k nodes)

AACO achieves the lowest error rate among all tested ACO algorithms on the Art TSP dataset.

| Instance | Nodes | Best Known Tour | AACO Result | Error (%) | Time (h) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **mona-lisa100K** | 100,000 | 5,757,191 | 5,769,900 | **0.22%** | 0.22 |
| **vangogh120K** | 120,000 | 6,543,609 | 6,558,668 | **0.23%** | 0.28 |
| **venus140K** | 140,000 | 6,810,665 | 6,826,911 | **0.23%** | 0.34 |
| **pareja160K** | 160,000 | 7,619,953 | 7,639,517 | **0.25%** | 0.44 |
| **courbet180K** | 180,000 | 7,888,731 | 7,911,253 | **0.28%** | 0.57 |
| **earring200K** | 200,000 | 8,171,677 | 8,197,462 | **0.31%** | 0.76 |

#### Comparison with State-of-the-Art ACOs
*Comparison of error rates (%):*

| Instance | ACO-RPMM | PartialACO | FocusedACO (2023) | **AACO (Ours)** |
| :--- | :---: | :---: | :---: | :---: |
| mona-lisa100K | 1.7% | 5.5% | 0.42% | **0.22%** |
| vangogh120K | 1.8% | 5.8% | 0.47% | **0.23%** |
| venus140K | 1.8% | 5.8% | 0.50% | **0.23%** |
| pareja160K | 1.9% | - | 0.56% | **0.25%** |
| courbet180K | 1.9% | - | 0.63% | **0.28%** |
| earring200K | 2.0% | 7.2% | 0.71% | **0.31%** |

### 2. New Best Known Solutions (30k - 100k nodes)

AACO found new best-known solutions for several instances, surpassing results from LKH and FocusedACO.

| Instance | Previous Best | AACO Best | Improvement |
| :--- | :--- | :--- | :--- |
| **Tnm30001** | 566,973,186 | **566,973,183** | -3 |
| **Tnm40000** | 756,254,121 | **756,254,119** | -2 |
| **Tnm50002** | 945,535,600 | **945,535,598** | -2 |
| **Tnm100000** | 1,891,945,653 | **1,891,945,649** | -4 |

> **Note:** The *Tnm* datasets are designed to be difficult for heuristic solvers like LKH. AACO's ability to improve these solutions demonstrates the robustness of the ant colony approach.

### Visualizing the Solution

![Mona Lisa Tour](path/to/mona-lisa100K.png)
*Visual representation of the solution constructed by AACO for the mona-lisa100K dataset.*

## ⚙️ Algorithm Overview

### Smooth Max-Min Ant System (SMMAS)
Unlike traditional MMAS which uses hard clamps, AACO updates pheromones using a smoothing function:
$$ \tau_{ij} := (1-p) \cdot \tau_{ij} + p \cdot \tau_{target} $$
Where $\tau_{target}$ is either $\tau_{max}$ or $\tau_{min}$ depending on whether the edge belongs to the best tour. This prevents the "search stagnation" often seen in large instances.

### Dynamic Ant Population
The algorithm starts with a small population to quickly identify good "skeleton" paths. Every `steps` iterations, the population size is doubled.
*   **Early Phase:** Low ant count $\to$ Fast convergence.
*   **Late Phase:** High ant count $\to$ High exploration to break local optima.

## 🔧 Usage

*(Include instructions on how to build and run your code here. Example below:)*

```bash
# Clone the repository
git clone https://github.com/yourusername/AACO-TSP.git

# Build
mkdir build && cd build
cmake ..
make

# Run on a specific instance
./aaco_solver --instance ../data/mona-lisa100K.tsp --ants 432 --steps 4000
```

## 📝 Citation

If you use this code or results in your research, please cite the paper:

```bibtex
@article{dat2024aaco,
  title={Improving Solutions Quality for Large-Scale Datasets of the Traveling Salesman Problem},
  author={Trần Thành Đạt},
  organization={VNU University of Engineering and Technology},
  year={2024}
}
```

## 🤝 Acknowledgements

Special thanks to:
*   **Dr. Do Duc Dong** for research guidance.
*   **Rafał Skinderowicz** (author of FocusedACO) for assistance in experimental comparisons.

---
*Author: Trần Thành Đạt - VNU University of Engineering and Technology*
*Contact: shora.dt@gmail.com*
