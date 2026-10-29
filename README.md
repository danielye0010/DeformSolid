# DeformSolid

**3D Boolean editing and independent geometric validation under nonlinear deformation.**

[Interactive 3D project](https://danielye.net/projects/deform-solid/) ·
[Published project code in the portfolio repository](https://github.com/danielye0010/danielye0010.github.io/tree/master/_code/deform-solid)

A reference solid is represented by analytic CSG, mapped through a continuous
piecewise-Q1 deformation, and edited in deformed coordinates. The project
combines FE-aligned boundary construction, Manifold-based 3D Boolean operations,
implicit queries with Newton inversion, cross-section extraction, and independent
geometric comparisons. Its goal is reliable editing and inspection rather than
a new Boolean kernel.

## Main result: why FE alignment matters

Deformation gradients can jump at finite-element interfaces. Mapping a mesh
edge across one of these interfaces without splitting it can miss a geometric
kink and introduce an error that carries into the Boolean result.

| Grid cells per axis | Unsplit max sampled error | FE-aligned max sampled error |
| ---: | ---: | ---: |
| 7 | 2.897610e-2 | 4.028896e-3 |
| 21 | 1.013866e-2 | 4.105835e-4 |
| 49 | 4.403282e-3 | 8.978049e-5 |
| 105 | 2.065682e-3 | 1.958339e-5 |

At the finest resolution, FE-aligned construction reduced sampled surface
error **105.48×**, while increasing the input triangle count by **3.75%**.
Observed convergence orders were **0.975** without alignment and **1.954**
with alignment for the tested geometry and deformation.

The Exp09 validation compared **12 saved 3D Boolean meshes** with independent
inverse-implicit geometry across **49,636** point–mesh comparisons. All
**4,214** definite label differences were within **1.892468e-4** of the
output mesh, and **120** representative independent checks passed. Four further
Boolean outputs passed mesh checks and retained the four-hole sections.

The standalone matched-input splitter also had six retained numerical seam
failures with approximately 1e-13 gaps. The supported approach is the complete
FE-aligned construction. These measurements are finite sampled results, not a
general continuous Hausdorff or topology certificate.

![Measured FE-aligned convergence](https://danielye.net/projects/deform-solid/figures/fe_alignment_convergence.svg)

[Download the Exp09 convergence CSV](results/exp09/fe_alignment_convergence.csv)

## Code and reproduction

This repository includes the existing executable modeling source released
through the portfolio site's `_code/deform-solid` directory. The Exp09
experiment's standalone scripts have **not** been included in this public
snapshot. The published Exp09 numerical summary is reproduced here and on
the live project page.

```sh
python -m venv .venv
# Activate the environment first.
python -m pip install -r requirements.txt
python -B examples/build_geometry.py --output output
```

The example exports deformed and Boolean mesh geometry, ordered cross sections,
and measured results. Use a new output directory. The modeling release includes
a native C++ batch evaluator; see `course_project/gpu/e1/INTERFACE.md` and
`course_project/gpu/e1/CMakeLists.txt`.

### Repository layout

- `solid_modeling/` — reference CSG, FE deformation, boundary meshing, cross-sections
- `experiments/` — selected established inverse/Boolean implementation modules
- `solid-query/` — inverse-field and reference dependencies
- `course_project/gpu/e1/` — native C++ CPU evaluator
- `examples/` — executable geometry example
- `results/` — existing native measurements and Exp09 published summary

The published Boolean computations use Manifold, an established mesh-Boolean
backend. The page's live WebGL model viewer uses saved meshes; it does not run
the Python solver in the browser.
