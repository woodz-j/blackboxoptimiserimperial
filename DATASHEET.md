# Datasheet for the Black-Box Optimisation (BBO) Capstone Project

## Motivation

This dataset was created to support a **black-box optimisation task** in which the objective is to maximise unknown functions under strict evaluation constraints. The project mirrors real-world ML settings where the system is opaque, feedback is delayed or noisy, and optimisation decisions must be made iteratively using limited information.

The dataset directly supports the goals described in the README:
- applying Bayesian Optimisation in practice,
- reasoning about exploration vs exploitation,
- adapting strategy based on observed outcomes rather than fixed metrics.

Weekly reflections document *why* each query was chosen, making the dataset a record of **decision-making under uncertainty**, not just numerical results.

---

## Composition

The dataset consists of observations from **eight independent black-box functions**, each treated as a separate optimisation problem.

### Inputs
- Continuous-valued vectors (`x`)
- Dimensionality increases from **2D to 8D** across functions
- Inputs are bounded and submitted in a strict portal-compatible format

### Outputs
- Single scalar response (`y`) per input
- Represents performance, score, or objective value
- May be noisy and non-smooth

### Initial Data
As described in the README:
- Each function starts with **10 known input–output pairs**
- Stored as:
  - `initial_inputs.npy`
  - `initial_outputs.npy`

### Iterative Growth
- One new query per function per week
- Dataset grows incrementally across optimisation rounds
- Weekly folders preserve the chronological evolution of strategy

### Known Gaps
- No access to:
  - internal function structure
  - gradients
  - true evaluation metric
- Sparse coverage in higher dimensions (6D–8D), consistent with the challenges noted in Weeks 1–3

---

## Collection Process

### Query Generation Strategy

Queries are generated using **Bayesian Optimisation**, as described consistently across weekly reflections:

- Gaussian Process surrogate models (Matern kernel)
- Acquisition functions:
  - Expected Improvement (EI) — primary
  - Upper Confidence Bound (UCB) — used selectively

The strategy evolves across weeks:

- **Initial**  
  Emphasis on sample-efficient acquisition (EI/UCB) to balance exploration and exploitation with minimal data.
- **Intermediate 2**  
  Strategy adapts based on observed outputs — increasing exploration when stagnation occurs and exploiting when strong improvements appear.
- **Advanced 3**  
  Greater reliance on surrogate predictions, local refinement near strong optima, and heuristic tuning of exploration parameters (ξ / κ).

### Decision Surface Interpretation
As observed in later reflections:
- High-value points act as **anchors**, shaping the GP mean
- Low-value points act as **repellers**, reducing uncertainty in poor regions
- These points function similarly to *support vectors*, heavily influencing acquisition decisions

The exploration–exploitation balance is explicitly controlled through acquisition parameters, consistent with README explanations.

### Time Frame
- Data collected over multiple weekly rounds
- Each round depends on delayed feedback from the previous submission
- Strategy evolves cumulatively, not independently per week

---

## Preprocessing and Uses

### Preprocessing
- Data stored in raw numeric `.npy` format
- No feature engineering or dimensionality reduction applied
- Gaussian Process models internally handle scaling and noise estimation
- This preserves the integrity of the black-box setting described in the README

### Intended Uses
- Demonstrating black-box optimisation workflows
- Studying Bayesian Optimisation behaviour across dimensionality
- Analysing exploration–exploitation trade-offs
- Supporting reflective analysis and portfolio presentation

### Inappropriate Uses
- Treating outputs as ground truth
- Using the dataset for supervised learning benchmarks
- Assuming convexity, smoothness, or unimodality beyond observed evidence
- Production deployment without additional validation

---

## Distribution and Maintenance

### Availability
The dataset is distributed as part of this GitHub repository and organised to reflect the optimisation process described in the README:


Each function folder contains:
- initial input/output files
- a `Run_Function.ipynb` notebook
- updated datasets reflecting weekly queries

### Maintenance
- Maintained by the project author
- Updated weekly as new outputs are received
- Documentation and reflections are updated alongside data to preserve context

### Terms of Use
- Educational and demonstrative use only
- Designed as a portfolio artefact
- Attribution expected for reuse in academic or professional contexts

---

## Interpretation Guidance

This dataset should be interpreted as a **trajectory of optimisation decisions**, not a complete mapping of any function. Success is defined consistently with the README and reflections:
- thoughtful strategy adaptation,
- principled use of uncertainty,
- and evidence-based iteration.

Failure to find a global optimum is not an error but an expected outcome in realistic black-box optimisation settings.
