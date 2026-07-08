# Drell--Yan angular coefficients with generic photon--Z' interference

This repository/notebook contains a numerical study of the Drell--Yan angular coefficients

$$
A_0,\qquad A_1,\qquad A_2,
$$

for proton--proton collisions at

$$
\sqrt{s}=8~\mathrm{TeV},
$$

using ATLAS angular-coefficient data from HEPData and a theoretical prediction based on collinear perturbative QCD.

The main goal of the calculation is to compare the photon-exchange Drell--Yan prediction with a modified prediction including a generic interference term between the photon and a hypothetical neutral gauge boson $Z'$:

$$
|\mathcal M|^2
\simeq
|\mathcal M_\gamma|^2
+
2\,\mathrm{Re}\left(\mathcal M_\gamma \mathcal M_{Z'}^\ast\right),
$$

while neglecting the quadratic term

$$
|\mathcal M_{Z'}|^2.
$$

This follows the task requested by the supervisor: extract angular-coefficient data, compare with theory, and include a photon--$Z'$ interference contribution analogous to the standard photon--$Z^0$ interference structure.

---

## 1. Data source

The experimental data used in the plots were obtained from HEPData for the ATLAS measurement:

**ATLAS Collaboration**, *Measurement of the angular coefficients in Z-boson events using electron and muon pairs from data taken at $\sqrt{s}=8$ TeV with the ATLAS detector*, JHEP 08 (2016) 159.

- Journal DOI: `10.1007/JHEP08(2016)159`
- HEPData DOI: `10.17182/hepdata.76986`
- arXiv: `1606.00689`

The relevant HEPData tables contain the measured Drell--Yan angular coefficients $A_i$ as functions of the dilepton transverse momentum $Q_T$ in bins of the dilepton rapidity $|y_Z|$.

In this calculation, the main datasets used are:

| Coefficient | Rapidity bin | Regularisation |
|---|---:|---|
| $A_0$ | $2<|y_Z|<3.5$ | regularised |
| $A_1$ | $1<|y_Z|<2$ | regularised |
| $A_2$ | $2<|y_Z|<3.5$ | regularised |

The notebook reads the HEPData CSV files from the folder defined by

```python
DATA_DIR = Path("/path/to/HEPData/tables")
```

The CSV reader extracts the following information from each file:

- coefficient label, e.g. `A0`, `A1`, `A2`;
- rapidity bin, e.g. `2<|yZ|<3.5`;
- regularisation type, e.g. `regularised` or `unregularised`;
- central value of $Q_T$;
- $Q_T$ bin edges;
- measured $A_i$ value;
- statistical, systematic, and regularisation-bias uncertainties when available.

---

## 2. Theoretical framework

The calculation follows the collinear-factorisation expression for Drell--Yan hadronic structure functions:

$$
W_i(x_1,x_2,\rho^2)
=
\frac{1}{x_1x_2}
\sum_{a,b}
\int dz_1 dz_2\,
\widetilde w_i^{ab}(z_1,z_2,\rho^2)
\delta\!\left(
(1-z_1)(1-z_2)-\frac{\rho^2 z_1z_2}{1+\rho^2}
\right)
 f_{a/H_1}\!\left(\frac{x_1}{z_1},\mu\right)
 f_{b/H_2}\!\left(\frac{x_2}{z_2},\mu\right).
$$

The kinematic variables are

$$
\rho = \frac{Q_T}{Q},
$$

$$
x_1=e^y\frac{\sqrt{Q^2+Q_T^2}}{\sqrt{s}},
\qquad
x_2=e^{-y}\frac{\sqrt{Q^2+Q_T^2}}{\sqrt{s}},
$$

and the factorisation/renormalisation scale is taken as

$$
\mu=\sqrt{Q^2+Q_T^2}.
$$

The notebook evaluates the full numerical convolution after using the phase-space delta function to reduce the two-dimensional integral to a one-dimensional integral.

No small-$Q_T$ expansion is used in this notebook.

---

## 3. Partonic channels included

The calculation includes the leading $\mathcal O(\alpha_s)$ transverse-momentum-dependent Drell--Yan channels:

$$
q\bar q \to \gamma^*/Z' + g,
$$

$$
qg \to \gamma^*/Z' + q,
$$

$$
gq \to \gamma^*/Z' + q.
$$

The QCD kernels are kept unchanged when the $Z'$ interference is added. The new physics enters only through the electroweak coupling factor.

---

## 4. Angular coefficients

The code computes the helicity structure functions

$$
W_T,\qquad W_L,\qquad W_\Delta,\qquad W_{\Delta\Delta},
$$

and constructs

$$
A_0 = \frac{2W_L}{2W_T+W_L},
$$

$$
A_1 = \frac{2W_\Delta}{2W_T+W_L},
$$

$$
A_2 = \frac{4W_{\Delta\Delta}}{2W_T+W_L}.
$$

These are the coefficients compared with the ATLAS/HEPData measurements.

---

## 5. Photon--Z' interference implementation

The original photon-only calculation has an electroweak factor schematically written as

$$
g_{\mathrm{EW}}^\gamma = 1.
$$

The generic $Z'$ interference is included by replacing this factor with

$$
g_{\mathrm{EW}}^{\gamma+\gamma Z'}(q,Q)
=
1+
2g'_{q}g'_{\ell}\,\mathrm{Re}\,D_{Z'}(Q^2),
$$

where $g'_q$ is the vector coupling of the $Z'$ to a quark flavour, $g'_\ell$ is the vector coupling to charged leptons, and

$$
\mathrm{Re}\,D_{Z'}(Q^2)
=
\frac{(M_{Z'}^2-Q^2)Q^2}
{(M_{Z'}^2-Q^2)^2+M_{Z'}^2\Gamma_{Z'}^2}.
$$

The benchmark parameters are defined in the notebook as

```python
ZPRIME_BENCHMARKS = {
    "custom": {
        "M": 500.0,
        "Gamma": 20.0,
        "gV_l": 0.20,
        "gV_u": 0.30,
        "gV_d": -0.10,
    }
}
```

Important: these benchmark values are not taken from the ATLAS data or from the paper. They are user-defined test points. They can be replaced by a specific $Z'$ model or scanned over.

---

## 6. Universal vs non-universal benchmarks

A benchmark is called **universal** when the $Z'$ couples equally to up-type and down-type quarks, for example

$$
g'_u=g'_d.
$$

In that case, the $Z'$ interference often behaves approximately as a common rescaling of all helicity structure functions:

$$
W_i\to C W_i.
$$

Since the angular coefficients are ratios, this common factor cancels almost exactly:

$$
A_0
\to
\frac{2CW_L}{2CW_T+CW_L}
=
A_0.
$$

A **non-universal** benchmark has

$$
g'_u\neq g'_d,
$$

which changes the relative weight of different quark flavours. Since the proton PDFs for $u$ and $d$ quarks are different, this can generate a small angular deformation.

---

## 7. Output plots

For each selected $Z'$ benchmark, the notebook produces two kinds of plots.

### 7.1 Direct comparison plots

These show

$$
A_i^{\mathrm{data}},
\qquad
A_i^\gamma,
\qquad
A_i^{\gamma+\gamma Z'}.
$$

The corresponding files are named as

```text
A012_comparison_<benchmark_name>.png
```

### 7.2 Isolated deformation plots

These show the shift induced by the $Z'$ interference:

$$
\Delta A_i
=
A_i^{\gamma+\gamma Z'}-A_i^\gamma.
$$

The corresponding files are named as

```text
A012_delta_<benchmark_name>.png
```

These plots are often more informative than the direct comparison, because the direct photon-only and photon--$Z'$ curves can be visually indistinguishable when $|\Delta A_i|$ is very small.

---

## 8. Numerical diagnostics

The notebook writes two diagnostic CSV files:

```text
individual_Zprime_shift_diagnostics.csv
individual_Zprime_chi2_diagnostics.csv
```

The shift table reports, for each benchmark and coefficient,

$$
\max |\Delta A_i|,
\qquad
\langle |\Delta A_i|\rangle.
$$

The chi-square table reports a simple diagnostic comparison with data:

$$
\chi^2
=
\sum_k
\frac{\left(A_i^{\mathrm{th}}(Q_{T,k})-A_i^{\mathrm{exp}}(Q_{T,k})\right)^2}
{\sigma_{i,k}^2}.
$$

This is only a first numerical diagnostic. It is not yet a full statistical exclusion or fit.

---

## 9. Main interpretation

The main physical result of the benchmark study is that the $Z'$ interference modifies the angular coefficients mostly through a flavour-dependent electroweak reweighting.

For universal couplings, the effect nearly cancels in the normalised angular coefficients. For non-universal couplings, a small nonzero deformation appears, especially in $A_0$ and $A_2$.

The relevant observable for visualising the new contribution is therefore

$$
\Delta A_i
=
A_i^{\gamma+\gamma Z'}-A_i^\gamma,
$$

rather than only the direct comparison between the two curves.

---

## 10. Software requirements

The notebook requires:

- Python 3;
- `numpy`;
- `pandas`;
- `matplotlib`;
- `scipy`;
- `LHAPDF` with the chosen PDF set installed.

The PDF set is defined in the configuration cell, for example

```python
PDF_SET = "cteq61"
```

or another installed LHAPDF set.

---

## 11. References

1. ATLAS Collaboration, *Measurement of the angular coefficients in Z-boson events using electron and muon pairs from data taken at $\sqrt{s}=8$ TeV with the ATLAS detector*, JHEP 08 (2016) 159. DOI: `10.1007/JHEP08(2016)159`. HEPData DOI: `10.17182/hepdata.76986`. arXiv: `1606.00689`.

2. V. E. Lyubovitskij, A. S. Zhevlakov, and I. A. Anikin, *Transverse momentum dependence of the T-even hadronic structure functions in the Drell-Yan process*, Phys. Rev. D 110, 074028 (2024). DOI: `10.1103/PhysRevD.110.074028`.

3. V. E. Lyubovitskij, W. Vogelsang, F. Wunder, and A. S. Zhevlakov, *Perturbative T-odd asymmetries in the Drell-Yan process revisited*, Phys. Rev. D 109, 114023 (2024). DOI: `10.1103/PhysRevD.109.114023`.

4. A. Buckley et al., *LHAPDF6: parton density access in the LHC precision era*, Eur. Phys. J. C 75, 132 (2015). DOI: `10.1140/epjc/s10052-015-3318-8`.

---

## 12. Notes

- The $Z'$ benchmarks are illustrative unless a specific model is supplied.
- The calculation includes only the interference term with the photon and neglects $|\mathcal M_{Z'}|^2$.
- The current implementation focuses on $A_0$, $A_1$, and $A_2$.
- Extending to $A_3$ and $A_4$ would require adding the corresponding helicity structure functions.
- Extending to $A_5$, $A_6$, and $A_7$ would require the T-odd structure functions, which arise from absorptive loop contributions.
