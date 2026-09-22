# Catalyst activity prediction from published data — data and code

Reproduction package for the manuscript

> **Catalyst activity prediction from published data: temporal validation limits transfer
> across publication eras**
> Boning Yan, Xu Huang, Jiankun Li, Yucai Cao, Ming Lei

Everything behind Figures 2–6 and the Supporting Information lives in [`Figure/`](Figure/).
The technical README of the pipeline (one script per panel, one output folder per panel, all
headline numbers) is [`Figure/README.md`](Figure/README.md).

## Contents

```
Figure/
  Parameters.xlsx      curated dataset: 1,408 measurement records x 18 columns
  sdf/                 541 DFT-optimised catalyst structures (.sdf)
  Fig2/ ... Fig6/      one script per figure panel + its output folder
  SI/                  SI tables S2-S6 assembly, Fig. S1, Table S4, Table S7
```

Each panel has exactly one script and one output folder, so any number in the paper can be
traced back to the code and the file that produced it.

## Quick start

```bash
pip install -r requirements.txt

cd Figure
python "Fig3/_local_inflation (3a, 3c).py"
python "Fig2/_local_rf_new (2c).py"
...
```

Run order, expected runtimes and per-script environment switches are listed in
[`Figure/README.md`](Figure/README.md). A few scripts consume the output of an earlier one
(e.g. `Fig2/_ceiling_analysis (2d).py` reads the out-of-fold tables of panels 2a–2c), and the
figure scripts must run before `SI/_assemble_tables (S2, S3, S5, S6).py`.

## Data

* `Figure/Parameters.xlsx` — the curated dataset. One row per measurement record; columns cover
  the catalyst identifier, the twelve process variables, the SDF path, the measured activity,
  the source DOI and the publication year. Activity is used as `ln(1 + activity)`.
* `Figure/sdf/` — the DFT-optimised geometries (Gaussian 16, B3LYP/def2-SVP, D3(BJ)), one file
  per catalyst. The `File_path` column of the spreadsheet stores absolute Windows paths from the
  machine that produced the results; **every script falls back to `Figure/sdf/<filename>`**, so
  the package runs as-is after cloning.
* `Figure/**/Fig_results/` and `Figure/SI/SI_results/` — the archived source data for every panel
  and table.

## Notes on reproducibility

* The four **graph** rows of panel 4f (GINE, EGNN and the two graph+fingerprint hybrids) are
  **not reproducible run to run**: CUDA `index_add_` is non-deterministic and the seed-to-seed
  spread on 367 training structures is large. The script reports the seed-averaged R² together
  with the seed mean and standard deviation. The two tree rows (Morgan+RF, topological
  descriptors+RF) and both year probes are exact.
* Panels 2c, 4a, 4b, 4d and 5a are deterministic given the seed, but the last digits of
  individual predictions can differ between scikit-learn versions; the reported metrics are not
  affected.
* `Fig4/_local_representation_gnn (4f).py` needs `torch` and `torch_geometric` for the graph
  models. With `GT_SKIP_GNN=1` it runs only the two tree baselines and needs neither.

## License and citation

* **Code**: add your chosen license here (MIT / BSD-3 are the usual choices for a chemistry
  methods repo).
* **Dataset and structures**: literature-derived; please check the terms of the source
  publications before choosing a licence for `Parameters.xlsx`. CC BY 4.0 is a common choice
  for curated datasets.
* For a citable snapshot, tag a release and archive it on Zenodo, then use that DOI in the
  paper's "Data and code availability" statement.
