# Bayesian Optimisation of a Noisy Black‑Box Function (13‑Week Study)

## Overview

This project investigated how **Bayesian Optimisation (BO)** can be used
to efficiently maximise a noisy, two‑dimensional black‑box
log‑likelihood function. The function had multiple local optima, sharp
curvature near the global maximum, and stochastic outputs, making
traditional optimisation methods unreliable.

A **Gaussian Process (GP)** surrogate model with a **Matérn kernel** and
an **Expected Improvement (EI)** acquisition function were used
throughout. Over 13 weeks, the optimisation strategy evolved from broad
exploration to highly targeted micro‑exploitation, ultimately locating a
sharp global maximum with high confidence.

------------------------------------------------------------------------

## Problem Characteristics

-   Black‑box function (no gradients available)
-   Noisy outputs (stochastic log‑likelihood)
-   Search space: (\[0,1\] `\times [0,1]`{=tex})
-   Multiple local peaks
-   Very sharp global maximum
-   Limited evaluation budget

------------------------------------------------------------------------

## Best Observed Result

  Best Point               Best Value
  ------------------------ ------------------
  \[0.509245, 0.510513\]   **0.7637259336**

------------------------------------------------------------------------

## Timeline: Strategy Evolution Over 13 Weeks

  -------------------------------------------------------------------------
          Week Strategy             Rationale              Result
  ------------ -------------------- ---------------------- ----------------
             1 Initial random       Establish broad        Identified rough
               sampling             coverage of domain     high‑value
                                                           regions

             2 First GP + EI        Begin model‑guided     Movement toward
               implementation       search                 promising basin

             3 Increased            Avoid EI local traps   More reliable
               acquisition restarts                        global proposals

             4 Increased GP noise   Handle stochastic      Stabilised GP
               parameter (`alpha`)  outputs                predictions

             5 Exploration‑biased   Deliberate search of   Ruled out weak
               EI (higher `xi`)     new regions            basins

             6 Discovery near (0.5, First evidence of      Values \> 0.6
               0.5)                 dominant peak          observed

             7 Continued            Confirm consistency    Local structure
               exploitation of this                        becoming clear
               region                                      

             8 GP length‑scale      Indicated sharp        Motivated
               warnings             curvature              tighter search

             9 Targeted             Sample clustering near Improved best to
               exploitation         peak                   \~0.69

            10 Directed exploration Test alternative       Confirmed
               of secondary basin   promising region       inferior to main
                                                           peak

            11 Introduced           Exploit local          Significant
               micro‑jitter (δ =    curvature              improvement
               0.01)                                       

            12 Finer micro‑jitter   Precision refinement   Jump to 0.76
               (δ = 0.005)                                 (best value)

            13 Ultra‑fine jitter (δ Convergence            Lower value
               = 0.003)             verification           confirmed peak
                                                           already found
  -------------------------------------------------------------------------

------------------------------------------------------------------------

## Why Bayesian Optimisation Worked Well

-   GP surrogate modelled both prediction and uncertainty
-   EI naturally transitioned from exploration to exploitation
-   Noise‑aware modelling prevented overfitting
-   Shrinking the search space enabled precise localisation
-   Micro‑jitter was crucial for resolving the sharp peak

------------------------------------------------------------------------

## Conclusion

This study shows how adaptive Bayesian Optimisation can efficiently
solve noisy black‑box optimisation problems, culminating in a precise
localisation of a sharp global maximum.
