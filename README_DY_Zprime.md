
# Drell--Yan Angular Coefficients with Photon--Z' Interference

This repository contains a numerical study of the Drell--Yan angular coefficients
`A0`, `A1`, and `A2` using collinear QCD factorization. The calculation compares
theoretical predictions with ATLAS data from HEPData and includes a generic
photon--Z' interference contribution.

The goal is not to introduce a new QCD channel. The QCD kernels and PDF
convolution are kept unchanged. The new physics enters only through the
electroweak factor multiplying the partonic kernels.

---

## 1. Physics goal

The Drell--Yan process considered here is

```math
p p \to \gamma^*/Z^0/Z' + X \to \ell^+\ell^- + X .
```

The angular distribution of the final-state leptons can be written in terms of
angular coefficients `A_i`. In this project we focus on

```math
A_0, \qquad A_1, \qquad A_2 .
```

These coefficients are obtained from the helicity structure functions
`W_T`, `W_L`, `W_Delta`, and `W_DeltaDelta` as

```math
A_0 = \frac{2 W_L}{2 W_T + W_L},
```

```math
A_1 = \frac{2 W_{\Delta}}{2 W_T + W_L},
```

```math
A_2 = \frac{4 W_{\Delta\Delta}}{2 W_T + W_L}.
```

The numerical calculation evaluates the full convolution formula, without using
the small-QT expansion.

---

## 2. Factorized structure

The hadronic structure functions are computed schematically as

```math
W_i = \mathrm{PDFs} \otimes \widehat{w}_i^{ab} \times g_{\mathrm{EW}}.
```

Here:

- `PDFs` are the parton distribution functions evaluated with LHAPDF.
- `w_hat_i^{ab}` are the perturbative QCD kernels for the partonic channels.
- `g_EW` is the electroweak factor.

The included partonic channels are

```math
q\bar q \to \gamma^*/Z' + g,
```

```math
qg \to \gamma^*/Z' + q,
```

```math
gq \to \gamma^*/Z' + q.
```

The QCD part of the calculation is the same for the photon-only and
photon--Z' cases. The only modification is the electroweak weight.

---

## 3. Photon--Z' interference

The total amplitude is approximated as

```math
\mathcal{M}_{\mathrm{tot}} = \mathcal{M}_{\gamma} + \mathcal{M}_{Z'}.
```

Following the requested approximation, the squared amplitude is kept only up to
interference level:

```math
|\mathcal{M}_{\mathrm{tot}}|^2
\simeq
|\mathcal{M}_{\gamma}|^2
+
2\,\mathrm{Re}\left(\mathcal{M}_{\gamma}\mathcal{M}_{Z'}^*\right).
```

The pure diagonal Z' contribution is neglected:

```math
|\mathcal{M}_{Z'}|^2 \quad \text{is not included.}
```

In the code, this is implemented as a modified electroweak factor:

```math
g_{\mathrm{EW}}^{\gamma+\gamma Z'}
=
1
+
2 g'_q g'_\ell\,\mathrm{Re}\,D_{Z'}(Q^2).
```

The real part of the Breit--Wigner propagator is

```math
\mathrm{Re}\,D_{Z'}(Q^2)
=
\frac{(M_{Z'}^2-Q^2)Q^2}
{(M_{Z'}^2-Q^2)^2 + M_{Z'}^2\Gamma_{Z'}^2}.
```

The free Z' parameters are

```math
M_{Z'}, \qquad \Gamma_{Z'}, \qquad g'_u, \qquad g'_d, \qquad g'_\ell .
```

Since no specific Z' model was assumed, these parameters are treated as
benchmarks that can be modified in the notebook.

---

## 4. Universal and non-universal benchmarks

A universal benchmark means that the Z' couples in the same way to up-type and
down-type quarks, for example

```math
g'_u = g'_d .
```

In that case, the Z' interference often behaves approximately as a common
rescaling of all helicity structure functions:

```math
W_i \to C W_i .
```

