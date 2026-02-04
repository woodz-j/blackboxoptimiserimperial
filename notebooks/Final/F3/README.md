# Bayesian Optimization of 3-Component Formulation  
13-Week Study

Optimizing a three-component mixture using Gaussian Process–based Bayesian Optimization

## Overview

This project applied **Bayesian Optimization** (BO) to maximize the performance of a **three-component formulation** over the bounded domain [0,1]³.

- Objective values were consistently **negative** → goal was to **maximize** the response (make it less negative / closest to zero)
- Surrogate: Gaussian Process
- Acquisition: Expected Improvement (EI)
- 13-week iterative process: global exploration → structure learning → exploitation → local refinement → convergence confirmation

The optimization exhibited classic BO behavior, including early exploration, natural clustering into a promising basin, kernel stabilization challenges, and strong local convergence signals.

## Methodology

- **Domain**: 3 continuous variables ∈ [0, 1]³  
- **Surrogate**: GaussianProcessRegressor  
  - Kernel: Matérn (ν = 2.5)  
  - Length-scale bounds and noise regularization progressively tightened  
  - Output normalization (`normalize_y=True`)  
- **Acquisition function**: Expected Improvement (EI)  
- **EI optimizer**: Multi-start L-BFGS-B  
- **Kernel tuning**: Manual bounding + increased noise to prevent length-scale collapse  
- **Optimizer restarts**: 10 restarts for robustness

## Timeline of Strategy and Results

| Week   | Strategy Focus                  | Why / Approach                              | Representative Point                  | Best Result       | Key Insight                              |
|--------|---------------------------------|---------------------------------------------|----------------------------------------|-------------------|------------------------------------------|
| 1–2    | Initial exploration             | Seed diverse space                          | Random / initial design                | Mixed values      | Landscape highly non-linear              |
| 3      | EI global search                | Let GP guide exploration                    | High-uncertainty regions               | Poorer values     | Most space is sub-optimal                |
| 4      | EI exploration                  | Continue uncertainty sampling               | Edge regions                           | Very negative     | Steep penalties away from center         |
| 5      | EI begins clustering            | GP learns structure                         | Mid-range region                       | Improving         | Promising basin emerging                 |
| 6      | EI refinement                   | More samples near basin                     | Around [0.4, 0.3, 0.4]                 | Better            | Basin becoming clear                     |
| 7      | EI exploitation begins          | Model confident in region                   | [0.1179, 0.1284, 0.4789]               | Moderate          | Third dimension important                |
| 8      | Kernel issues observed          | Length scale collapse → erratic EI          | Inconsistent                           | —                 | Needed kernel tightening                 |
| 9      | Kernel bounded + noise          | Stabilize GP                                | Near [0.5, 0.4, 0.45]                  | **-0.00505**      | Core optimum region found                |
| 10     | Exploitation-leaning EI         | Test local curvature                        | Small perturbations                    | Worse             | Confirm local peak                       |
| 11     | Pure exploitation               | Maximize GP mean                            | Near incumbent                         | Worse             | GP confidence validated                  |
| 12     | Local symmetric perturbations   | Stress test optimum                         | ±0.02 changes                          | Worse             | Sharp curvature confirmed                |
| 13     | Final refinement                | Convergence check                           | Tiny local moves                       | Worse             | Converged                                |

## Best Point Found

```text
x* = [0.514909, 0.429977, 0.441068]
y* = -0.0050467853497373665
```

No perturbation in the final weeks improved upon this value.

## Evidence of Convergence

EI values near incumbent → ≈ 0
Very low GP predictive variance at/around best point
All local refinements → worse performance
Kernel length-scales stabilized (~0.22–0.25)
Pure exploitation and exploitation-leaning EI failed to improve

→ Strong empirical evidence of local convergence.

## Lessons Learned

EI explores aggressively early unless kernel is well-regularized
Length-scale collapse causes random / meaningless EI suggestions
Bounding length-scales + adding controlled noise dramatically stabilizes BO
Natural clustering of EI proposals is a reliable sign of approaching the optimum
Repeated failure of local perturbations is one of the strongest convergence signals

## Final Conclusion
The Bayesian Optimization process successfully located and validated a sharp, highly localized optimum in a 3D continuous domain.
Further sampling consistently degraded performance, and the GP surrogate expressed high confidence in the discovered region.
The study illustrates a complete BO lifecycle:
Exploration → Learning → Exploitation → Local refinement → Convergence

## Reproducibility – Key Implementation Details
``` code
from sklearn.gaussian_process import GaussianProcessRegressor
from sklearn.gaussian_process.kernels import Matern

kernel = Matern(
    length_scale=0.2,
    length_scale_bounds=(1e-2, 2.0),
    nu=2.5
)

gp = GaussianProcessRegressor(
    kernel=kernel,
    alpha=1e-4,               # noise level
    normalize_y=True,
    n_restarts_optimizer=10,
    random_state=0
)
```
EI optimized via multi-start L-BFGS-B over [0,1]³
Progressive tightening of kernel bounds and noise was critical for late-stage stability

## Project Outcome
This 13-week case study demonstrates practical Bayesian Optimization in action, including:

Surrogate model debugging
Kernel parameter control
Understanding acquisition function dynamics
Reliable convergence assessment

The final result is not merely a point — it is well-supported by the full optimization trajectory.
