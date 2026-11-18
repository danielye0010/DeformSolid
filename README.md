# DeformSolid

DeformSolid is a solid modeling course project for cutting and inspecting solids after nonlinear deformation. The demo starts with a four-hole component and compares cutting before deformation, cutting with a sphere fixed in space, and cutting with a tool that moves with the material.

[Interactive 3D demo](https://danielye.net/projects/deform-solid/)

![Original four-hole solid, nonlinear deformation, and spherical Boolean cut](https://danielye.net/projects/deform-solid/figures/workflow.png)

The application maps an analytic CSG solid through a piecewise trilinear (Q1) finite-element deformation. The boundary mesh follows element interfaces so it can represent changes in surface slope. Manifold computes the 3D differences. Damped Newton inversion evaluates the reference solid at deformed points and provides an independent check on the meshes and sections.

## Results

| Test | Result |
| --- | --- |
| Boundary alignment, finest strong-deformation mesh | Maximum sampled surface error fell from 2.07 × 10⁻³ to 1.96 × 10⁻⁵, a 105× reduction with 3.75% more input triangles. |
| Cut first vs. cut with the transported tool | Same continuous target. The fine meshes differed by 6.56 × 10⁻⁵ in sampled surface distance. The transported-tool output had absolute volume error 7.89 × 10⁻⁷. |
| Section inspection | All 18 sections of the cut-order outputs kept four holes. In a separate small-hole case, coarse sampling found three holes and finer sampling recovered the fourth. |
| Independent Boolean classification | 12 meshes, 49,636 point–mesh comparisons. All 4,214 definite disagreements were within about 1.89 × 10⁻⁴ of the mesh surface. |
| Dense field queries, 2048² cells | Single-thread CPU medians were 0.8514 s in native C++ and 6.1203 s in Python (7.19× over five runs). |

[FE alignment data](results/exp09/fe_alignment_convergence.csv) · [CPU benchmark data](results/native/) · [Cross-section comparison](https://danielye.net/projects/deform-solid/figures/cut_order_sections.pdf)

## Run the geometry example

With Python 3.13 and an activated environment, run from the repository root:

```sh
python -m pip install -r requirements.txt
python -B examples/build_geometry.py --output output
```

Use a new output directory. The example saves OBJ and PLY meshes, ordered cross-sections, and a `results.json` summary. The web viewer opens the saved meshes for interactive inspection.

## Repository layout

- `examples/build_geometry.py` is the application entry point. It uses the mesh assembly helpers in `course_project/application_geometry.py`.
- `solid_modeling/` contains the solid, deformation, boundary meshing, and slicing code. `solid-query/` and selected modules under `experiments/` supply reused numerical routines and validation tools.
- `course_project/gpu/e1/` contains the double-precision C++ CPU evaluator and its [batch interface](course_project/gpu/e1/INTERFACE.md).
- `results/` contains archived FE-alignment and CPU measurements. The example regenerates the application meshes and sections. The full Exp09 comparison also requires the original Exp08 configurations and saved meshes, which are outside this distribution.
