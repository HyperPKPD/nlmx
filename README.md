# nlmx: Low code PKPD software suite that talks to you

**Built for automation capabilities. Faster than NONMEM. More complete.**

`nlmx` can fit the model for you or start from a *completed* NONMEM base model: the control stream,
`.ext`, `.cov`, `.cor`, and optionally a `.phi` or `.tab`, plus the analysis
dataset. You list the covariates to test on each parameter
(`CL = ["WT", "AGE", "SEX"]`). nlmx then:

* detects each covariate's type (continuous, binary or categorical);
* tries the typical equation of that type: a power model
  `(COV/median)**θ` for continuous covariates and a proportional shift
  `(1 + θ)` per level for binary and categorical ones (`--all-equations`
  also tries the exponential, linear and grouped forms and keeps the best
  one per pair);
* decides which covariates are retained, refits the retained effects
  jointly and reports the final covariate model with its objective
  function value.

None of this  NONMEM ([docs/COVARIATES.md](docs/COVARIATES.md)).
There are two engines, selected with `method`:

| method | engine | question it answers |
|---|---|---|
| `frozen` | Frozen-model efficient score test | Does the data push each candidate coefficient away from zero at the base solution? Gives a univariate and conditional score test, forward/backward selection and parameter-uncertainty propagation. |
| `posterior` | Posterior random-effect stability selection | Which covariates *repeatedly* explain between-subject variability across posterior draws, subject resamples, base-parameter perturbations and several learners? |
| `both` | both engines + comparison | Where do the two lines of evidence agree? |

`nlmx` combines the former `frozenmodel` and `posteriormodel` programs. It
reads the model directly from the NONMEM control stream. It can also:

* check a dataset (`nlmx qc-data`, [docs/QC-DATA.md](docs/QC-DATA.md)) and a
  control stream (`nlmx qc-nm`, [docs/QC-NM.md](docs/QC-NM.md)) before
  modelling, with a corrected copy of the control stream;
* run a complete non-compartmental analysis of a dataset, with tables, a
  report and figures (`nlmx nca`, [docs/NCA.md](docs/NCA.md));
* draw the exploratory plots and tables of a PK/PD dataset, standard and
  custom (`nlmx eda`, [docs/EDA.md](docs/EDA.md));
* simulate a study like NONMEM's `$SIMULATION`: generate the dataset (CDC growth charts for ages 2–20,
  normal weights for adults) or use your own, simulate it, compute the steady-state exposure of every
  subject (Cmax, Cmin, Cavg, AUCtau, …), tables and figures, in parallel
  (`--threads`) and reproducibly (`--seed`) (`nlmx sims`,
  [docs/SIMS.md](docs/SIMS.md));
* derive initial estimates from a dataset alone (`nlmx autoinit`,
  [docs/AUTOINIT.md](docs/AUTOINIT.md));
* screen 1-, 2- and 3-compartment structures, 1-/2-compartment TMDD
  models and, on request, transit-compartment absorption (Savic) for the
  base model (`nlmx screen`, [docs/SCREEN.md](docs/SCREEN.md));
* fit the model (`nlmx fit`, [docs/FITTING.md](docs/FITTING.md));
* check it by simulation (`nlmx vpc`, [docs/VPC.md](docs/VPC.md));
* draw the forest plot of its covariate effects (`nlmx forest`,
  [docs/FOREST.md](docs/FOREST.md));
* write synthetic projects of any design to practise on (`nlmx generate`,
  [docs/GENERATE.md](docs/GENERATE.md));
* draw publication figures for all of these (`--plots`).

This README describes **version 2.27.0**. [CHANGELOG.md](CHANGELOG.md) has
the version history, and [docs/VERSIONING.md](docs/VERSIONING.md) explains
how versions are made.

> Results are **recommendations**. Every retained relationship must be
> confirmed with an explicit NONMEM fit before it is used for inference or
> submission.

---
