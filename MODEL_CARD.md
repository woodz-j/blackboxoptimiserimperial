# Model Card: Black-Box Optimisation Strategy

## Overview

**Model name:** Bayesian Optimisation with Gaussian Process Surrogate  
**Model type:** Sequential decision-making / black-box optimisation  
**Core techniques:**  
- Gaussian Process Regression (Matern kernel)  
- Expected Improvement (EI) acquisition function (primary)  
- Upper Confidence Bound (UCB) acquisition function (secondary)  

**Version:** Iterative, week-by-week refinement (Weeks 1–3 documented)

This model is not a single static predictor but an **adaptive optimisation strategy** that evolves as new observations are collected. Its purpose is to guide query selection for maximising unknown black-box functions under strict evaluation constraints.

---

## Intended Use

### Suitable Use Cases
- Black-box optimisation problems with:
  - expensive or delayed evaluations
  - unknown internal structure
  - limited query budgets
- Hyperparameter tuning
- Scientific or engineering optimisation
- Educational demonstrations of Bayesian Optimisation
- Portfolio evidence of ML decision-making under uncertainty

### Uses to Avoid
- Tasks requiring guaranteed global optima
- Fully supervised prediction problems
- High-frequency or real-time decision systems
- Settings where uncertainty estimates are unreliable or misleading
- Production deployment without further validation and robustness checks

---

## Strategy and Methodology

### Core Decision-Making Approach

The strategy models each unknown function using a **Gaussian Process surrogate**, which provides:
- a mean prediction (expected performance)
- an uncertainty estimate (variance)

Query decisions are made using acquisition functions that balance:
- **exploitation** (refining high-performing regions)
- **exploration** (sampling uncertain or unseen regions)

---

### Strategy Evolution Across Rounds

#### Early Rounds 
- Sparse initial data (10 points per function)
- Heavy reliance on uncertainty-aware acquisition
- Primary focus on **sample efficiency**
- EI and UCB used to avoid premature exploitation

#### Mid Rounds 
- Strategy adapts based on observed improvement or stagnation
- Exploration increased when no improvement is observed
- Exploitation prioritised after strong results
- Acquisition hyperparameters (ξ / κ) tuned heuristically

#### Later Rounds 
- Increased trust in surrogate predictions
- Local refinement near strong optima
- Use of random restarts to avoid local acquisition optima
- Recognition of “anchor” points shaping the decision surface

---

### Decision Surface Interpretation

Observed behaviour across functions:
- **High-value points** act as anchors, pulling the GP mean upward
- **Low-value points** act as repellers, reducing uncertainty in poor regions
- These points function similarly to *support vectors*, disproportionately influencing acquisition decisions

The acquisition function then decides whether to:
- exploit anchored regions, or
- explore uncertain regions that may contain hidden optima

This balance is controlled explicitly via:
- exploration parameter **ξ** (EI)
- confidence parameter **κ** (UCB)

---

## Performance Summary

### Evaluation Context
- Eight black-box functions
- Dimensionality from 2D to 8D
- One query per function per week
- No access to true evaluation metric or function internals

### Metrics Used
- Relative improvement over best observed value
- Stability of improvements across rounds
- Qualitative assessment of exploration vs exploitation
- Reflection-based justification of query choices

### Summary of Results
- Consistent improvement across most functions
- Strong local optima identified even in high-dimensional settings
- No guarantee of global optima (expected in black-box optimisation)
- Performance evaluated holistically, not purely numerically

---

## Assumptions and Limitations

### Key Assumptions
- The objective function is smooth enough to be approximated by a GP
- Past observations are informative for future queries
- Uncertainty estimates are meaningful guides for exploration

### Limitations
- Curse of dimensionality in 6D–8D functions
- GP smoothness assumptions may misrepresent sharp surfaces
- Edge optimism due to inflated uncertainty near boundaries
- Limited query budget restricts exhaustive exploration
- Potential to settle in strong local optima

---

## Ethical and Responsible AI Considerations

- Full transparency of modelling assumptions and decision logic
- Explicit documentation of uncertainty and limitations
- No human-subject or sensitive data involved
- Emphasis on reproducibility through:
  - documented strategy evolution
  - weekly reflections
  - consistent repository structure

Transparency ensures that results are not overstated and that the approach can be adapted responsibly to real-world problems.

---

## Reflection on Model Design

### How the Model Makes Decisions
- Decisions are driven by probabilistic reasoning, not fixed rules
- Each new observation reshapes the surrogate model
- Exploration vs exploitation is treated as a controllable trade-off

### Strengths
- Sample-efficient
- Adaptive and interpretable
- Well-suited to incomplete knowledge settings
- Encourages reflective, data-driven reasoning

### Limitations
- Sensitive to kernel and acquisition choices
- Performance depends on careful heuristic tuning
- Not robust to highly discontinuous or adversarial functions

### On Model Card Detail
The current structure is sufficient because:
- It clearly separates *what the model does* from *how decisions are made*
- It aligns directly with the README and Datasheet
- It balances technical depth with readability for reviewers

Additional detail (e.g. full mathematical derivations) would reduce clarity without significantly improving interpretability for the intended audience.

---

## Summary

This model represents a **thoughtful, adaptive black-box optimisation strategy**, designed to operate under uncertainty and constraints. Its value lies not only in the results achieved, but in the documented reasoning process that guided each decision — a key requirement for real-world ML and data-driven systems.
