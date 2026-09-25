# HZMPS Surveillance: Bayesian State-Space Hurdle Model for Disease Surveillance

This repository contains code, data, and results for a Bayesian statistical
framework for disease surveillance that separates true outbreak signals from
persistent reporting differences between regions or countries.

The model combines a Hurdle Zero-Modified Power Series (HZMPS) observation
distribution with a sparse finite mixture over units and an AR(1) process on
the count mean, all embedded in a state-space framework. Estimation is
performed via Hamiltonian Monte Carlo in Stan.

---

## Repository structure

| Folder | Platform | Contents |
|---|---|---|
| `pilot_survey/`    | Google Colab | Pilot study, N=100, T=100, 97% cluster recovery |
| `simulation_T50/`  | Kaggle       | Full T=50 simulation study: 180 fits, 6 scenarios |
| `simulation_T100/` | Kaggle       | T=100 extension: 90 fits, 3 scenarios |
| `analysis/`        | Google Colab | Aggregation notebook, combined CSV, headline table, figures |

The T=50 and T=100 simulation studies were run on Kaggle (using `reduce_sum`
parallelization, under the 11-hour per-commit limit). Post-processing,
summary tables, and figures were generated on Google Colab.

---

## Headline result

Cluster count recovery (`G0`) and cluster assignment accuracy improve
monotonically with the length of the time series `T`. The within-cluster
spread parameter `sigma_eta` is weakly identified at T=50 but is recovered
cleanly at T=100.

| N   | G0 correct at T=50 | G0 correct at T=100 | sigma_eta Rhat failure at T=50 | sigma_eta Rhat failure at T=100 |
|-----|--------------------|---------------------|--------------------------------|----------------------------------|
| 50  | 60%                | 80%                 | 43%                            | 27%                              |
| 100 | 80%                | 93%                 | 63%                            | 27%                              |
| 200 | 90%                | 100%                | 60%                            | 3%                               |

**Recommendation for applied users:** use T >= 100 when the within-cluster
spread is of interest. Cluster membership and count-mean dynamics are
recovered at both time horizons.

---

## Requirements

- Python 3.10+
- CmdStanPy (with CmdStan 2.36.0 installed)
- Pandas, NumPy, Matplotlib
- Optional: Seaborn for additional plotting

---

## Notes on scope

- The simulation studies are self-contained and reproducible from the
  corresponding Kaggle notebooks.
- The `analysis/` folder combines the two simulation outputs into the
  summary tables and figures used in the manuscript.
- Additional empirical work on real disease surveillance data is planned
  and will be added in a separate folder.
