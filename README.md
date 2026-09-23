# Patran/Nastran — Finite Element Analysis Labs

A series of finite element analysis practical works (TP) using **MSC Patran** (pre/post-processing) and **MSC Nastran** (solver), covering static structural analysis, stress concentration, mesh convergence, and thermal analysis. Completed as part of the Mé422 course at IPSA Paris (PA2/4PA2).

**Authors:** Yanis Boutabia, with Noa Bourtal-Zarat (and Dan Ghrenassia on TP1/TP2)

## Series overview

| TP | Topic | Key result |
|---|---|---|
| [TP1 — Beam: Axial & Bending](./TP1-Beam-Axial-Bending) | Linear static analysis of a hollow steel beam under axial load and combined bending/self-weight | FE results within 7.3% of analytical (axial), exact match under superposition (bending) |
| [TP2 — Perforated Plate](./TP2-Perforated-Plate) | Stress concentration around a circular hole in a thin plate, mesh convergence study (12→50 elements), symmetry reduction | Numerical max stress converges toward Peterson's analytical value (157 MPa) as mesh refines |
| [TP3 — Clevis: Element Type Comparison](./TP3-Clevis-Element-Comparison) | Comparing HEX8/HEX20/TET4/TET10 3D elements on a mechanical clevis under bending and pin-hole pressure loading | HEX20 most accurate reference; TET4 largest error (up to 7%); accuracy ranks HEX20 > TET10 > HEX8 > TET4 |
| [TP4 — Thermal Analysis](./TP4-Thermal-Analysis) | Thermal stress in a constrained cylinder, 1D/2D/3D conduction-convection through a wall, multilayer furnace wall | FE vs analytical error consistently <0.2% across all three exercises |

## Progression

The series builds up finite element competency step by step:
- **TP1** introduces the basics — bar/beam elements, boundary conditions, linear static analysis, validation against strength-of-materials theory.
- **TP2** introduces 2D shell elements and geometric stress concentrations, plus a systematic mesh convergence study and the use of symmetry to reduce model size (and computation time by ~4×).
- **TP3** moves to full 3D solid elements, comparing linear vs. quadratic and hexahedral vs. tetrahedral formulations (HEX8/HEX20/TET4/TET10) on a real mechanical component (a clevis), isolating the effect of element order from mesh density.
- **TP4** extends the method to thermal and thermomechanical problems, comparing 1D, 2D and 3D idealizations of the same physical problem and their respective computational cost.

## Tools

`MSC Patran` (geometry, meshing, boundary conditions, post-processing) · `MSC Nastran` (linear static & thermal solver)

## Common methodology

Each TP follows the same validation approach: an analytical solution is derived first (strength of materials / heat transfer theory), then a finite element model is built in Patran and solved with Nastran, and the numerical results are compared against the analytical prediction to assess model accuracy — a standard practice in aerospace structural analysis.

---
*Academic coursework — IPSA Paris, Mé422.*
