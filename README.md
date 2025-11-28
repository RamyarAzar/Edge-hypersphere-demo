# Dynamic hyperspherical spacetime: metric, curvature, and stability on a 400³ grid

**Author:** Ramyar  Azar
**Last updated:** November 26, 2025  

This repository contains a working numerical implementation of a **dynamically evolving four-dimensional hyperspherical spacetime**, including metric construction, curvature extraction, and stability analysis on a high-resolution 400³ grid.  

The code here is the same core used in the corresponding case study on my modeling website and serves both as a **demonstration of methods** and as a **reproducible numerical experiment**.

---

## Snapshot

### Field
- Theoretical physics  
- Emergent geometry & quantum foundations  

### Model type
- Embedded 4D hypersphere + induced metric  
- Christoffel symbols (Levi-Civita connection)  
- Ricci tensor and extrinsic curvature (numerical-relativity style)

### Stage
- Advanced prototype  
- Full simulation using 80+ Python modules and archived tensor outputs  

### Main question
Can a purely geometric, time-evolving four-dimensional hyperspherical spacetime—defined only by a radial field and its embedding in a curved ambient space—remain dynamically stable?

More concretely:

> If we construct the induced metric, Christoffel symbols, and Ricci tensor on a 400×400×400 spatial grid over 50 time steps, do curvature components stay bounded and well-behaved, or do they develop divergences that would make the emergent geometry physically implausible?

---

## Model & approach

We work with a four-dimensional **orbital hypersphere** with coordinates  
\((\chi, \theta, \phi, t)\), embedded in a mildly curved higher-dimensional ambient space.

- The embedding is defined by a position vector  
  \[
  X^A(x^\mu),
  \]
  where a single time- and angle-dependent radial function \(R(\chi,\theta,t)\) controls the geometry of the hypersurface.
- \(R(\chi,\theta,t)\) includes angular harmonics and Gaussian modulation, generating interacting **modes** on the hypersphere.

From this embedding:

1. We build the **induced metric** \(g_{\mu\nu}\) on the hypersurface by pulling back the ambient metric.
2. From \(g_{\mu\nu}\) we compute **Levi-Civita connection coefficients** (Christoffel symbols).
3. Using the connection, we evaluate the **Ricci tensor** as a measure of intrinsic curvature.
4. We compute **extrinsic curvature** for the embedded hypersurface (numerical-relativity style).

---

## Numerical setup

All nontrivial quantities are evaluated on a regular numerical grid:

- **Spatial grid:** \(400 \times 400 \times 400\) points  
- **Metric:** full \(4 \times 4\) tensor stored at each grid point  
- **Temporal evolution:** 50 time steps  
- **Resolutions:** fixed \(\Delta \chi, \Delta \theta, \Delta \phi, \Delta t\)

Spatial and temporal derivatives are implemented via **high-order finite-difference stencils**.  

The full pipeline:

> embedding → induced metric → Christoffel symbols → Ricci tensor → extrinsic curvature

is implemented as a modular Python codebase.

---

## Repository contents


```text
README.md                 # This file

src/
  alm.py            # Defines X^A(x^μ) and radial field R(χ,θ,t)
  metric_radius.py         # Constructs induced metric g_{μν}
  dx_christoffel.py          # Computes Levi-Civita connection
  ricci_r.py                # Computes Ricci tensor components
  k_tensor.py             # Computes extrinsic curvature
  stability_analysis.py   # Time series of mean Ricci components
  run_full_pipeline.py    # Orchestrates full 400³ × 50-step run

config/
  params.yaml             # Grid sizes, resolutions, model parameters

data/
  metric_output/          # Saved metric arrays (NumPy)
  christoffel_output/     # Saved connection coefficients
  ricci_output/           # Saved curvature tensors
  k_output/               # Extrinsic curvature and related tensors

plots/
  ricci_time_series.png   # Example diagnostic plots
  curvature_histograms.png
```
## How to run

**This is a heavy simulation** (400³ grid × 50 steps).  
For testing, reduce the grid size and/or number of time steps in `config/params.yaml`.

### 1. Set up environment

Create and activate a Python environment (e.g., using `venv` or `conda`), then install dependencies:

```bash
pip install -r requirements.txt
```
### 2. (Optional) Adjust parameters

Edit `config/params.yaml` to change:

- grid size (e.g., use 64³ for testing),
- number of time steps,
- radial field parameters.

### 3. Run full pipeline

```bash
python src/run_full_pipeline.py
```
Results will be written to `data/` and diagnostic plots to `plots/`.

---

## Relation to the EDGE framework

This numerical engine forms the geometric backbone of the broader EDGE model.

- It validates that a dynamically evolving, embedded 4D hypersphere can sustain bounded curvature dynamics under the chosen radial field and embedding.
- Later structures in EDGE (fields, gauge dynamics, quantum-like behavior) are built on top of this geometric core.

For additional context and case studies, see the corresponding pages on the modeling website.

---

## Code archive & publications

The full simulation assets (including large tensor arrays) are archived on Zenodo under a restricted license:

- **Simulation archive (Zenodo):** DOI `10.5281/zenodo.15873778`

The main EDGE manuscript is currently under preparation;  
a preprint or journal link will be added here when available.

---

## License & usage

This repository is provided as a demonstration and case study.

- Use is limited to personal, academic, and non-commercial purposes.
- Do not integrate this code into clinical, diagnostic, or commercial products.
- Redistribution of modified or unmodified versions is not permitted without explicit permission.

For collaboration or extended access, please contact the author.
