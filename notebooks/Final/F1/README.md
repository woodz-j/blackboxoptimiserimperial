# Bayesian Optimisation for 2D Contamination Source Detection

## Project Overview

This project explores how Bayesian Optimisation (BO) can efficiently
locate likely contamination sources in a 2D area where only close
proximity produces a measurable reading.

The search space is extremely sparse: most sampled locations return
values effectively equal to zero (often between 10\^-80 and 10\^-225).
This makes naive sampling ineffective and requires a principled strategy
driven by uncertainty-aware modelling.

Over 13 weeks, a Gaussian Process (GP) surrogate model combined with
Upper Confidence Bound (UCB) and Expected Improvement (EI) acquisition
strategies was progressively refined to reliably explore, then locally
refine, the search for contamination sources.

------------------------------------------------------------------------

## Key Technical Challenges

-   Output values span more than 200 orders of magnitude
-   The response surface appears flat across most of the domain
-   Early GP models became overconfident due to clustered samples
-   Acquisition functions repeatedly chose points near existing samples
-   Required careful output transformation and kernel hyperparameter
    tuning

------------------------------------------------------------------------

## Core Design Decisions

  -----------------------------------------------------------------------
  Component               Choice                  Reason
  ----------------------- ----------------------- -----------------------
  Surrogate model         Gaussian Process        Provides predictive
                                                  uncertainty for BO

  Output transform        Signed log10 scaling    Stabilises GP across
                                                  extreme magnitudes

  Kernel                  RBF with wide bounds    Learns both global and
                                                  local structure

  Noise level             Near-zero               Measurements are
                                                  deterministic

  Exploration phase       UCB with high kappa     Forces sampling into
                                                  uncertain regions

  Refinement phase        EI                      Efficient local
                                                  optimisation

  Candidate search        Dense grid              Full acquisition
                                                  landscape inspection
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## What We Learned

1.  Raw outputs break GP behaviour --- log scaling was essential.
2.  Early tight clustering caused GP overconfidence and poor
    exploration.
3.  Increasing kernel length_scale restored meaningful uncertainty
    estimates.
4.  UCB outperformed EI in early sparse exploration.
5.  EI became useful only after sufficient global coverage.
6.  Some samples acted like "support vectors" strongly shaping the GP.
7.  Extremely low readings confirmed a sharply localised source rather
    than a broad region.

------------------------------------------------------------------------

## 13-Week Timeline

  ---------------------------------------------------------------------------
      Week Strategy         Key Change        Result / Observation
  -------- ---------------- ----------------- -------------------------------
         1 Initial GP + UCB Raw outputs       GP unstable, poor guidance

         2 Adjust kappa     Minor improvement Still sampling locally

         3 Diagnose         Recognised        Planned log transform
           magnitude issue  scaling problem   

         4 Signed log10     Stabilised GP     Major behaviour improvement
           transform                          

         5 Introduced EI    Attempted         EI stuck near samples
                            exploitation      

         6 Increased        Restored          Better exploration
           length_scale     uncertainty       

         7 High kappa UCB   Forced            Sampling far regions
                            exploration       

         8 Wider kernel     GP learned global Improved sigma spread
           bounds           structure         

         9 Identified       Better            Guided queries
           support-vector   understanding     
           points                             

        10 Phase-based      Balanced strategy Improved sampling logic
           UCB/EI                             

        11 Local EI         Around best       Confirmed rapid decay
           refinement       region            

        12 Large-scale UCB  Global coverage   Diminishing returns observed
           exploration                        

        13 Mature hybrid    UCB + EI          Converged, principled BO
           pipeline                           process
  ---------------------------------------------------------------------------

------------------------------------------------------------------------

## Current Best Practice Pipeline

1.  Apply signed log10 transform to outputs
2.  Fit GP with:
    -   RBF kernel
    -   Broad length_scale bounds
    -   Multiple optimiser restarts
3.  Use UCB (kappa ≥ 6) for global exploration
4.  Switch to EI for local refinement
5.  Monitor predictive sigma to confirm meaningful uncertainty

------------------------------------------------------------------------

## Why This Approach Is Technically Sound

This pipeline follows established best practices in Gaussian Process
regression and Bayesian Optimisation under sparse, high dynamic-range
conditions. The exploration--exploitation balance mirrors approaches
used in modern BO frameworks.

------------------------------------------------------------------------

## Future Extensions

-   Deep Kernel Learning for scalability
-   Uncertainty-aware neural networks with larger datasets
-   Migration to BoTorch or GPyTorch for GPU acceleration
-   Trust-region Bayesian optimisation for faster local convergence

------------------------------------------------------------------------

## Key Takeaway

> In extremely sparse search spaces, most of Bayesian Optimisation
> effort goes into conditioning the model correctly rather than tuning
> the acquisition function.

------------------------------------------------------------------------

## Suggested Repository Structure

    /src
        gp_model.py
        acquisition.py
        optimizer_loop.py
    /data
        samples.csv
    README.md