Since the angular coefficients are ratios, this common factor cancels almost
exactly. For example,

```math
A_0
\to
\frac{2 C W_L}{2 C W_T + C W_L}
=
\frac{2 W_L}{2 W_T + W_L}
=
A_0 .
```

A non-universal benchmark has

```math
g'_u \neq g'_d .
```

This changes the relative weight of different quark flavours. Since the proton
PDFs for `u` and `d` quarks are different, a non-universal benchmark can generate
a small angular deformation.

---

## 5. Data source

The experimental data are taken from HEPData:

- Dataset: ATLAS Drell--Yan angular coefficients `A0`--`A7`.
- Collision energy: 8 TeV.
- Final state: lepton-pair production near the Z-boson invariant-mass window.
- HEPData DOI: `10.17182/hepdata.76986`.
- Publication: ATLAS Collaboration, *Measurement of the angular coefficients in
  Z-boson events using electron and muon pairs from data taken at sqrt(s) = 8 TeV
  with the ATLAS detector*, JHEP 08 (2016) 159.

The notebook reads the regularised ATLAS data points for selected rapidity bins
and compares them with the theoretical curves.

---

## 6. Output plots

For each selected Z' benchmark, the notebook produces two kinds of plots.

### 6.1 Direct comparison plots

These plots show

```math
A_i^{\mathrm{data}}, \qquad A_i^{\gamma}, \qquad A_i^{\gamma+\gamma Z'} .
```

The corresponding files are named as

```text
A012_comparison_<benchmark_name>.png
```

### 6.2 Isolated deformation plots

These plots show the shift induced by the Z' interference:

```math
\Delta A_i
=
A_i^{\gamma+\gamma Z'} - A_i^{\gamma}.
```

The corresponding files are named as

```text
A012_delta_<benchmark_name>.png
```

The deformation plots are often more informative than the direct comparison
plots, because the photon-only and photon--Z' curves can be visually
indistinguishable when the absolute shift is very small.

---

## 7. Numerical diagnostics

The notebook writes two diagnostic CSV files:

```text
individual_Zprime_shift_diagnostics.csv
individual_Zprime_chi2_diagnostics.csv
```

The shift table reports, for each benchmark and coefficient,

```math
\max |\Delta A_i|, \qquad \langle |\Delta A_i| \rangle .
```

The chi-square table reports a simple diagnostic comparison with data:

```math
\chi^2
=
\sum_k
\frac{
\left(A_i^{\mathrm{th}}(Q_{T,k})-A_i^{\mathrm{exp}}(Q_{T,k})\right)^2
}{
\sigma_{i,k}^2
}.
```

This chi-square is intended as a first diagnostic only, not as a final exclusion
limit.

---

## 8. References

1. G. Aad et al. (ATLAS Collaboration),
   *Measurement of the angular coefficients in Z-boson events using electron and
   muon pairs from data taken at sqrt(s) = 8 TeV with the ATLAS detector*,
   JHEP 08 (2016) 159.

2. HEPData record for the ATLAS angular-coefficient measurement,
   DOI: `10.17182/hepdata.76986`.

3. V. E. Lyubovitskij, A. S. Zhevlakov, and I. A. Anikin,
   *Transverse momentum dependence of the T-even hadronic structure functions
   in the Drell--Yan process*, Phys. Rev. D 110, 074028 (2024).

4. V. E. Lyubovitskij, W. Vogelsang, F. Wunder, and A. S. Zhevlakov,
   *Perturbative T-odd asymmetries in the Drell--Yan process revisited*,
   Phys. Rev. D 109, 114023 (2024).

---

## 9. Notes

- The calculation currently focuses on `A0`, `A1`, and `A2`.
- The Z' implementation keeps only the photon--Z' interference term.
- The term `|M_Z'|^2` is not included.
- The benchmark values are illustrative unless a specific Z' model is supplied.
- Very small differences between the photon-only and photon--Z' direct curves are
  expected, especially for universal or heavy Z' benchmarks.
