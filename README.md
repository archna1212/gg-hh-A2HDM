# gg → hh in the SM and the aligned 2HDM

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/archna1212/gg-hh-A2HDM/blob/main/gg_hh_SM_A2HDM.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A single, self-contained Jupyter notebook that computes Higgs-pair production through gluon fusion at the
LHC, **pp → hh at leading order (LO) with exact top-quark mass dependence**, first in the Standard Model (SM)
and then in the aligned two-Higgs-doublet model (A2HDM).

The notebook is shipped **with its outputs saved**, so you can read the plots, validation checks and tables
directly on GitHub without running anything.

## What it does

- Builds the exact one-loop gg → hh amplitude (Glover–van der Bij; Plehn–Spira–Zerwas form factors) from scalar
  one-loop integrals evaluated inside the notebook. No external Fortran library is needed.
- Splits the amplitude into **triangle**, **box** (spin 0 and spin 2) and **resonant heavy-scalar** pieces,
  keeping every interference term.
- Convolves with the gluon–gluon luminosity from an LHAPDF grid (read with the pure-Python
  [`parton`](https://pypi.org/project/parton/) package) to get σ(pp → hh) and the m_hh and p_T,h distributions.
- Implements the A2HDM in the Higgs basis (CP conserved) with a constraint filter (perturbativity, boundedness
  from below, tree-level unitarity, oblique T, Higgs-coupling fits, flavour bound on ς_u) applied before any
  cross section is computed.
- Validates itself: 21 checks, each printed as PASS or FAIL.

## Headline numbers (13 TeV, μ_R = μ_F = m_hh/2, PDF4LHC21_40, LO)

| Benchmark | Description | σ_LO [fb] | σ / σ_SM |
|---|---|---|---|
| SM | m_t = 173 GeV | 16.51 | 1 |
| BP1 (non-resonant) | m_H = m_A = m_H± = 500 GeV, sin α̃ = −0.10, y_t^H = 0, κ_λ = 0.725 | 20.96 | 1.27 |
| BP2 (resonant) | m_H = 300 GeV, m_A = m_H± = 350 GeV | 60.89 | 3.69 |

With the PDF4LHC15 set used in the literature, the SM value is 16.79 fb, which agrees with the full-m_t LO
result of Borowka et al. (16.72 fb) to within 2%.

**Accuracy statement.** Everything here is LO QCD. NLO QCD with full top-mass dependence raises the SM rate by
a factor of about 1.7. Use the ratios σ_A2HDM / σ_SM, which are far more stable, and do not quote the absolute
LO numbers as predictions. The approximations (no b-quark box, fixed-width Breit–Wigner for H, no one-loop
corrections to the scalar self-couplings, simplified constraints) are listed in Section 12 of the notebook.

## Running it

**In the browser:** click the Colab badge above. Colab already provides numpy, scipy, pandas and matplotlib.

**Locally:**

```bash
git clone https://github.com/archna1212/gg-hh-A2HDM.git
cd gg-hh-A2HDM
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab gg_hh_SM_A2HDM.ipynb
```

Then run the notebook **top to bottom**.

- **Internet needed on first run.** The notebook downloads the LHAPDF grid (`PDF4LHC21_40`, and `PDF4LHC15_nlo_100_pdfas` for the literature check) from the CERN LHAPDF mirror into a local `pdfsets/` folder. That folder is git-ignored.
- **Runtime.** The first full run takes several minutes, because the loop integrals are computed numerically (in the saved run, about 140 s for the SM and 100–125 s for each A2HDM benchmark). Box form factors are cached and reused, so later models cost seconds.
- **Deterministic.** There are no random numbers. Clearing the cache and recomputing gives bit-identical results (checked in Section 11).

## Changing the setup

Edit the `SM`, `COLLIDER` or `NUM` dictionaries in Section 3 and re-run. To study a new A2HDM point, call
`a2hdm_point(...)` and then `run_a2hdm(point)`. To use another PDF set, set `COLLIDER["pdf_set"]` to any
LHAPDF set name.

## Repository contents

| File | Purpose |
|---|---|
| `gg_hh_SM_A2HDM.ipynb` | The notebook, with executed outputs |
| `requirements.txt` | Python dependencies |
| `CITATION.cff` | Citation metadata (GitHub shows a "Cite this repository" button) |
| `LICENSE` | MIT |

## References

- E. W. N. Glover and J. J. van der Bij, Nucl. Phys. B309 (1988) 282
- T. Plehn, M. Spira, P. M. Zerwas, Nucl. Phys. B479 (1996) 46 [hep-ph/9603205]
- S. Borowka et al., PRL 117 (2016) 012001 [1604.06447] and JHEP 10 (2016) 107 [1608.04798]
- A. Pich and P. Tuzón, PRD 80 (2009) 091702 [0908.1554]
- A. Celis, V. Ilisie, A. Pich, JHEP 07 (2013) 053 [1302.4022]
- I. F. Ginzburg and I. P. Ivanov, PRD 72 (2005) 115010 [hep-ph/0508020]
- H. E. Haber and D. O'Neil, PRD 83 (2011) 055017 [1011.6188]
- M. Jung, A. Pich, P. Tuzón, JHEP 11 (2010) 003 [1006.0470]

## Citing

Please use the "Cite this repository" button on GitHub, or `CITATION.cff`. An arXiv reference will be added here
when available.

## Author

Archana Gupta, Department of Physics, IIT (BHU), Varanasi.

Released under the [MIT License](LICENSE).
