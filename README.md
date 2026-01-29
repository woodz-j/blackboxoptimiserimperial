## Project Overview

The **Black-Box Optimization Capstone Project** is a hands-on machine learning challenge designed to simulate real-world conditions where the inner workings of a system are hidden. In this environment, we observe how inputs affect outputs, but we do not have access to the model’s internal logic or exact evaluation metrics.

The main goal is to develop, tune, and optimise a machine learning model using **iterative, data-driven experimentation**. Success depends on the ability to make smart, evidence-based decisions to improve model performance under uncertainty.

In practice, ML engineers frequently encounter black-box scenarios, such as working with proprietary APIs, limited feedback loops, or unseen data pipelines. This capstone mirrors those challenges by testing the ability to:

- Build and refine models without full visibility
- Think critically about experimentation and evaluation
- Optimise performance under uncertainty

Over the course of the project, the model improves incrementally based on observed outcomes, applying optimisation strategies and analytical reasoning rather than relying on perfect information.

This capstone focuses on machine learning while also building strong foundations for a future career in **Data Engineering**. It encourages thinking like both an ML practitioner and a data engineer by developing skills to build, optimise, and maintain robust data systems that power intelligent applications.

---
## Non-technical write-up
In this project, I built a system that learns how to make smart decisions when testing expensive or unknown processes. Instead of trying every possible option, the model uses past results to predict which choices are most promising and which are still uncertain. Early on, it explores widely to learn how the system behaves, and later it focuses on refining the best-performing options. This approach allowed me to find strong solutions using far fewer tests than a trial-and-error method. Overall, the project demonstrates how careful, data-efficient optimisation can save time and resources while still achieving high performance.
---

## Inputs and Outputs

### Inputs

- A vector of numerical values corresponding to the dimensions of the target function.
- Each query must follow this format (x₁, x₂, ..., xₙ) where each xi starts with 0. and is specified to six decimal places
- Example (for a 2D function): 0.242364 - 0.231669


### Initial Data

Each function starts with **10 known (x, y) data points**, where:
- `x` represents the input vector
- `y` represents the function output

**Example format:**

 x: [0.1, 0.4], y: 0.45  
 x: [0.5, 0.9], y: 0.61 

### Outputs

- A single numerical value (`y`) representing the evaluated result of the black-box function for the submitted input.
- After each submission, the new `(x, y)` pair is added to the dataset, allowing the model to be refined iteratively and predictions to improve over time.

### Additional Constraints

- Only **one query per iteration** is allowed (one input vector).
- Once submitted, an input cannot be modified.

---

## Challenge Objectives

### Objective

The objective is to find input values that **maximise the output** of **eight hidden black-box functions**.

Each function is a maximisation problem, and the goal is to iteratively discover the input that yields the highest possible output under strict query constraints.

---

## Constraints and Limitations

- Only one query per function can be submitted **per week**
- No access to the underlying model or its equations
- Outputs are returned after processing, resulting in **delayed feedback**

These limitations are intentional and reflect real-world optimisation scenarios.

---

## Technical Approach

I primarily used **Bayesian Optimisation (BO)** with:
- a **Gaussian Process (GP)** surrogate model
- **Expected Improvement (EI)** as the main acquisition function
- **Upper Confidence Bound (UCB)** for comparison and specific cases

### Expected Improvement (EI)

- Balances:
  - **Exploration** (sampling uncertain regions)
  - **Exploitation** (refining high-value areas)
- Automatically scales with model uncertainty and expected gain
- Requires minimal manual tuning

### Upper Confidence Bound (UCB)

- Prioritises regions with either:
  - high predicted mean, or
  - high uncertainty
- Increasing the exploration parameter (κ) explicitly biases the search toward uncertain regions
- In contrast to EI, which balances exploration and exploitation automatically based on expected improvement

---

## Strategy Evolution

Across the first three query submissions, the strategy evolved from **curiosity-driven exploration** to **evidence-based optimisation**. By analysing model outputs, tuning acquisition parameters, and adapting dynamically, I developed a more pragmatic and data-aware optimisation mindset.

---

## Heuristic Techniques Used

### Performance-Based Adaptation

- Used the most recent output as feedback to guide strategy:
  - Poor output → increase exploration
  - Strong output → lean toward exploitation

### Exploration–Exploitation Balance

- Early rounds favoured exploration due to sparse data
- Later rounds focused on exploitation as promising regions emerged
- Decisions evolve dynamically based on:
  - observed performance
  - uncertainty levels
  - model confidence

### Dynamic Exploration Parameter

- When outputs plateau or uncertainty remains high:
  - increase the EI parameter (ξ) to encourage exploration
- When outputs improve significantly or EI peaks near the current best:
  - lower ξ to refine the search locally around potential optima

### Random Restarts

- Added multiple random restarts during acquisition optimisation to reduce the risk of converging to local optima

### Kernel Flexibility

- Plan to adjust GP kernel parameters (e.g. reducing ν in the Matérn kernel) to better model complex or high-dimensional response surfaces

