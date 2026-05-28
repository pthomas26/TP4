# TP4: Unsupervised Learning & Associative Memory

**Course:** Sistemas de Inteligencia Artificial (ITBA)  
**Topic:** Self-Organizing Maps (Kohonen), Neural PCA (Oja's Rule), and Auto-associative Memory (Hopfield).  

This repository contains our implementation and analysis for TP4. Following a product-delivery methodology, we focused heavily on **exhaustive benchmarking, architectural trade-offs, and data-driven storytelling** directly within our interactive notebooks.

---

## Executive Summary

This project explores three different unsupervised learning paradigms. Since the models operate without labels, our evaluation relies heavily on structural trade-off analysis, dimensionality reduction metrics, and energy landscape visualizations.

1. **Kohonen Network (SOM):** Clustered 28 European countries based on 7 socio-economic indicators.
2. **Oja's Rule:** Implemented a single-neuron Hebbian learning model to extract the First Principal Component (PC1) of the Europe dataset online.
3. **Hopfield Network:** Built a 25-neuron recurrent associative memory to store and recover 5x5 pixel-art letters from noisy states, including a deep dive into spurious states.

---

## Key Insights & Evidence-Based Decisions

### 1. Kohonen SOM: The $N \times N$ Grid Trade-off
Instead of arbitrarily guessing a grid size, we evaluated grids from $2 \times 2$ to $5 \times 5$ over 20 random seeds.
* **The Problem:** A $2 \times 2$ grid is too dense (~7.2 countries per neuron), while $4 \times 4$ and $5 \times 5$ grids result in heavy "dead neuron" counts (up to 9.4 dead neurons per run).
* **Our Choice:** The **$3 \times 3$ grid** proved to be the mathematical sweet spot, minimizing dead neurons (~1.4 per run) while maintaining meaningful, interpretable clusters (~3.8 countries per neuron).
* **Result:** The U-Matrix successfully mapped a clear "prosperity gradient," cleanly separating Eastern European economies from Western/Northern ones without any geographic priors.

### 2. Oja's Rule: Perfecting Neural PCA
We implemented Oja's rule from scratch and benchmarked it against `scikit-learn`'s exact SVD-based PCA.
* **Validation:** Our single-neuron implementation achieved a **$0.999989$ Cosine Similarity** with the exact analytic PC1.
* **Storytelling:** By extracting the weight loadings, we interpreted this new dimension as a **"Socioeconomic Prosperity Axis"**—driven heavily upward by GDP and Life Expectancy, and pulled downward by Inflation and Unemployment. Physical size (Area) had a near-zero weight, proving the model correctly ignored irrelevant data.

### 3. Hopfield Network: Exhaustive Orthogonality Search
A 25-neuron network has a theoretical capacity limit of $p \le 0.15 \times 25 \approx 3.75$ patterns. Forcing it to learn 4 letters pushes the network to the brink of failure.
* **Methodology:** We did not pick 4 random letters. We programmatically generated all **$2,380$ combinations** $C(17, 4)$ from our alphabet and ranked them by their mathematical crosstalk (orthogonality).
* **Findings:** We proved that visual distinctness $\neq$ mathematical orthogonality. The "Best" orthogonal sets degraded gracefully even at 40% noise, while correlated sets (like our initial choice `A, E, I, L`) suffered from unbalanced energy basins, causing the network to collapse into spurious states frequently. 
* **Spurious States:** We successfully engineered and isolated both inverse attractors ($-S$) and mixed-state attractors (e.g., $sign(A+E)$).

---

## Repository Structure & Notebook Contents

Our work is divided into three self-contained Jupyter Notebooks, each acting as a complete analytical report.

* `europe.csv` - The socioeconomic dataset used for Oja's Rule and the Kohonen Network.
* `oja_model.ipynb` - Contains the implementation of Oja's Rule, feature loading extraction, sorting of the socio-economic prosperity axis, and the direct benchmarking against `scikit-learn`.
* `TP4_Ex1_Kohonen.ipynb` - Contains the SOM implementation, the quantitative $3 \times 3$ vs. other grid sizes trade-off analysis, and all visual mappings (Country Assignments, U-Matrix, Activation Counts).
* `TP4_Ex2_Hopfield.ipynb` - Contains the Hopfield network, the $2,380$-combination exhaustive orthogonality search, noise recall testing (at 20%, 30%, and 40%), and the isolation of spurious states via energy landscape analysis.