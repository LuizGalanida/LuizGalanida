# AE 544 – Project 01

**Attitude Representations & Impact of Singularities**  
**Author**: Luizandrei E. Galanida  
**Course**: AE 544 – Analytical Dynamics  
**Date**: March 12, 2025  

---

## 1. Overview

This project goes over **three** standard representations for a rigid body’s orientation and the **singularities or ambiguities** that they may run into:

1. **3–2–1 Euler Angles**  
   - Known to have issues when the middle angle $\theta$ approaches $\pm 90^\circ$.  
2. **Euler Parameters (Quaternions)**  
   - No singularities, but $(\mathbf{q} \leftrightarrow -\mathbf{q})$ is the same physical orientation.  
3. **Classical Rodrigues Parameters**  
   - Can **blow up** if the total rotation angle reaches $180^\circ$ (i.e., $\Phi = \pi$).

We **numerically integrate** each representation’s kinematic equation, using a **time‐varying angular velocity** ${\omega}(t)$ that pushes the system near the known “problem angles.” We compare three ODE solvers:

- **`ode45`** (non‐stiff, variable‐step),  
- **`ode15s`** (stiff, variable‐step),  
- **`fixedRK4`** (a custom 4th‐order Runge–Kutta at fixed steps).

Finally, a **3D animation** (via quaternions) confirms that the body’s rotation is physically smooth despite any issues in the other representations.

---

## 2. Directory & File Organization

```
SIS_LUIZANDREI_GALANIDA_202503xx/
  ├── README.md       <-- This file
  ├── main.m          <-- Single-entry MATLAB script
  ├── .png files
  └── .gif animation
```

Zip as `SIS_LUIZANDREI_GALANIDA_202503xx.zip`, ensuring **≤ 100 MB** total size.

---

## 3. How to Run & Replicate Results

1. **Open MATLAB**.  
2. Navigate into `SIS_LUIZANDREI_GALANIDA_202503xx/`.  
3. Type:
   ```matlab
   >> main
   ```
4. The script will:
   - Integrate **Euler angles** $(\phi,	\theta,\psi)$, **Quaternions** $(q_0,q_1,q_2,q_3)$, and **Rodrigues** $(\sigma_1,\sigma_2,\sigma_3)$ 
   - Use each of the solvers: `ode45`, `ode15s`, and `fixedRK4`  
   - Generate time‐series plots  
   - Optionally produce a **3D animation** from the quaternion solution

### Initial Conditions & Setup

- **Time Span**: $(t \in [0,10]\)$ s  
- **Euler Angles**: $(\phi=0)$, $(\theta=80^\circ)$, $(\psi=0)$  
- **Quaternions**: $[1,0,0,0]$  
- **Rodrigues**: $[0,0,0]$  
- **${\omega}(t)$** (example profile):
  ```matlab
  function w = wProfile(t)
      w = [0.2;  0.6 + 0.1*t;  0.0];
  end
  ```
  This ensures the 3–2–1 middle angle $\theta$ can cross $90^\circ$ if $\theta$ increases enough.

> **Note**: In our **final** `main.m` script, the default setup may slightly differ (e.g., final time or step sizes). Adjust as needed to see more/less singularity.

---

## 4. Mathematical Setup

### 4.1 Euler Angles (3–2–1)
$\[\dot{{\Theta}} = \mathbf{T}(\phi,\theta){\omega}, \quad{\Theta} = [\phi,\theta,\psi]^T.\]$
The transform $\mathbf{T}$ becomes singular if $(\theta \pm 90^\circ)$.

### 4.2 Quaternions
$\[\dot{\mathbf{q}} = 	\frac12\{\Omega}({\omega})\\mathbf{q}, \quad\mathbf{q}\in\mathbb{R}^4.\]$
No blow‐up, but $(\mathbf{q}\leftrightarrow -\mathbf{q})$ is the same attitude.

### 4.3 Classical Rodrigues Parameters
$\[\dot{{\sigma}} = 0.5 \Bigl[(1 - |{\sigma}|^2) \mathbf{I} \+\ 2[{\sigma}]_	x \+ 2{\sigma} {\sigma}^T\Bigr]{\omega}.\]$
Can diverge near $(\Phi=180^\circ)$.

---

## 5. Observations & Analysis

### 5.1 Euler Angle Singularity
- As $\theta$ nears $90^\circ$, $\dot{\phi}$ or $\dot{\psi}$ can blow up in the 3–2–1 formulation.  
- Variable‐step solvers (`ode45`, `ode15s`) shrink their step size and often *smoothly* handle the near‐singular region. A fixed‐step solver (`fixedRK4`) may produce larger numerical errors or blow up.

### 5.2 Quaternion ±Ambiguity
- $\mathbf{q}$ remains finite, but flipping sign $\mathbf{q} -\mathbf{q}$ does **not** change the physical orientation.

### 5.3 Rodrigues Blow‐Up
- If the rotation angle crosses $180^\circ$, ${\sigma}$ can become unbounded/very large.
- Our chosen motion in `main.m` might push the body close to larger angles over the specified duration.

### 5.4 3D Animation
- We animate the quaternion solution from `ode45`, rotating a triad in inertial space.  
- This demonstrates a smooth physical rotation, confirming that any “blow‐up” is purely in the coordinate representation.

---

## 6. Figures & Animations

Upon running `main.m`, the code generates:
1. **3 Euler Angles vs. Time** (in degrees)  
2. **3 Quaternion Components vs. Time**  
3. **3 Rodrigues Parameters vs. Time**  
4. (Optionally) A **3D Animation** using the quaternion solution.

You can save the animation as a `.gif` by modifying the `animate3D()` (or `animateAttitude()`) function in `main.m` to capture frames via `getframe()` and `imwrite()` if needed. Keep the `.gif` under **100 MB**.

---

## 7. Conclusion

Each representation can encounter a “problem angle” under certain rotations. The three ODE solvers handle these steep regions differently, especially regarding **adaptive** vs. **fixed** step‐size approaches. Meanwhile, **quaternions** avoid infinite singularities but exhibit a **sign ambiguity**. The **3D animation** confirms that physically, the rigid body’s orientation remains continuous despite any parameter blow‐ups.

All results, instructions, and analyses are **self‐contained** in this `README.md`; no additional report is necessary per the project guidelines.

---
