# Local Risk Bounds for Statistical Aggregation

A theoretical and numerical study of how combining predictors can yield sharper
risk guarantees than selecting a single model. The project examines **exponential
weights (EW)** and **Q-aggregation**, with a focus on local complexity measures
and the practical computation of aggregation estimators.

**Main question:** how can risk bounds reflect the concentration of the prior
around good predictors, and how can the resulting estimators be approximated
numerically?

**Project authors:** Laurent Martini-Bonnet and Yann Adorno, ETH Zurich.

Based on *Local Risk Bounds for Statistical Aggregation* by Jaouad Mourtada,
Tomas Vaškevičius and Nikita Zhivotovskiy.

**[Read the report](final_paper_GML.pdf)** ·
**[Explore the numerical notebook](experiments_ULA.ipynb)**

## Theoretical analysis

Under squared loss, aggregation replaces the choice of one predictor with a
weighted combination of predictors. This convexifies the prediction problem
and can enable fast excess-risk rates under the relevant assumptions.

| Method | Principle | Guarantee studied in the report |
| --- | --- | --- |
| **Exponential weights** | Assign Gibbs weights according to empirical risk and a prior distribution. | A localized risk bound in expectation under Gaussian noise. |
| **Q-aggregation** | Penalize both the average individual risk and the risk of the combined predictor, with KL regularization. | A localized high-probability bound under sub-Gaussian noise. |

The report explains how **local complexity** improves on a global description
of the predictor class by emphasizing predictors with low risk. It also outlines
the Q-aggregation proof through strong convexity, noise control and convex duality,
and explains why that argument does not transfer directly to EW.

## Contributions of the project

- **Explain the theory:** connect model selection, aggregation, localization and
  the distinction between expectation and high-probability guarantees.
- **Examine extensions:** discuss other noise distributions and losses, identifying
  assumptions and obstacles rather than asserting unrestricted generalizations.
- **Connect theory with computation:** study grid approximations and Langevin
  sampling for EW, including the tractable linear-Gaussian case.
- **Investigate Q-aggregation in practice:** discuss finite-dictionary optimization
  and the opportunities and limitations of its moment-based characterization.

The main localized risk theorems are results of the reference paper. This project
provides an exposition, analysis of computational questions and numerical
illustrations.

## Numerical notebook

The published notebook focuses on **EW approximation using the Unadjusted
Langevin Algorithm (ULA)** in synthetic linear regression. It uses a smooth
approximation to a Laplace-prior penalty and a full-data gradient, with the
sampling loop compiled using Numba.

1. **Generate a regression problem:** 50 observations, 10 parameters and a sparse
   ground-truth coefficient vector with three nonzero entries.
2. **Approximate EW:** run ULA and average parameter samples after burn-in.
3. **Illustrate the local bound:** estimate prediction risk over 100 independent
   observation-noise realizations and approximate the localized-prior quantity
   using a second sampling procedure.
4. **Inspect sampling diagnostics:** compare three chains using trace plots and
   coordinate-wise Gelman-Rubin statistics.
5. **Compare estimators:** inspect coefficients and prediction risks against a
   Lasso reference.

### Saved results

The notebook's recorded run reports an average prediction risk of **0.1951** and
an estimated localized-bound quantity of **1.5411**. The mean reported
Gelman-Rubin statistic is **1.0034**.

These values are saved notebook outputs, not a fresh rerun. They provide a
numerical illustration consistent with the inequality in this setting, rather
than a proof or a general performance guarantee.

## Repository structure

| File | Contents |
| --- | --- |
| `final_paper_GML.pdf` | Theory, proof discussion, computational approaches, figures and experimental settings. |
| `experiments_ULA.ipynb` | EW sampling with a smoothed Laplace penalty, risk experiments, chain diagnostics and Lasso comparison. |
| `LICENSE` | MIT license. |

The report also discusses a Gaussian-prior benchmark and Q-aggregation methods;
the published notebook concentrates on the Laplace-type EW experiment and does
not include every computation described in the report.

## Running the notebook

In a Python virtual environment, install the dependencies:

```bash
python -m pip install numpy matplotlib numba scikit-learn jupyter
python -m jupyter notebook experiments_ULA.ipynb
```

Run the cells in order. The notebook generates synthetic data internally, so no
external dataset is required. The repeated sampling experiment is the main
computational cost, and the first call also compiles the Numba sampling loop.
Package versions are not pinned in the original repository, and stochastic
reruns may produce different values.

## Interpretation and limitations

- Constant-step ULA introduces discretization bias, while smoothing the Laplace
  penalty changes the exact target distribution.
- The localized-prior calculation uses the known synthetic ground truth; it is
  not a directly observable certificate for an arbitrary real dataset.
- Monte Carlo averages and chain diagnostics do not prove the risk theorem or
  establish exact convergence to the target distribution.
- The Lasso comparison is illustrative: its regularization scale is not matched
  to the ULA target, so it should not be described as the exact MAP estimator of
  that same distribution.

## Reference

Jaouad Mourtada, Tomas Vaškevičius and Nikita Zhivotovskiy.
**Local Risk Bounds for Statistical Aggregation.** COLT, 2023.
[Publication](https://proceedings.mlr.press/v195/mourtada23a.html) ·
[Full preprint](https://arxiv.org/abs/2306.17151).

