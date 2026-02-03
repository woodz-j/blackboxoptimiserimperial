# Function 6 – Bayesian Optimisation of a Black‑Box Cake Recipe

## Overview
This project aimed to optimise a black‑box cake recipe with five ingredient inputs (flour, sugar, eggs, butter, milk). Each recipe was evaluated by an expert taster and scored negatively based on flavour, consistency, calories, waste, and cost. The objective was to **maximise performance by driving the score as close to zero as possible**.

Because the function was unknown, non‑linear, and noisy, **Bayesian Optimisation with a Gaussian Process (GP)** surrogate model and acquisition functions (Expected Improvement and UCB) was used to sequentially choose new recipes to evaluate over 13 weeks.

---

## Key Characteristics of the Problem

- 5‑dimensional continuous input space \([0,1]^5\)
- Very small dataset (≈20 initial points)
- No analytical form of the objective function (black‑box)
- Expert tasting introduced **heteroscedastic noise**
- Highly **non‑linear response surface**
- Evidence of a **narrow optimum** and large regions of poor performance

---

## Methodology

- Outputs were negated to convert the problem into a maximisation task
- A **Gaussian Process with Matern(ν=2.5)** kernel was fitted at each step
- Two acquisition strategies were used at different stages:
  - **Expected Improvement (EI)** for exploitation
  - **Upper Confidence Bound (UCB)** for exploration
- Strategy evolved over time from global exploration to tight local exploitation

---

## Strategic Learning Over Time

Early rounds prioritised learning the shape of the surface. Midway through the project, a promising region was identified. Later rounds focused on exploiting this basin while performing occasional controlled exploration to confirm no better regions existed.

---

## Timeline of Strategy and Results

| Week | Strategy Used | Purpose | Result / Insight |
|------|----------------|---------|------------------|
| 1 | Initial random data | Baseline sampling | Large performance spread, many poor regions |
| 2 | GP + EI (moderate \`xi\`) | Balanced exploration | Began mapping response surface |
| 3 | Increased EI restarts | Avoid local optima | Confirmed high non‑linearity |
| 4 | First promising region found | Early exploitation | Identified cluster of better scores |
| 5 | Continued EI | Probe around cluster | Consistent improvement near same region |
| 6 | Introduced UCB exploration | Test other basins | Boundary/corner recipes performed very poorly |
| 7 | Soft‑constrained UCB | Interior exploration | Centre region moderately good (~−1.0) but not optimal |
| 8 | Deliberate centre sampling | Rule out missed basin | Confirmed optimum is off‑centre |
| 9 | Reduced EI \`xi\` | Stronger exploitation | Fewer wasted evaluations |
| 10 | Reduced local perturbation | Very local search | Focused refinement within best basin |
| 11 | Mixed EI + occasional exploration | Safety check | No competing basin discovered |
| 12 | Tight EI with local Gaussian seeds | Trust‑region behaviour | Consistent moderate improvements only near best |
| 13 | Final exploitative EI | Extract remaining gain | Confirmed convergence toward single dominant optimum |

---

## Observed Structure of the Search Space

- One **dominant narrow optimum**
- A broad **moderate plateau** (~−1.0 to −1.3)
- Large regions of **consistently poor performance** (edges/corners)
- Smooth, not chaotic behaviour

---

## What Worked

- Early broad exploration to understand the landscape
- Switching to exploitation once a promising basin was identified
- Using very small perturbations around the best point in later stages
- Controlled exploration to rule out missed optima

## What Didn’t Work

- Aggressive UCB exploration pushed points to unrealistic boundaries
- Excessive global restarts late in the process wasted evaluations

---

## Final Strategy

The final approach used:

- EI with \`xi = 0\` (pure exploitation)
- Local Gaussian perturbations around the best‑known point
- Minimal global restarts as a safeguard

This matched the evidence gathered over the 13 weeks that the optimum lies in a small, specific region of the space.

---

## Key Takeaways

1. Exploration is essential early but harmful late if overused.
2. The response surface contained a single meaningful basin.
3. Local refinement was far more effective than global search in later rounds.
4. Bayesian Optimisation adapts well to noisy, black‑box problems with small datasets.

---

## Conclusion

Over 13 weeks, the optimisation evolved from broad learning to precise refinement. The process successfully identified the structure of the objective function, ruled out alternative optima, and converged on an efficient, exploitation‑focused strategy aligned with the behaviour of the system.

