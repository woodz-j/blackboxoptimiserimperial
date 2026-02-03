# Bayesian Optimisation of an 8‑Dimensional Black‑Box Function

## Project Overview

This project investigates the optimisation of an unknown,
eight‑dimensional black‑box function over a 13‑week period. Each
evaluation maps an 8‑parameter input vector (normalised to \[0,1\]) to a
single scalar output representing performance. The internal structure of
the function is unknown, making gradient‑based or analytical
optimisation infeasible.

The objective was to **maximise the output value** efficiently under a
limited evaluation budget, while building confidence that the discovered
solution corresponds to the dominant peak of the landscape.

A **Bayesian Optimisation** framework using a **Gaussian Process (GP)**
surrogate model was employed throughout.

------------------------------------------------------------------------

## Problem Characteristics

-   8 continuous input dimensions
-   Black‑box, expensive to evaluate
-   Likely non‑convex and multi‑modal
-   Outputs observed to be smooth but with diminishing returns near the
    optimum

------------------------------------------------------------------------

## Methodology

### Surrogate Model

A **Gaussian Process Regressor** with a Matérn (ν = 2.5) kernel was used
to model the unknown function: - Automatically learns length scales and
variance via marginal likelihood optimisation - Normalised outputs for
numerical stability - Small noise term (alpha = 1e‑6) to prevent
overfitting

The GP's internal optimiser (**L‑BFGS‑B**) refit kernel hyperparameters
at each iteration as new data was added.

------------------------------------------------------------------------

### Acquisition Strategy

The optimisation strategy evolved over time and was deliberately
adaptive:

#### Early Phase (Weeks 1--4): Exploration

-   Expected Improvement (EI) with moderate exploration parameter (ξ)
-   Global bounds across all dimensions
-   Goal: map broad structure and identify promising regions

#### Middle Phase (Weeks 5--9): Balanced Explore--Exploit

-   Alternating exploration and exploitation
-   EI with higher ξ for exploration
-   Local trust‑region EI for exploitation
-   Identification of a dominant high‑performing basin

#### Late Phase (Weeks 10--13): Exploitation & Refinement

-   Clear diminishing returns observed
-   Explicit separation of strategies:
    -   **Exploit:** maximise GP predictive mean (μ)
    -   **Explore:** EI only when uncertainty justified it
-   Progressive shrinking of trust region radius
-   Final tight exploit radius ≈ 0.03

This separation avoided boundary chasing and stabilised convergence.

------------------------------------------------------------------------

## Key Findings

### Landscape Structure

-   The function exhibits **one dominant, broad peak** rather than many
    sharp optima
-   Secondary regions exist but are significantly inferior
-   Output surface is smooth and locally well‑behaved

### Parameter Patterns

High‑performing points consistently shared similar structure: - Early
dimensions: low--moderate values - Middle dimensions: mid‑range - One
dimension consistently high but not extreme - Best results avoided hard
boundaries (0 or 1)

This stability supported the use of local refinement strategies.

------------------------------------------------------------------------

## Convergence Evidence

Convergence was assessed using multiple signals: - Successive exploit
steps yielded progressively smaller improvements - Multiple nearby
points achieved nearly identical high outputs - Exploratory probes
failed to uncover competing peaks - GP predictive variance around the
best point became very low

The final best observed value was:

**Best output:** 9.9835

Achieved through minimal perturbations of the best‑known region,
indicating convergence to the dominant optimum.

------------------------------------------------------------------------

## Final Strategy Justification

The final optimisation approach reflects best practice for
high‑dimensional Bayesian optimisation: - Avoid premature exploitation -
Explicitly separate exploration and exploitation objectives - Use EI for
discovery, GP mean for refinement - Stop when improvements fall below
meaningful thresholds

Further evaluations are unlikely to yield statistically significant
gains.

------------------------------------------------------------------------

## Optimisation Timeline

  ------------------------------------------------------------------------
  Weeks     Strategy Focus           Key Actions          Outcomes
  --------- ------------------------ -------------------- ----------------
  1--2      Initial exploration      Random sampling,     Established
                                     broad EI             baseline
                                     optimisation         performance and
                                                          rough landscape
                                                          shape

  3--4      Structured exploration   EI with global       Identified
                                     bounds, GP fitting   several
                                     stabilised           promising
                                                          regions

  5--6      Early exploitation       EI with reduced      First strong
                                     exploration          basin discovered
                                     parameter, local     (outputs
                                     search               \~9.6--9.7)

  7--8      Balanced                 Alternating global   Confirmed
            explore--exploit         explore and local    dominant basin,
                                     refine               ruled out weak
                                                          competitors

  9--10     Focused exploitation     Local trust-region   Incremental
                                     EI                   improvements,
                                                          diminishing
                                                          returns observed

  11        Strategy refinement      Separation of        Eliminated
                                     exploit (GP mean)    boundary
                                     and explore (EI)     chasing,
                                                          stabilised
                                                          optimisation

  12        Tight exploitation       Mean maximisation    Achieved
                                     with small trust     near-optimal
                                     region               outputs (\~9.97)

  13        Final confirmation       Very tight exploit,  Best result
                                     convergence check    9.9835, strong
                                                          convergence
                                                          evidence
  ------------------------------------------------------------------------

------------------------------------------------------------------------

## Conclusion

Over 13 weeks, the optimisation progressed from global uncertainty
reduction to disciplined local refinement. The final result demonstrates
effective use of Gaussian Process--based Bayesian optimisation for
complex black‑box functions, achieving near‑optimal performance with a
limited evaluation budget and strong confidence in convergence.

------------------------------------------------------------------------

## Tools & Libraries

-   Python
-   NumPy
-   SciPy
-   scikit‑learn (GaussianProcessRegressor)

------------------------------------------------------------------------

## Notes

This project emphasises **process and reasoning** as much as final
performance, illustrating how adaptive query strategies and model‑driven
decisions outperform static heuristics in complex optimisation problems.
