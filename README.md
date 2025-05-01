# Project 02 — Three-Link Manipulator Control

**AE 544 – Analytical Dynamics – Spring 2025**  
**Student: Luizandrei E. Galanida**  
**SIS: 2568267**

---

## 🎯 Objective

This project analyzes the control of a 3-link manipulator using Lyapunov-based feedback, testing global stability, control robustness, and freedom of control design. It explores how Hamiltonian mechanics can be applied in modern nonlinear control, replicating the dynamics from Example 8.9 in the textbook.

---

## 1. Overview

This project simulates the nonlinear dynamics of a **three-link planar manipulator** using symbolic derivation and Lyapunov-based feedback control laws. It compares the stability and robustness of three controllers:

1. **$Q_1 = -P_1 \dot{q}$** (Proportional damping)  
2. **$Q_2 = -P_2 M(q)\dot{q}$** (Energy-shaped damping)  
3. **$Q_3 = -P_2(M(q) + \Delta M)\dot{q}$** (Perturbed model)

We verify the manipulator's equations of motion symbolically and simulate:

- **100 randomized Monte Carlo trials**  
- A **fixed initial condition** comparison  
- A **robustness test** with mass matrix perturbation $\Delta M$

## 2. Directory & File Organization

SIS_LUIZANDREI_GALANIDA_202503xx/
├── README.md
├── main.m
├── verify_derivation.m
├── dynamics.m
├── compute_mass_matrix.m
├── control_Q1.m / Q2 / Q3
├── monte_carlo_Q1.m / Q2
├── fixed_initial_condition_comparison.m
├── perturbation_Q3.m
├── animate_qdot_decay.m
└── figures/
    ├── fixed_ic_qdot_comparison.png
    ├── fixed_ic_control_comparison.png
    ├── perturbation_q3_qdot.png
    └── qdot_decay_comparison.gif

Zip as `SIS_LUIZANDREI_GALANIDA_202503xx.zip`, ensuring **≤ 100 MB** total size.

## 3. How to Run & Replicate Results

1. **Open MATLAB**.  
2. Navigate into `SIS_LUIZANDREI_GALANIDA_202503xx/`.  
3. Run:
   ```matlab
   >> main
   ```
4. The script will:
   - Symbolically verify the equation:  
     $[M(q)]\ddot{q} + [\dot{M}(q)]\dot{q} - \frac{1}{2}\dot{q}^T[M_q]\dot{q} = Q$
   - Simulate the system using 3 controllers: $Q_1$, $Q_2$, $Q_3$  
   - Output plots in `/figures/`

### Initial Conditions & Setup

- **Time Span**: $t \in [0, 10]$ s  
- **Fixed $q$**: $[0.5,\ -0.4,\ 0.6]^T$,  $\dot{q}$: $[0.2,\ -0.1,\ 0.3]^T$  
- **Random ICs**: $q_i,\ \dot{q}_i \in [-1,\ 1]$ uniformly  
- **Perturbation**: $\Delta M = 0.05 \cdot I$

## 4. Mathematical Setup

### 4.1 Equations of Motion

$$
[M(q)]\ddot{q} + [\dot{M}(q)]\dot{q} - \frac{1}{2}\dot{q}^T[M_q]\dot{q} = Q
$$

- Verified symbolically in `verify_derivation.m`
- $M(q)$ is symmetric and configuration-dependent

### 4.2 Control Laws

- **Q1** (Proportional damping):  
  $Q_1 = -P_1 \dot{q}$
- **Q2** (Mass-weighted damping):  
  $Q_2 = -P_2 M(q) \dot{q}$
- **Q3** (Perturbed Q2):  
  $Q_3 = -P_2 (M(q) + \Delta M) \dot{q}$

## 5. Observations & Analysis

### 5.1 Monte Carlo Results (100 Trials)

| Control Law | Success Rate (%) |
|-------------|------------------|
| $Q_1$       | 100              |
| $Q_2$       | 100              |

> Success if $\|\dot{q}(T)\| < 0.05$ rad/s

---

### 5.2 Fixed Initial Condition Comparison

- Initial state:  
  $q = \begin{bmatrix} 0.5 \\ -0.4 \\ 0.6 \end{bmatrix}$
  
  $\dot{q} = \begin{bmatrix} 0.2 \\ -0.1 \\ 0.3 \end{bmatrix}$

#### $\dot{q}(t)$ under $Q_1$ and $Q_2$  
![qdot](figures/fixed_ic_qdot_comparison.png)

#### Control Inputs  
![control](figures/fixed_ic_control_comparison.png)

> $Q_2$ showed faster convergence with higher initial control effort due to the mass-scaling effect. $Q_1$ was slower but still stabilized the system.

---

### 5.3 Perturbation Test with $\Delta M$

- Applied: $\Delta M = 0.05 \cdot I$  
- Control law:  
  $Q_3 = -P_2 (M(q) + \Delta M) \dot{q}$

#### Resulting $\dot{q}(t)$  
![perturbed](figures/perturbation_q3_qdot.png)

> The perturbed control ($Q_3$) remained stable and performed similarly to $Q_2$, indicating robustness to moderate mass matrix uncertainty.

## 6. Reflections

This project deepened my understanding of Lyapunov-based control design and how system energy can be shaped through feedback. Symbolic verification of the equations helped confirm the nonlinear structure of the mass matrix and interaction terms.

From the fixed initial condition plots, I observed that $Q_2$ converged significantly faster than $Q_1$, albeit with higher initial control effort, as expected. $Q_2$ applied stronger control torque early on, especially for the first joint, and reached steady state nearly twice as fast as $Q_1$. Meanwhile, $Q_1$ still stabilized the system but with a more gradual decay of angular velocities.

The robustness test using $Q_3$ (perturbed mass matrix) showed that even with model uncertainty, the system remained stable and behaved similarly to $Q_2$, indicating good tolerance to small structural errors in $M(q)$.

Lastly, the animated $\dot{q}(t)$ comparison between $Q_1$ and $Q_2$ visually emphasized the faster settling time of $Q_2$, reinforcing the analytical results. Overall, this project tied together symbolic analysis, feedback design, simulation, and post-processing into a cohesive nonlinear dynamics study.

## 7. GenAI Use Disclosure

ChatGPT was used to assist in the structure of code and derivation formatting. All MATLAB scripts were tested and modified to meet the project criteria by the author.



