# Exp09 — FE alignment and independent Boolean geometry checks

The existing piecewise-Q1 FE-aligned tessellation was compared with reconstruction
that did not respect FE interfaces. At the finest setting the maximum sampled
distance dropped from 2.065682e-3 to 1.958339e-5 (105.48×); observed rates
were 0.975 (unsplit) and 1.954 (aligned).

Twelve saved 3D Boolean meshes were compared to the independent inverse-mapped
implicit solid at 49,636 points. All 4,214 definite differences were within
1.892468e-4 of the mesh. Checks on 120 representative points passed. Four
new Boolean outputs passed mesh checks and preserved four-hole sections.

Six matched-input standalone splitter cases had numerical seam failures with
gaps near 1e-13. The overall FE-aligned construction remains the supported
approach; no universal continuous error or topology certificate is claimed.

This release contains the prior public executable geometry source and these
measured Exp09 results, **not** the separate Exp09 runner scripts.
