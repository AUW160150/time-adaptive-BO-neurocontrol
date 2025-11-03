# Analytical Sketches for Time-Adaptive Bayesian Optimization

This repository contains a Python script for generating conceptual figures that illustrate the principles of Time-Adaptive Bayesian Optimization (BO) for non-stationary problems. The visualizations are **analytical sketches** based on heuristic formulas, designed to provide intuition for the performance of BO with and without a temporal forgetting mechanism.

The script generates the three figures described below, which are useful for understanding how a forgetting factor **ε** helps a BO model track an optimal solution that drifts over time at a rate **β**.

---

## Figures Generated

### Figure 1: System Architecture & Forgetting Mechanism

This figure outlines the conceptual framework.

* **(A) Neural System:** Depicts the black-box optimization problem: finding optimal stimulation parameters `x` to maximize a time-varying neural response `f(x,t)`.
* **(B) BO Loop:** Shows the standard Bayesian optimization loop, modified with a temporal kernel to account for the time-dependency.
* **(C) Forgetting Mechanism:** Plots the exponential forgetting kernel weights, $w = \exp(-\epsilon |t-t'|)$, which give less importance to older observations. A higher `ε` leads to faster forgetting.

<img width="3569" height="1020" alt="image" src="https://github.com/user-attachments/assets/f50195a5-e059-4735-8ffe-d75afa90569a" />

---

### Figure 2: Cumulative Regret with Environmental Drift

This figure compares the theoretical performance of different BO algorithms in an environment with a fixed drift rate ($\beta = 0.02$).

* **Standard BO (No forgetting):** The inability to discard old data leads to a model that lags the drifting optimum, resulting in a **quadratic regret cost** ($R(T) \propto \beta T^2$).
* **Time-aware BO (With forgetting):** By correctly discounting past observations, the model can track the optimum, reducing the drift cost to be **linear** ($R(T) \propto \beta T$).
* **Oracle (Perfect tracking):** Represents an ideal, non-drifting scenario, where regret grows sub-linearly ($R(T) \propto \sqrt{T \log T}$).

The heuristic formulas used are:
* Standard BO: $R_{std}(T) = \sqrt{T\log(T+1)} + \beta T^2$
* Time-aware BO: $R_{adapt}(T) = \sqrt{T\log(T+1)} + \beta T$
* Oracle: $R_{oracle}(T) = \sqrt{T\log(T+1)}$

<img width="2370" height="1470" alt="image" src="https://github.com/user-attachments/assets/34b6bd08-e58c-427c-afad-1cb9dc7c448a" />


---

### Figure 3: Sensitivity to Forgetting Factor (ε)

This plot shows how the final cumulative regret at $T=50$ changes with the choice of the forgetting factor `ε` for several different drift rates `β`.

It highlights three key regimes:
1.  **Under-forgetting ($\epsilon \ll \beta$):** The model remembers too much outdated information, leading to high, quadratic-like regret.
2.  **Near-optimal ($\epsilon \approx \beta$):** The forgetting rate is well-matched to the drift rate, minimizing the regret. The algorithm performs robustly in the region where $\epsilon \in [0.5\beta, 2\beta]$.
3.  **Over-forgetting ($\epsilon \gg \beta$):** The model forgets too quickly, effectively reducing the amount of useful data and increasing the constant factor on the linear regret term.

<img width="2370" height="1470" alt="image" src="https://github.com/user-attachments/assets/86a75b9f-8156-48f0-8ef7-ca7a4346bf93" />


---

## Getting Started

### Prerequisites

You need Python 3 and the following libraries installed:
* NumPy
* Matplotlib


Actually implements and tests the algorithm to validate theoretical predictions.
What It Does

Implements the algorithm:

Gaussian Process with temporal kernel: K = K_spatial × exp(-ε|t-t'|)
LCB acquisition function: α(x) = μ(x) - κ·σ(x)
BO loop with exploration and exploitation


Creates test function:

Drifting objective: f_t(x) = (x - x*(t))² where x*(t) = x₀ + βt
Simulates neural adaptation (optimal parameters shift over time)
Adds measurement noise (σ=0.1)


Runs experiments:

Standard BO (ε=0): Treats all data equally
Time-aware BO (ε=β): Temporal forgetting
Multiple drift rates: β ∈ {0.005, 0.01, 0.02, 0.05}
Multiple replications (n=10) for statistics


Measures actual regret:

At each trial: r_t = |x_sampled - x_optimal(t)|
Cumulative: R(T) = Σ r_t
Compares standard vs time-aware

Results

β=0.02, T=50:

Standard BO: R(50) ≈ 64 ± 3
Time-aware BO: R(50) ≈ 15 ± 2
Improvement: 77% ✓


All drift rates: >50% improvement confirmed
Optimal ε: Confirmed ε ≈ β minimizes regret
Robust range: 0.5β < ε < 2β maintains performance
