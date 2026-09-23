# TP1 — Finite Element Modeling with Nastran: Beam Under Axial & Bending Loads

Linear static finite element analysis of a hollow steel beam, using Patran for modeling and Nastran for solving, with results validated against classical strength-of-materials theory.

## Structure

Hollow rectangular steel beam, embedded (clamped) at one end:
- Length L = 2 m
- Cross-section: 40 mm × 30 mm external, 4 mm wall thickness (496 mm² cross-sectional area)
- Material: steel — ρ = 7800 kg/m³, E = 2.1×10¹¹ Pa, ν = 0.33, σ₀.₂ = 660 MPa, σRm = 849 MPa

## Case 1 — Axial force (bar element)

The beam is modeled as a 1D bar element under a 30,000 daN axial force at the free end.

| Quantity | Analytical | FE (Patran/Nastran) | Δ |
|---|---|---|---|
| Normal stress | 605 MPa | 605 MPa (uniform) | ≈0% |
| Max displacement | 5.76×10⁻³ m | 6.18×10⁻³ m | 7.3% |

- A single bar element is sufficient here — the linear displacement formulation exactly represents pure axial loading, so mesh refinement doesn't change results.
- σ = 605 MPa < σ₀.₂ = 660 MPa → elastic domain, but with a small safety margin. At 35,000 daN, stress would reach 705.6 MPa, exceeding yield — the linear analysis would no longer be valid.

## Case 2 — Combined bending (beam element, 20-element mesh)

Same beam, now modeled with beam elements (20-element mesh, needed to capture the internal force variation along the span), subjected to a vertical point load (1500 N) and its own self-weight, separately and combined.

| Load case | Reaction Rz | Bending moment My | Max bending stress |
|---|---|---|---|
| Point load only | 1500 N | 3000 N·m | 730.5 MPa |
| Self-weight only | 75.91 N | 75.91 N·m | 18.48 MPa |
| Combined | 1575.91 N | 3075.91 N·m | 749 MPa |

The combined case confirms the **principle of superposition**: σ_load+weight = σ_load + σ_weight, both analytically and numerically — matched exactly by the FE solver.

Deflections (Euler-Bernoulli beam theory vs FE):
- Point load only: δ = 0.309 m
- Self-weight only: δ = 5.87×10⁻³ m
- Combined: δ = 0.315 m — small deviations from theory attributed to discretization of the distributed self-weight load.

## Key takeaways

- A bar element is exact for pure axial loading regardless of mesh density; bending requires a finer beam-element mesh to resolve internal force variation.
- Superposition (σ_load+weight = σ_load + σ_weight) holds exactly in the linear elastic domain, validated both analytically and numerically.
- Structural sizing must account for the gap to yield strength (σ₀.₂), not just the ultimate strength (σRm) — the axial case operates close to yield with limited safety margin.

## Tools

`MSC Patran` (modeling) · `MSC Nastran` (linear static solver)

---
*Academic project (Mé422) — IPSA Paris, PA2, 10/02/2026.*
