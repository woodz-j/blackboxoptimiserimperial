# Function 5 – Black‑Box Optimisation README

## Overview
This project focused on optimising a four‑variable black‑box function representing the yield of a chemical process. The function was assumed to be **unimodal** with a single global maximum. Only input–output evaluations were available, requiring the use of **surrogate modelling** and **Bayesian optimisation techniques** to guide sampling efficiently.

A Gaussian Process (GP) with a Matérn kernel was used throughout as a probabilistic surrogate model. An Upper Confidence Bound (UCB) acquisition function guided where to sample next. Over 13 weeks, the strategy evolved from broad modelling of the search space to highly targeted boundary refinement, ultimately revealing that the global maximum lies at the upper corner of the domain.

---

## Key Methods Used

- Gaussian Process Regression (Matérn ν=2.5)
- Upper Confidence Bound (UCB) acquisition
- L‑BFGS‑B optimisation with bounds
- Adaptive bounds tightening
- Anisotropic local sampling
- 1‑D conditional sweeps of sensitive variables
- Iterative model refitting with new data

---

## Observed Behaviour of the Function

From the evaluations, the following structure emerged:

- \(x_1, x_2, x_3\) act as **support variables** that saturate near 1.0
- \(x_4\) remained the **primary driver** of continued yield improvement
- Yield increased smoothly and monotonically as all variables approached their upper bounds
- No evidence of secondary peaks or interior optima was observed

This revealed a **ridge‑like structure** leading to the corner
\((1, 1, 1, 1)\).

---

## Timeline of Strategy Evolution

| Week | Strategy | What Was Learned | Result / Impact |
|-----:|----------|------------------|------------------|
| 1 | Fit initial GP to sparse data | Model severely underestimated boundary behaviour | Low predicted yields everywhere |
| 2 | Basic acquisition optimisation | First move toward upper region | Moderate improvement |
| 3 | Multiple random restarts in optimiser | Avoided local artefacts in surrogate | More reliable suggestions |
| 4 | First boundary‑near evaluation | Yield jumped dramatically (>2000) | Realised initial data missed key region |
| 5 | Tightened bounds toward upper region | Consistent improvements | Confirmed directionality |
| 6 | Reduced exploration (lower kappa) | Focused on exploitation | Faster yield gains |
| 7 | Identified saturation of \(x_1,x_2,x_3\) | Treated them as near‑fixed | Simplified search |
| 8 | Introduced 1‑D sweeps in \(x_4\) | Revealed strong sensitivity in \(x_4\) | Large incremental gains |
| 9 | Anisotropic local sampling | Allocated more variance to \(x_4\) | More efficient refinement |
|10 | Bracketing ranges for \(x_4\) (0.78–0.88) | Monotonic rise observed | GP predictions validated |
|11 | Pushed bracket to 0.95+ | No interior peak detected | Confirmed boundary ridge |
|12 | Sampling near 0.97–0.99 | Yield exceeded 8000 | Strong evidence for boundary optimum |
|13 | Evaluate at \((0.999,0.999,0.999,0.999)\) | Highest yield observed (~8585) | Global optimum effectively located |

---

## Strategic Decisions That Mattered

- Moving from isotropic perturbations to **dimension‑aware sampling**
- Decoupling \(x_4\) from the other variables and treating it as a conditional optimisation
- Gradually tightening bounds rather than keeping the entire domain active
- Lowering exploration pressure once the structure of the function was understood

---

## Assumptions and Limitations

**Key assumption:** the function is unimodal.

This assumption justified focusing heavily on the high‑yield corner. If the function had contained secondary peaks, this strategy could have missed them. The approach is therefore best suited to smooth, single‑peak response surfaces.

---

## Final Conclusion

The optimisation process revealed that the chemical yield increases smoothly as all inputs approach their upper bounds. The best observed result occurred at:

\[
[0.999, 0.999, 0.999, 0.999]
\]

This strongly indicates the global optimum lies at or extremely close to the domain boundary. The combination of GP modelling, adaptive acquisition, and informed sampling decisions allowed the peak to be located efficiently despite the black‑box nature of the function.

---

## Reproducibility

A researcher could reproduce this strategy by:

1. Fitting a GP with a Matérn kernel
2. Using UCB acquisition with decreasing exploration
3. Iteratively refitting with new samples
4. Introducing dimension‑aware sampling once patterns emerge
5. Tightening bounds as evidence accumulates

All decisions were data‑driven and reflected in the sampling logic across weeks.

