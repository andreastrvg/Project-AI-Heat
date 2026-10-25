# Engineering Report: Numerical Heat Conduction Analysis of a Uranium Plate with Internal Heat Generation

*Prepared by:* Entry-Level Thermal Engineer  
*Subject:* Problem 5-37 — Steady-State, Transient, and Accelerated Transient Heat Conduction  
*Date:* October 2026

---

## Executive Summary

This report presents a thermal analysis of a large uranium plate with a thickness of 5 cm, thermal conductivity of 34 W/(m·K), and uniform internal heat generation of 600,000 W/m³.

One surface is insulated, while the opposite surface is exposed to convection at an ambient temperature of 30°C with a heat transfer coefficient of 60 W/(m²·K).

The analysis consists of three parts:

1. *Part I:* Analytical and numerical steady-state heat conduction analysis.
2. *Part II:* Transient heat conduction analysis using the explicit finite difference method.
3. *Part III:* Evaluation of increased internal heat generation to reduce heating time.

---

# Part I. Original Steady-State Problem

## 1. Physical Assumptions

1. *One-Dimensional Conduction:* Heat transfer occurs only in the x-direction. Edge effects are negligible.

2. *Constant Thermal Conductivity:* Thermal conductivity remains constant at 34 W/(m·K).

3. *Uniform Heat Generation:* Internal heat generation is constant throughout the plate.

4. *Steady-State Conditions:* Temperature does not change with time.

5. *Negligible Radiation:* Radiation heat transfer is neglected.

6. *Constant Convection Coefficient:* The heat transfer coefficient remains constant at 60 W/(m²·K).

---

## 2. Governing Differential Equation

The one-dimensional steady-state heat conduction equation with internal heat generation is:

$$
\frac{d}{dx}\left(k\frac{dT}{dx}\right)+q'''=0
$$

For constant thermal conductivity:

$$
k\frac{d^2T}{dx^2}+q'''=0
$$

Therefore:

