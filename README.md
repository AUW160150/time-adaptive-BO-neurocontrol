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

<img width="2370" height="1470" alt="image" src="https://github.com/user-attachments/assets/f4d8a8ce-889a-4800-ad88-eb0f11acc1b7" />



---

## Getting Started

### Prerequisites

Requires Python 3 and the following libraries installed:
* NumPy
* Matplotlib

