# Function 4 Optimisation – 13-Week Bayesian Optimisation Study

**Black-box optimisation of warehouse placement accuracy**

## Overview

Function 4 is an expensive black-box objective representing **warehouse placement accuracy**.  
Direct evaluations are computationally costly, so **Bayesian Optimisation** (BO) was employed to intelligently select evaluation points.

We used:

- **Gaussian Process** (GP) surrogate  
- **Expected Improvement** (EI) acquisition function  

The 13-week study progressed from broad exploration → guided exploitation → boundary confirmation → validation of the discovered optimum.

## Problem Definition

- **Inputs**: 4 continuous hyperparameters ∈ [0, 1]  
- **Output**: Difference from expensive baseline  
  → **negative values = better** (lower is better)  
- **Evaluation budget**: Very limited (weekly runs)  
- **Function characteristics**: Highly non-linear, strong dimension interactions, sharp ridge-like structure

## Methodology

- **Surrogate**: Gaussian Process with Matérn kernel  
- **Acquisition**: Expected Improvement (EI)  
- **Special handling**: Manual penalties / forced exploration introduced in later stages to escape corner bias

## 13-Week Timeline & Key Observations

| Week | Strategy                          | Key Observation                                      | Main Result / Value          |
|------|-----------------------------------|------------------------------------------------------|------------------------------|
| 1    | Random initial sampling           | Wide variation, no clear pattern                     | Outputs −4 to −32            |
| 2    | First GP + EI fit                 | Model begins guiding search                          | Near-edge points suggested   |
| 3    | First extreme-edge evaluation     | Major improvement                                    | −43.88                       |
| 4    | Exploit around edge               | Confirms strong region                               | −53.94                       |
| 5    | Further corner refinement         | Suggests optimum at boundary                         | ≈ −54                        |
| 6    | Boundary probing                  | GP repeatedly proposes [1,1,1,1]                     | Model shows high certainty   |
| 7    | Controlled near-corner tests      | Performance drops sharply when any dimension reduced | Confirms strong interactions |
| 8    | Introduced corner penalties       | Forced exploration away from boundary                | Interior ≈ −12 to −20        |
| 9    | Structured interior sampling      | No competing optima found                            | Corner dominance confirmed   |
| 10   | Sensitivity-style points          | One-dimension reductions give ~−41                   | Steep ridge, not plateau     |
| 11   | Final corner confirmation         | True best value observed                             | **−55.83** at [1,1,1,1]      |
| 12   | Exploration with penalties        | Verified no hidden interior peak                     | Weak results                 |
| 13   | Validation & surface analysis     | Focus on understanding shape & uncertainty           | Optimisation concluded       |

## Final Best Result

**Best observed configuration:**

Input:  [0.999999, 0.999999, 0.999999, 0.999999]
Output: -55.827686352381896

## Key Findings

The global optimum lies at the extreme corner [1,1,1,1]
The function shows a sharp, interaction-driven ridge rather than a smooth plateau
Reducing any single dimension causes large performance degradation
Once sufficient evidence accumulated, the GP/EI became strongly biased toward the corner
Later evaluations were more valuable for validation & understanding than for further optimisation

## Limitations Observed

Heavy sampling near the corner → dataset bias
GP smoothness assumption → possible narrow spikes elsewhere might be missed
Very limited evaluation budget → incomplete mapping of the full space
Acquisition function needed manual intervention (penalties) to escape local certainty

## Conclusion
Bayesian Optimisation efficiently located the dominant optimum in a highly non-linear, interaction-heavy 4D black-box function.
The process clearly illustrated how surrogate-based methods transition over time:

early: exploration tools
middle: exploitation drivers
late: validation & diagnostic instruments