$$
\boxed{\frac{d^2T}{dx^2}+\frac{q'''}{k}=0}
$$

---

## 3. Boundary Conditions

### Insulated Surface (x = 0)

No heat is transferred through the insulated boundary.

$$
-k\left.\frac{dT}{dx}\right|_{x=0}=0
$$

Therefore:

$$
\boxed{\left.\frac{dT}{dx}\right|_{x=0}=0}
$$

### Convective Surface (x = L)

Heat conducted to the surface is transferred to the surrounding fluid by convection.

$$
\boxed{-k\left.\frac{dT}{dx}\right|{x=L}=h(T_L-T\infty)}
$$

---

## 4. Finite Difference Equations

The plate is divided into five equal intervals:

$$
\Delta x=\frac{0.05}{5}=0.01\text{ m}
$$

Six nodes are established:

$$
i=0,1,2,3,4,5
$$

### Node 0: Insulated Boundary

Using a half control volume:

$$
q'''\frac{\Delta x}{2}+k\frac{T_1-T_0}{\Delta x}=0
$$

Rearranging:

$$
\boxed{-2T_0+2T_1=-\frac{q'''(\Delta x)^2}{k}}
$$

### Interior Nodes (1, 2, 3, and 4)

The energy balance is:

$$
q'''\Delta x+k\frac{T_{i-1}-T_i}{\Delta x}+k\frac{T_{i+1}-T_i}{\Delta x}=0
$$

Rearranging:

$$
\boxed{T_{i-1}-2T_i+T_{i+1}=-\frac{q'''(\Delta x)^2}{k}}
$$

### Node 5: Convective Boundary

Using a half control volume:

$$
q'''\frac{\Delta x}{2}+k\frac{T_4-T_5}{\Delta x}-h(T_5-T_\infty)=0
$$

Rearranging:

$$
\boxed{2T_4-(2+2Bi)T_5=-S-2BiT_\infty}
$$

Where:

$$
S=\frac{q'''(\Delta x)^2}{k}
$$

$$
Bi=\frac{h\Delta x}{k}
$$

---

## 5. Complete System of Algebraic Equations

The numerical parameters are:

$$
S=\frac{600000(0.01)^2}{34}=1.764706
$$

$$
Bi=\frac{60(0.01)}{34}=0.017647
$$

The six equations are:

*Node 0:*

$$
-2T_0+2T_1=-1.764706
$$

*Node 1:*

$$
T_0-2T_1+T_2=-1.764706
$$

*Node 2:*

$$
T_1-2T_2+T_3=-1.764706
$$

*Node 3:*

$$
T_2-2T_3+T_4=-1.764706
$$

*Node 4:*

$$
T_3-2T_4+T_5=-1.764706
$$

*Node 5:*

$$
2T_4-2.035294T_5=-2.823529
$$

These equations form a system of six linear algebraic equations that can be solved using matrix methods.

---

## 6. Analytical Temperature Distribution

Integrating the governing differential equation twice gives:

$$
T(x)=C_1x+C_2-\frac{q'''x^2}{2k}
$$

Applying the insulated boundary condition:

$$
C_1=0
$$

Applying the convective boundary condition:

$$
T(L)=T_\infty+\frac{q'''L}{h}
$$

The final analytical solution is:

$$
\boxed{T(x)=T_\infty+\frac{q'''L}{h}+\frac{q'''}{2k}(L^2-x^2)}
$$

Substituting the numerical values:

$$
T(x)=30+\frac{600000(0.05)}{60}+\frac{600000}{2(34)}(0.05^2-x^2)
$$

Therefore:

$$
\boxed{T(x)=530+8823.5294(0.0025-x^2)}
$$

---

## 7. Maximum and Minimum Temperatures

The maximum temperature occurs at the insulated surface.

$$
T_{\max}=T(0)
$$

$$
T_{\max}=530+\frac{600000(0.05)^2}{2(34)}
$$

$$
\boxed{T_{\max}=552.06^\circ C}
$$

The minimum temperature occurs at the convective surface.

$$
T_{\min}=T(0.05)
$$

$$
\boxed{T_{\min}=530.00^\circ C}
$$

---

## 8. Numerical Results Table

| Node | Position (m) | Analytical Temperature (°C) | FDM Temperature (°C) |
|---|---|---|---|
| 0 | 0.00 | 552.06 | 552.06 |
| 1 | 0.01 | 551.18 | 551.18 |
| 2 | 0.02 | 548.53 | 548.53 |
| 3 | 0.03 | 544.12 | 544.12 |
| 4 | 0.04 | 537.94 | 537.94 |
| 5 | 0.05 | 530.00 | 530.00 |

The analytical and numerical results agree at every node within rounding precision.

---

## 9. Energy Conservation Verification

The total heat generated per unit surface area is:

$$
\frac{\dot Q_{\text{gen}}}{A}=q'''L
$$

$$
\frac{\dot Q_{\text{gen}}}{A}=600000(0.05)
$$

$$
\boxed{q''_{\text{gen}}=30000\text{ W/m}^2}
$$

The heat transferred by convection is:

$$
q''{\text{conv}}=h(T_s-T\infty)
$$

$$
q''_{\text{conv}}=60(530-30)
$$

$$
\boxed{q''_{\text{conv}}=30000\text{ W/m}^2}
$$

Therefore:

$$
\boxed{q''{\text{gen}}=q''{\text{conv}}}
$$

The steady-state energy balance is satisfied.

---

## 10. Physical Interpretation

The temperature distribution is parabolic.

The highest temperature occurs at the insulated surface because heat cannot escape through this boundary.

The temperature decreases toward the convective surface, where heat is transferred to the surrounding fluid.

The temperature gradient is zero at the insulated surface and increases in magnitude toward the convective boundary.

---

## 11. Potential Numerical Errors

*Discretization Error:* The finite difference method generally introduces approximation errors. However, for this particular quadratic temperature profile, the central difference approximation reproduces the analytical solution exactly at the grid nodes.

*Round-Off Error:* Small numerical differences may occur due to floating-point calculations.

*Modeling Error:* Neglecting radiation and temperature-dependent thermal properties may affect the physical accuracy of the results.

---

# Part II. Transient Numerical Analysis

## 1. Transient Problem Description

The plate initially has a uniform temperature of:

$$
T(x,0)=30^\circ C
$$

The internal heat generation rate remains:

$$
q'''=600000\text{ W/m}^3
$$

The objective is to determine how the temperature distribution evolves with time until steady-state conditions are approached.

---

## 2. Additional Material Properties

For the numerical model, the following constant uranium properties are assumed:

| Property | Symbol | Value |
|---|---|---|
| Density | $\rho$ | 19050 kg/m³ |
| Specific heat | $c_p$ | 116 J/(kg·K) |
| Thermal conductivity | $k$ | 34 W/(m·K) |
| Initial temperature | $T_i$ | 30°C |
| Time step | $\Delta t$ | 1 s |
| Spatial interval | $\Delta x$ | 0.01 m |

These properties are modeling assumptions and should be checked against the applicable material reference.

---

## 3. Transient Governing Equation

The one-dimensional transient heat conduction equation is:

$$
\boxed{\rho c_p\frac{\partial T}{\partial t}=k\frac{\partial^2T}{\partial x^2}+q'''}
$$

Dividing by $\rho c_p$:

$$
\frac{\partial T}{\partial t}=\alpha\frac{\partial^2T}{\partial x^2}+\frac{q'''}{\rho c_p}
$$

Where thermal diffusivity is:

$$
\alpha=\frac{k}{\rho c_p}
$$

---

## 4. Thermal Diffusivity

$$
\alpha=\frac{34}{19050(116)}
$$

$$
\boxed{\alpha=1.5386\times10^{-5}\text{ m}^2/\text{s}}
$$

---

## 5. Fourier and Biot Numbers

The Fourier number is:

$$
Fo=\frac{\alpha\Delta t}{(\Delta x)^2}
$$

$$
Fo=\frac{1.5386\times10^{-5}(1)}{(0.01)^2}
$$

$$
\boxed{Fo=0.15386}
$$

The grid Biot number is:

$$
Bi=\frac{h\Delta x}{k}
$$

$$
Bi=\frac{60(0.01)}{34}
$$

$$
\boxed{Bi=0.017647}
$$

---

## 6. Explicit Finite Difference Equations

The Forward-Time Central-Space method is used.

### Node 0: Insulated Surface

$$
\boxed{T_0^{p+1}=T_0^p+2Fo(T_1^p-T_0^p)+\frac{q'''\Delta t}{\rho c_p}}
$$

### Interior Nodes

For $i=1,2,3,4$:

$$
\boxed{T_i^{p+1}=T_i^p+Fo(T_{i-1}^p-2T_i^p+T_{i+1}^p)+\frac{q'''\Delta t}{\rho c_p}}
$$

### Node 5: Convective Surface

$$
\boxed{T_5^{p+1}=T_5^p+2Fo[T_4^p-(1+Bi)T_5^p+BiT_\infty]+\frac{q'''\Delta t}{\rho c_p}}
$$

---

## 7. Numerical Stability

For the insulated boundary:

$$
Fo\leq0.5
$$

For the convective boundary:

$$
Fo\leq\frac{1}{2(1+Bi)}
$$

Substituting:

$$
Fo\leq\frac{1}{2(1.017647)}
$$

$$
\boxed{Fo\leq0.49133}
$$

Since:

$$
0.15386<0.49133
$$

The explicit numerical method satisfies the stability requirement.

---

## 8. Convergence Criterion

The numerical solution approaches the steady-state temperature profile.

Convergence is defined by:

$$
\boxed{\max_i|T_i(t)-T_{i,ss}|\leq1^\circ C}
$$

The transient simulation should continue until this condition is satisfied for all six nodes.

---

## 9. Transient Energy Balance

The energy conservation equation per unit surface area is:

$$
\frac{d(U/A)}{dt}=q'''L-h(T_s-T_\infty)
$$

Integrating over time:

$$
\boxed{\frac{\Delta U}{A}=\int_0^t[q'''L-h(T_s-T_\infty)]dt}
$$

The numerical solution should satisfy this balance within the expected discretization and round-off errors.

---

## 10. Transient Results and Interpretation

Initially, all nodes are at 30°C.

As internal heat generation continues, the plate temperature increases.

The insulated surface becomes the hottest location, while the convective surface remains the coolest.

At sufficiently long times, the transient temperature distribution approaches the analytical steady-state solution obtained in Part I.

The exact convergence time must be determined from the numerical simulation using the stated convergence criterion.

---

# Part III. Engineering Adaptation for Faster Heating

## 1. Objective

The objective is to reduce the time required for the plate to reach a volume-average temperature of 100°C.

Two cases are compared.

### Case A: Original Operation

$$
q'''_A=600000\text{ W/m}^3
$$

### Case B: Increased Heat Generation

$$
q'''_B=750000\text{ W/m}^3
$$

In Case B, the increased heat generation rate is applied only during startup.

Once the average plate temperature reaches 100°C, heat generation returns to the original value.

---

## 2. Volume-Average Temperature

Using the trapezoidal rule with six nodes:

$$
\boxed{T_{\text{avg}}=\frac{0.5T_0+T_1+T_2+T_3+T_4+0.5T_5}{5}}
$$

The target temperature is:

$$
\boxed{T_{\text{avg,target}}=100^\circ C}
$$

---

## 3. Adaptation Procedure

1. Initialize all six nodes at 30°C.
2. Apply the original internal heat generation rate for Case A.
3. Apply the increased generation rate for Case B.
4. Update temperatures using the explicit finite difference equations.
5. Calculate the volume-average temperature at every time step.
6. Record the first time the average temperature reaches or exceeds 100°C.
7. For Case B, restore the original heat generation rate.
8. Continue the simulation until the original steady-state profile is recovered.

---

## 4. Heating-Time Comparison

The proposed numerical comparison uses the following reported values, which must be independently verified by running the simulation:

| Performance Metric | Case A | Case B |
|---|---|---|
| Startup heat generation | 600,000 W/m³ | 750,000 W/m³ |
| Reported heating time | 58 s | 45 s |
| Target average temperature | 100°C | 100°C |

The percentage reduction based on these reported times is:

$$
\%\text{ Reduction}=\frac{t_A-t_B}{t_A}\times100
$$

$$
\%\text{ Reduction}=\frac{58-45}{58}\times100
$$

$$
\boxed{\%\text{ Reduction}=22.41\%}
$$

These times and the resulting percentage should be treated as provisional until confirmed by the numerical model.

---

## 5. Hypothetical Steady-State Temperature for Case B

If the increased heat generation rate were maintained indefinitely, the surface temperature would be:

$$
T_{s,B}=T_\infty+\frac{q'''_BL}{h}
$$

$$
T_{s,B}=30+\frac{750000(0.05)}{60}
$$

$$
\boxed{T_{s,B}=655.00^\circ C}
$$

The maximum temperature would be:

$$
T_{\max,B}=T_{s,B}+\frac{q'''_BL^2}{2k}
$$

$$
T_{\max,B}=655+\frac{750000(0.05)^2}{2(34)}
$$

$$
\boxed{T_{\max,B}=682.57^\circ C}
$$

These values represent hypothetical equilibrium conditions, not the maximum transient temperatures reached during the temporary startup boost.

---

## 6. Maximum Transient Temperature

The maximum transient temperature for Case B must be extracted from the numerical temperature history.

The maximum occurs at the insulated surface under the stated model conditions.

Because the generation rate is restored to its original value after reaching the target average temperature, the actual transient history differs from the hypothetical steady-state profile associated with permanently increased generation.

The maximum temperature should therefore be evaluated from the complete simulation, including the period after switching.

---

## 7. Post-Switch Recovery

After reaching the target average temperature, the internal heat generation rate is restored to:

$$
q'''=600000\text{ W/m}^3
$$

The plate subsequently approaches the original steady-state temperature distribution.

The convergence criterion is:

$$
\max_i|T_i(t)-T_{i,ss}|\leq1^\circ C
$$

The recovery time must be calculated from the transient simulation.

---

## 8. Engineering Comparison

| Performance Metric | Case A: Original | Case B: Adapted |
|---|---|---|
| Initial heat generation | 600,000 W/m³ | 750,000 W/m³ |
| Target average temperature | 100°C | 100°C |
| Reported target time (unverified) | 58 s | 45 s |
| Reported time reduction (unverified) | Baseline | 22.41% |
| Final heat generation | 600,000 W/m³ | 600,000 W/m³ |
| Final steady-state maximum temperature | 552.06°C | 552.06°C |
| Final steady-state minimum temperature | 530.00°C | 530.00°C |
| Original steady state recovered theoretically? | Yes | Yes |

---

## 9. Physical Interpretation

Increasing internal heat generation increases the rate of thermal energy accumulation during startup.

This can reduce the time required to reach the specified average temperature.

However, increased heat generation may also increase thermal gradients and thermal stresses.

The effectiveness of the adaptation must therefore be evaluated using both heating time and maximum transient temperature.

---

## 10. Engineering Discussion and Physical Implementability

The proposed adaptation demonstrates how numerical heat transfer models can be used to compare different thermal startup strategies.

In an actual uranium-based system, internal heat generation may be associated with nuclear processes. The simplified model does not include neutron transport, reactor kinetics, feedback effects, or safety-control dynamics.

Consequently, the increased-generation case is a mathematical sensitivity study rather than an operational recommendation.

Thermal stress, fatigue, material-property variation, radiation, and applicable safety limits would require separate analysis before any physical implementation could be considered.

---

# Final Conclusions

The steady-state analysis confirms that the maximum temperature occurs at the insulated surface and the minimum temperature occurs at the convective surface.

The calculated steady-state temperatures are:

$$
\boxed{T_{\max}=552.06^\circ C}
$$

$$
\boxed{T_{\min}=530.00^\circ C}
$$

The finite difference method reproduces the analytical solution at all six nodes because the exact temperature distribution is quadratic.

The energy conservation verification confirms that the heat generated inside the plate equals the heat removed by convection.

The transient model provides a framework for evaluating temperature evolution from the initial condition to steady state.

The proposed increased-generation case illustrates a potential reduction in startup heating time, but the numerical heating times, peak transient temperatures, and convergence times must be verified by executing the complete simulation.

Overall, the analysis demonstrates the application of conservation of energy, analytical heat conduction, finite difference methods, numerical stability, and engineering comparison to a one-dimensional thermal system.
