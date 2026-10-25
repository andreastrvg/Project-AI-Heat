Act as an entry-level thermal engineer specializing in numerical heat transfer, heat conduction with internal heat generation, and finite difference methods.

Solve Problem 5-37 from the heat conduction problem bank.

## Problem Description:

A large uranium plate has a thickness of 5 cm and a thermal conductivity of 34 W/(m·K). Heat is generated uniformly inside the plate at a constant rate of \(6\times10^5\) W/m³. One surface of the plate is perfectly insulated, while the opposite surface is exposed to convection with an ambient temperature of 30°C and a heat transfer coefficient of 60 W/(m²·K).

Divide the plate into five equal intervals of 1 cm, resulting in six nodes.

## Engineering Tasks:

### Part I. Original Steady-State Problem

1. Identify all physical assumptions.
2. Derive the governing one-dimensional steady-state heat conduction equation with internal heat generation.
3. Define the insulated and convective boundary conditions.
4. Derive the finite difference equations for all six nodes using energy balances.
5. Construct the complete system of six linear algebraic equations.
6. Solve for the temperature at each node.
7. Identify the maximum and minimum temperatures.
8. Verify the numerical results using the analytical steady-state solution.
9. Verify energy conservation and units.
10. Explain the physical meaning of the temperature distribution.
11. Identify possible numerical or mathematical errors.
12. Present all results in a clear table.

### Part II. Transient Numerical Analysis of the Original Problem

1. Extend the original problem from steady-state to transient heat conduction using the finite difference method.
2. Identify the additional data required for transient analysis, including uranium density \(\rho\), specific heat \(c_p\), initial temperature distribution \(T(x,0)\), time-step size \(\Delta t\), and simulation duration.
3. Do not invent missing material properties. Verify the previously used values \(\rho=19{,}050\) kg/m³ and \(c_p=116\) J/(kg·K) using reliable references, or clearly retain them as provisional assumptions.
4. Since the original problem does not specify an initial temperature, explicitly assume a uniform initial temperature of \(30^\circ C\).
5. Derive the transient heat conduction equation with uniform internal heat generation. Assume constant thermal properties unless otherwise justified.
6. Derive the explicit finite difference equations for the interior nodes and both boundary nodes using consistent control-volume energy balances.
7. Calculate thermal diffusivity, Fourier number, and Biot number. Select a stable time step and verify both interior-node and convective-boundary stability restrictions.
8. Numerically calculate the temperatures of all six nodes at successive time steps using the finite difference equations.
9. Use the original analytical steady-state temperature profile as the reference for convergence. Define convergence as the maximum absolute difference between all six transient nodal temperatures and their corresponding steady-state values being less than or equal to \(1.0^\circ C\).
10. Report the time required for the plate to satisfy this convergence criterion and present temperature-versus-time tables for selected nodes.
11. Verify numerically that the transient solution approaches the original steady-state profile.
12. Perform a transient energy balance comparing stored energy with internal heat generation minus convective heat loss.

### Part III. Engineering Adaptation for Faster Heating

Investigate whether temporarily increasing the internal heat-generation rate can reduce the heating time while preserving the original long-term steady-state condition.

1. Increase the heat-generation rate from \(600{,}000\) W/m³ to \(750{,}000\) W/m³ during startup. Keep the geometry, thermal properties, ambient temperature, convection coefficient, and boundary conditions unchanged.

2. Use a volume-average plate temperature of \(100^\circ C\) as the switching target. Clearly state that this is an added comparison benchmark and is not specified in the original textbook problem. Calculate the volume-average temperature using:

\[
T_{\mathrm{avg}}=
\frac{0.5T_0+T_1+T_2+T_3+T_4+0.5T_5}{5}.
\]

3. Use the finite difference method from Part II to numerically solve both cases:
   - **Case A — Original:** Maintain \(q'''=600{,}000\) W/m³ throughout.
   - **Case B — Adapted:** Use \(q'''=750{,}000\) W/m³ until the volume-average temperature first reaches \(100^\circ C\), then restore \(q'''=600{,}000\) W/m³.

   Use the same initial conditions, spatial grid, time step, and finite difference equations in both cases. Update all six nodal temperatures at every time step, using the current heat-generation rate in the source term.

4. Report the time required for each case to reach the target, all six nodal temperatures near the target-crossing time, and the volume-average temperature. Show several initial iterations and the iterations immediately before and after the target is reached. Use interpolation between time steps when appropriate.

5. Calculate the percentage reduction in heating time:

\[
\%\text{ reduction}=
\frac{t_{\mathrm{original}}-t_{\mathrm{adapted}}}
{t_{\mathrm{original}}}\times100\%.
\]

6. Report the maximum transient temperature reached during startup. Also calculate the hypothetical steady-state temperature profile if the boosted heat-generation rate were maintained indefinitely. Clearly distinguish this hypothetical equilibrium from the actual transient temperatures reached before switching.

7. After restoring the original heat-generation rate, continue the numerical simulation until the nodal temperatures approach the original steady-state profile within the \(1.0^\circ C\) maximum nodal error criterion defined in Part II. Report the recovery time and verify that the original steady state is recovered.

8. Verify energy conservation throughout both simulations, including the source-switching event. Compare the change in stored energy per unit area with the time-integrated net heat input:

\[
\frac{\Delta U}{A}=
\int_0^t
\left[q'''(t)L-h(T_s-T_\infty)\right]dt.
\]

Use a consistent numerical integration method and report the energy-balance residual.

9. Present a comparison table containing heating time, percentage improvement, maximum transient temperature, original steady-state temperatures, energy-balance residual, and whether the original steady state is recovered.

10. Discuss whether the proposed adaptation is physically implementable. Explain that the adjustable heat-generation rate is a mathematical assumption unless a controllable physical heat-generation mechanism can be identified.

## Engineering Requirements

- Use SI units.
- Show all essential equations and intermediate calculations.
- Explain the physical principles behind each step.
- Do not assume numerical results are correct without verification.
- Clearly distinguish steady-state and transient heat transfer.
- Parts II and III must include actual numerical solutions using the finite difference method, not just equations or conceptual explanations.
- Do not calculate heating or cooling time using steady-state equations.
- Use reproducible Python code or a spreadsheet-ready algorithm.
- Clearly identify assumptions, provisional properties, numerical errors, and corrected results.
- Do not invent numerical results or claim to have executed code that was not actually run.

## Expected Output

A complete engineering report covering the original steady-state problem, transient heating, and the proposed adaptation, including assumptions, governing equations, boundary conditions, finite difference equations, numerical results, analytical verification, energy balance, comparison tables, reproducible calculations, and engineering conclusions.
