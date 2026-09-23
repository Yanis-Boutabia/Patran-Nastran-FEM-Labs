# TP4 — Thermal Finite Element Analysis with Patran/Nastran

Three complementary thermal/thermomechanical finite element exercises: yield onset under restrained thermal expansion, steady-state conduction-convection through a wall (compared across 1D/2D/3D idealizations), and heat transfer through a multilayer furnace wall.

## Exercise 1 — Thermal yield of a restrained hollow cylinder

Hollow aluminum cylinder (D = 0.15 m, L = 0.6 m, thickness 6.25 mm), clamped at one end, subjected to a uniform temperature rise ΔT. Since axial expansion is fully restrained, all thermal strain converts to stress: σ = E·α·ΔT.

- Material: aluminum — E = 70 GPa, α = 13×10⁻⁶ °C⁻¹, R₀.₂ = 85 MPa
- Analytical critical threshold: **ΔTmax ≈ 93°C**

| Case | FE max Von Mises stress | vs R₀.₂ = 85 MPa | Result |
|---|---|---|---|
| ΔT = 85°C (< 93°C) | 82.5 MPa | below | Elastic ✓ |
| ΔT = 100°C (> 93°C) | 97 MPa | above | Onset of yielding |

The FE results correctly bracket the analytical threshold, validating the model's ability to capture the elastic-to-plastic transition under restrained thermal expansion.

## Exercise 2 — Conduction-convection wall: 1D vs 2D vs 3D

Steady-state heat transfer through a wall (λ = 1.2 W/m·°C, L = 0.15 m), one face fixed at T₁ = 350°C, the other exposed to convection (h = 20 W/m²·°C, T∞ = 25°C).

Analytical: **T₂ = 117.9°C**

| Model | Mesh | FE result | Error vs analytical | Real time |
|---|---|---|---|---|
| 3D (Hex8) | 8228 nodes / 6700 elements | 118°C | 0.085% | 4.76 s |
| 2D | — | 118°C | ~equal | 3.69 s |
| 1D | 11 nodes / 10 elements | 118°C | ~equal | 1.47 s |

All three idealizations converge to essentially the same result, because the problem is physically one-dimensional (uniform boundary conditions on parallel faces, homogeneous material). **The 1D model is the most efficient choice** here — same accuracy, ~3× faster than 3D — though 2D/3D remain useful to visually confirm the absence of multidimensional effects.

## Exercise 3 — Multilayer furnace wall

Three-layer wall: refractory alumina (e₁ = 0.15 m, λ₁ = 1.62) → insulating kaolin (e₂ = ?, λ₂ = 0.23) → ordinary bricks (e₃ = 0.225 m, λ₃ = 1.39). Known temperatures: Ti = 982°C, T₁ = 938°C, T₂ = 138°C.

**Part 1 — Solving for the unknown kaolin thickness** (equal heat flux through each layer): e₂ ≈ 0.387 m

**Part 2 — Exterior surface temperature**, compared with and without convective exchange (T_air = 25°C, h = 20 W/m²·°C):

| Configuration | Analytical | FE | Error |
|---|---|---|---|
| No convection (Te fixed by conduction only) | 61.1°C | 61°C | 0.16% |
| With convection | 51.7°C | 51.7°C | 0% |

Adding convection lowers the exterior surface temperature (extra thermal resistance pulling heat away), and the kaolin layer — lowest conductivity — accounts for the largest temperature drop, confirming it as the wall's main thermal barrier.

## Summary — analytical vs FE across all exercises

| Exercise | Case | Analytical | FEA | Error |
|---|---|---|---|---|
| Ex1.1 | ΔT = 85°C | σ < 85 MPa | 82.5 MPa → Elastic | — |
| Ex1.2 | ΔT = 100°C | σ > 85 MPa | 97 MPa → Plastic | — |
| Ex2 | T₂ | 117.9°C | 118°C | 0.085% |
| Ex3 | Te (no convection) | 61.1°C | 61°C | 0.16% |
| Ex3 | Te (with convection) | 51.7°C | 51.7°C | 0% |

## Key takeaways

- Restrained thermal expansion converts directly to stress (σ = E·α·ΔT) — a purely geometric/material effect independent of load history.
- When a problem is physically one-dimensional, a 1D model gives the same accuracy as 2D/3D at a fraction of the computational cost — added dimensionality only pays off when boundary conditions or geometry genuinely vary in those directions.
- In a multilayer system, the lowest-conductivity layer dominates the total thermal resistance and the temperature drop.

## Tools

`MSC Patran` (modeling, meshing) · `MSC Nastran` (linear static & thermal solver)

---
*Academic project (Mé422) — IPSA Paris, 4PA2, 27/03/2026.*
