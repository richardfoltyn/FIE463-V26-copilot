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
# Exercise 2: Transition dynamics for general CRRA preferences

In lecture 7 and exercise 1, we have imposed log preferences ($\gamma = 1$) as this simplifies simulating the economy. In this exercise, we relax this assumption and implement simulation code for the general CRRA case. The remainder of this exercise is unchanged from exercise 1, but we focus on Scenario B (the persistent shock scenario).

## Optimal savings rate

Recall that the optimal savings rate in $t$ is given by
<a id='olg-ex2-trans-srate'></a>
$$
\tag{3}
s_t = \left[ 1 + \beta^{-\frac{1}{\gamma}} (1+r_{t+1})^{1-\frac{1}{\gamma}}\right]^{-1}
$$
It is important to note that the savings rate $s_t$ in period $t$ depends on the return on savings $r_{t+1}$ realized in $t+1$. Because of the perfect foresight assumptions, rational households know the future path of interest rates after a shock hits the economy (full information rational expectations, a common assumption in macroeconomics).

As you can guess from [(3)](#olg-ex2-trans-srate), the expression for the savings rate does not have an analytical solution in general.
To see this, recall from the firm's
first-order conditions [(1)](#olg-firm-prices) that the interest rate *next period* is given by
$$
r_{t+1} = \alpha z_{t+1} \left(\frac{K_{t+1}}{N}\right)^{\alpha-1}  - \delta
$$
where $L = N$ follows from labor market clearing.
Moreover, from the asset market clearing condition we have that
$$
K_{t+1} = N a_t = N s_t w_t
$$
so ultimately $r_{t+1}$ itself depends on $s_t$:
$$
r_{t+1} = \alpha z_{t+1} \left( s_t  w_t \right)^{\alpha-1} - \delta
$$
It is therefore not possible to find a closed-form solution for $s_t$ with $\gamma \neq 1$, and thus
we need to resort to root-finding in each period of the simulation. The remainder of this exercise guides you through the process of implementing this simulation.


## Euler equation errors

In previous lectures, we applied root-finding to first-order conditions,
and we are going to repeat this approach here. 
The first-order condition for the household's optimization problem is the usual Euler equation,
given by
$$
u'(c_{y,t}) = \beta (1+r_{t+1}) u'(c_{o,t+1})
$$
As shown in the lecture notebook, plugging in the CRRA utility function and the budget constraints, we can rewrite this as
$$
\Bigl(\underbrace{(1-s_t)w_t}_{\equiv c_{y,t}}\Bigr)^{-\gamma}
 = \beta (1+r_{t+1}) \Bigl(\underbrace{(1+r_{t+1})s_t w_t}_{\equiv c_{o,t+1}}\Bigr)^{-\gamma}
$$
which, for given factor prices $w_t$ and $r_{t+1}$, is a nonlinear equation in $s_t$ that can be solved numerically for the optimal savings rate.

To this end, we define the function $f(s)$ to return the difference between the left- and right-hand sides of the Euler equation:
$$
f(s) = \Bigl((1-s_t)w_t\Bigr)^{-\gamma} - \beta (1+r_{t+1}) \Bigl((1+r_{t+1})s_t w_t \Bigr)^{-\gamma}
$$
In each period along the transition path, we need to find the optimal savings rate such that $f(s^*) = 0$ to solve the household's optimization problem.

For a given guess of $s$, we can evaluate both sides of the Euler equation as follows:

1.  Using the pre-determined $K_t = N a_{t-1}$ (which follows from the asset market clearing condition) and the current $z_t$, 
    we compute $w_t$ from the firm's first-order conditions [(1)](#olg-firm-prices).
2.  Using the guess for $s$, we compute next period's capital stock
    $K_{t+1} = Nsw_t$.
3.  With $K_{t+1}$ and $z_{t+1}$ in hand, we compute $r_{t+1}$ from the 
    firm's first-order conditions (note that households know $z_{t+1}$
    with certainty due to the perfect foresight assumption).
4.  We now have all terms on the left- and right-hand sides of the Euler equation
    and can therefore compute the Euler equation error $f(s)$ for the current guess $s$.

## Tasks

You can reuse almost all the functions you have from the 
[`workshop07_ex01.py`](workshop07_ex01.py) 
file for this exercise, which you should import directly into 
[`workshop07_ex02.py`](workshop07_ex02.py) instead of copying the code.

1. Implement the function `euler_err(s, w, z_next, par)` in [`workshop07_ex02.py`](workshop07_ex02.py) 
    to calculate the deviation from the Euler equation. The function should have the following signature:

    ```python
    def euler_err(s, w, z_next, par):
        """
        Compute the euler equation error for a given savings rate.

        Parameters
        ----------
        s : float
            Guess for the savings rate
        w : float
            Current wage rate
        z_next : float
            Next period's TFP
        par : Parameters
            Parameters for the given problem

        Returns
        -------
        float
            Euler equation error
        """
    ```
    
2.  Visually check that the function `euler_err()` is correctly implemented by plotting the Euler equation error as a function 
    of the savings rate $s$ on the interval $[0.1, 0.9]$ using the wage rate $w$ and the TFP level $z$ that prevail 
    in the initial steady state if we assume that $\gamma = 1$ (this is the same steady state as in exercise 1).

3. Implement `simulate_olg_crra(z_series, eq)` in [`workshop07_ex02.py`](workshop07_ex02.py) 
    to simulate the transition dynamics of the economy for a given time series of 
    TFP values and general CRRA preferences. 
    The function should have the following signature:

    ```python
    def simulate_olg_crra(z_series, eq: SteadyState):
        """
        Simulate the transition dynamics of the OLG model for arbitrary RRA values.

        Parameters
        ----------
        z_series : np.ndarray
            A time series of TFP values for each period.
        eq : SteadyState
            The initial steady state of the economy.

        Returns
        -------
        Simulation
            A dataclass containing the simulated transition path of key variables.
        """
    ```
    
    In each period, you should use a root-finding algorithm to find the optimal savings rate $s_t$ that solves the Euler equation, using the `euler_err()` function you implemented in task 1.
4. Simulate 20 periods for $\gamma = 1$ using the persistent TFP shock from exercise 1.
    Plot the results to verify that you get the same results as in exercise 1.
5. Simulate 20 periods for $\gamma = 5$ using the persistent TFP shock from exercise 1.
    Plot the results and compare the transition dynamics to the case with log preferences.
