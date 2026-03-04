# Model Background

In this workshop, we extend the OLG model studied in lecture 7 to examine transition dynamics under different TFP shock scenarios and more general preference specifications.

The following section summarizes the key elements of the model, see the lecture notes for details. 
Recall that at each point in time, the economy is populated by $N$ identical young and $N$ identical old households. Households solve a two-period consumption-savings problem:
$$
\begin{aligned}
\max_{c_{y,t},~c_{o,t+1},~a_t} \enskip & \Bigl\{ u(c_{y,t}) + \beta u(c_{o,t+1}) \Bigr\} \\
\text{s.t.} \quad c_{y,t} + a_t &= w_t \\
                  c_{o,t+1} &= (1+r_{t+1})a_t \\
    c_{y,t} &\geq 0, ~ c_{o,t+1} \geq 0,~ a_t \geq 0 \enskip \text{for all } t
\end{aligned}
$$
Firms maximize profits using a Cobb-Douglas technology $Y = z K^\alpha L^{1-\alpha}$, leading to factor prices:
<a id='olg-firm-prices'></a>
$$
\tag{1}
\begin{aligned}
r + \delta &= \alpha z \left(\frac{K}{L}\right)^{\alpha-1} = \alpha z k^{\alpha-1} \\
w &= (1-\alpha) z\left(\frac{K}{L}\right)^{\alpha} = (1-\alpha) z k^{\alpha}
\end{aligned}
$$
where $k_t = K_t/L_t$ is the capital-labor ratio.

Let $s_t$ be the savings rate of the young generation at time $t$, defined as $s_t = a_t/w_t$. 
The household's optimality conditions imply that the savings rate satisfies
$$
\tag{2}
s_t = \left[ 1 + \beta^{-\frac{1}{\gamma}} (1+r_{t+1})^{1-\frac{1}{\gamma}}\right]^{-1}
$$
where $\beta$ is the discount factor and $\gamma$ is the coefficient of relative risk aversion.
Note that the savings rate $s_t$ depends on the interest rate $r_{t+1}$, which in turn depends on the 
savings in period $t$ and therefore on the savings rate $s_t$. Thus, we cannot solve for $s_t$ except in special cases. 
For most parameter values, we therefore need to implement a root-finding algorithm to solve for the equilibrium savings rate in each period.

***
# Exercise 1: Transitory vs persistent TFP changes

In the lecture, we studied a *permanent* shock for the special case of $\gamma = 1$ where the savings rate is constant and given by
$$
s_t = \frac{\beta}{1 + \beta}
$$
Here, we examine scenarios where the economy eventually returns to the original steady state, i.e., the long-run TFP level is unchanged. We will compare the transition dynamics under two scenarios: a transitory shock and a persistent shock.

### Transition dynamics

Consider the following sequence of events:

1.  The economy is in the steady state implied by the original parameters.
2.  The economy is hit by an unexpected drop in TFP $z$ of 10%.

    - **Scenario A (Transitory)**: The drop is a one-time shock at $t=1$, and TFP returns to its original level for $t \geq 2$.
    - **Scenario B (Persistent)**: The drop occurs at $t=1$ and then decays geometrically over time, following the process 
      $$
      z_{t+1} = (1-\kappa) z_t + \kappa z
      $$
      where $z$ is the original steady-state TFP level and $\kappa$ governs the speed of mean reversion.
      This means that TFP will eventually return to its steady-state level. 
      For this exercise, assume that $\kappa = 0.1$, i.e., in each period the gap to the steady-state value shrinks by 10%.

  
    Once the TFP shock is realized, all households fully understand the future path of TFP and make their optimal decisions accordingly.


## Implementation

In the following, you are asked to adapt the code from lecture 7 to compute the transition dynamics for both scenarios.
You should use the template file [`workshop07_ex01.py`](workshop07_ex01.py)
provided for this exercise to implement your solution. 

Use the following parameters to solve this problem:


| Symbol | Description | Value |
|--------|-------------|-------|
| $\alpha$ | Capital share in production function | 0.36 |
| $\delta$ | Depreciation rate | 1.0 |
| $z$ | TFP | 1.0 |
| $\beta$ | Discount factor (0.96 per year, 30-year periods) | $0.96^{30}$ |
| $\gamma$ | Coefficient of relative risk aversion | 1.0 |
| $N$ | Number of households per cohort | 1 |
| $\kappa$ | Mean reversion parameter (scenario B) | 0.1 |


### Tasks

1. Implement the function `simulate_olg(z_series, eq)` in [`workshop07_ex01.py`](workshop07_ex01.py) to accept a full time-series of TFP values. The function signature is as follows:

    ```python
    def simulate_olg(z_series, eq: SteadyState):
        """
        Simulate the transition dynamics of the OLG model for a given TFP series.
        This implementation assumes log utility (gamma=1).

        Parameters
        ----------
        z_series : np.ndarray
            Time series of TFP values
        eq : SteadyState
            Initial steady-state equilibrium
            
        Returns
        -------
        sim : Simulation
            Simulation of the OLG model
        """
    ```

    The function should compute and return the transition path of the economy for a given sequence of TFP values. 
    Adapt the code from `simulate_olg()` we defined in lecture 7 for this purpose.
    
2. **Scenario A (Transitory)**: 

    -   Create a time series of 20 periods for TFP $z$ which has the steady-state value in all periods except for a one-time 10% drop at $t=1$.
    -   Use the simulation function you implemented in task 1 to compute the transition path of the economy under this scenario.
    -   Adapt the plotting function `plot_simulation()` from lecture 7 to show the transition path of key variables. The function should produce a figure with 4-by-2 subplots showing the transition path of TFP, $Y$, $K$, $s$, $c_y$, $c_o$, $r$, and $w$. Place this function in `workshop07_ex01.py`.
    -   Explain the intuition behind the transition dynamics you observe in the plots, in particular the behavior of the capital stock, output and the interest rate.

3. **Scenario B (Persistent):** 

    - Extend the `Parameters` dataclass in `workshop07_ex01.py` to include the mean reversion parameter $\kappa$ for the persistent shock scenario.
    - Create a time series of 20 periods for TFP $z$ that starts at the steady-state value in $t=0$, then drops by 10% at $t=1$ and subsequently decays geometrically back to the steady-state level according to the process described above.

    -   Use the simulation function you implemented in task 1 to compute the transition path of the economy under this scenario.
    -   Use the plotting function `plot_simulation()` to plot the transition path of the same key variables as in Scenario A.
    -   How does the dynamic response of the economy differ from the transitory shock scenario? Explain the intuition behind the differences you observe in the plots.

