npm install mathjax@3
mv node_modules/mathjax/es5 <path-to-server-location>/mathjax

# AE 544 – Project 01

**Project Title**: Attitude Representations and Singularities  
**Author**: Luizandrei E. Galanida
**Course**: AE 544 – Analytical Dynamics  
**Date**: March 12, 2025

---

## 1. Overview


This project investigates the **singularities** or **ambiguities** that arise in three widely used attitude representations:

1. **3–2–1 Euler Angles**  
   - Exhibits singularities when the middle angle $(\theta)$ approaches $(\pm 90^\circ\)$.
2. **Euler Parameters (Quaternions)**  
   - Has a $(\pm \mathbf{q}\)$ ambiguity, which still represents the same physical orientation.
3. **Classical Rodrigues Parameters**  
   - Goes to infinity if the total rotation angle is $(180^\circ\)$ (i.e., $\Phi$ = $\pi$).

We **numerically integrate** simple kinematic equations, driven by a **time‐varying angular velocity** $(\boldsymbol{\omega}(t)\)$, specifically chosen to nudge each representation near its problematic region. Three ODE solvers are used:

- `ode45` – a non‐stiff variable‐step solver,  
- `ode15s` – a stiff (but still variable‐step) solver,  
- `fixedRK4` – a manually coded 4th‐order Runge–Kutta with a fixed step size.

Finally, we **animate** the quaternion solution in 3D to illustrate how the **actual** rigid‐body orientation remains physically smooth, even if certain coordinate sets blow up or flip signs.

---

## 2. Directory & Files

Please ensure the following structure is maintained:


Keep the **total ZIP** size **≤ 100 MB**.

---

## 3. How to Run

1. **Open MATLAB** (R2021 or newer).  
2. Navigate into the folder `SIS_FIRST_LAST_202503xx/`.  
3. Type:
   ```matlab
   >> main
   
