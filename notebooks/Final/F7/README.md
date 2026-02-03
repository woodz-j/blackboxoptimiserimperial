# Bayesian Optimization of a 6D Black-Box Model

**Function 7** — Finding the optimum of a six-dimensional expensive black-box function using Bayesian Optimization

## Overview

This project demonstrates the application of **Bayesian Optimization** (BO) to efficiently optimize a six-dimensional black-box objective function — most likely representing the performance (e.g. accuracy, F1-score, or similar metric) of a machine learning model under different hyperparameter settings.

Because evaluations are expensive and the true function is unknown, non-convex, and noisy, we used a **Gaussian Process** surrogate model combined with the **Expected Improvement (EI)** acquisition function.

The optimization ran iteratively over **13 weeks**, gradually transitioning from global exploration → guided exploitation → local polishing.

## Problem Setting

- **Objective**: Maximize a scalar performance metric  
- **Input space**: 6 continuous hyperparameters, each ∈ [0, 1]  
- **Evaluation characteristics**: black-box, non-convex, noisy  
- **Initial design**: 30 randomly sampled points

## Methodology

- **Surrogate model**  
  - Gaussian Process Regression  
  - ARD Matérn 5/2 (ν = 2.5) kernel  
  - WhiteKernel (noise model)  
  - Output normalization

- **Acquisition function**  
  - Expected Improvement (EI)

- **Optimization strategy evolution**  
  - Weeks 1–4: global exploration  
  - Weeks 5–7: mixed exploration / exploitation  
  - Weeks 8–10: local exploitation  
  - Weeks 11–13: micro-perturbation & final polishing

- **Key structural insight**  
  Dimension 6 consistently converged very close to 0 → later clamped / fixed near zero

## 13-Week Optimization Timeline

| Week | Strategy                        | Key Actions                              | Main Insight / Result                          |
|------|---------------------------------|------------------------------------------|------------------------------------------------|
| 1    | Initialization                  | Evaluate provided initial design         | Baseline established, high variance            |
| 2    | Global exploration              | GP + EI over full domain                 | Noisy landscape, no clear mode yet             |
| 3    | Global EI (multi-start)         | Large random candidate pools             | First moderate improvements                    |
| 4    | Exploration-heavy EI            | Increased exploration parameter ξ        | Promising region identified                    |
| 5    | Mixed exploration/exploitation  | EI + uncertainty emphasis                | Single dominant basin emerges                  |
| 6    | Local exploitation              | EI near best candidates                  | Rapid improvement trend                        |
| 7    | Structural learning             | Observed x₆ → low values                 | Hypothesis: x₆ ≈ 0 is optimal                  |
| 8    | Constraint-aware search         | Clamp x₆ near zero                       | Significant performance jump                   |
| 9    | GP refinement                   | ARD kernel + optimizer restarts          | Stable lengthscales, smoother EI landscape     |
| 10   | Local EI                        | Small-radius perturbations               | Peak region clearly defined                    |
| 11   | Micro-perturbation              | Very small local steps                   | Best value ≈ 2.82                              |
| 12   | Final polish                    | Tighter perturbation radius              | New best ≈ 2.86                                |
| 13   | Convergence check               | EI collapse + curvature mapping          | Confirmed convergence                          |

## Dominant High-Performance Region

All top-performing configurations converged to a narrow cluster:

```text
x₁ ≈ 0.36 – 0.39
x₂ ≈ 0.80 – 0.83
x₃ ≈ 0.44 – 0.47
x₄ ≈ 0.56 – 0.61
x₅ ≈ 0.72 – 0.75
x₆ ≈ 0.000 – 0.001