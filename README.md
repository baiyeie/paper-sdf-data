# Metallocene catalyst activity prediction

Analysis scripts and source data for Figures 2–6, 7a and 7c, and the associated supplementary analyses.

- `Parameters.xlsx` contains 1,408 propylene-polymerization records.
- Place the 541 catalyst structure files in `sdf/` next to `Parameters.xlsx` before running the analyses.
- `Fig2/` through `Fig7/` and `SI/` contain the scripts. Output tables are stored in `Fig_results/` or `SI/SI_results/`.

## Run an analysis

From this directory, install the dependencies and run the script for the panel you want to reproduce.

```sh
python -m pip install numpy pandas scipy scikit-learn rdkit openpyxl lightgbm
python "Fig3/_local_inflation (3a, 3c).py"
```

On macOS and Linux, replace backslashes with forward slashes in the spreadsheet's `File_path` column so the scripts can find the filenames in `sdf/`.

- Run Figures 2a–c before 2d, Figure 4f before 5d, and Figure 5a before 6d–e.
- Run Figures 3d, 4f and 5d before running `SI/_assemble_tables (S2, S3, S5, S6).py`.
- Figure 4f also requires `torch`, `torch-geometric` and `tqdm`. Graph-model results can vary across runs.

## Figure 7

```sh
python "Fig7/_local_recovery (7a, TableS8).py"
python "Fig7/_local_shortlist (7c, SI).py"
```

The recovery script uses the saved predictions from Figures 4a and 6d to generate the source data for Figure 7a and Table S8. The shortlist script uses the fixed platform scores and normalized family priors in `Fig7/Fig7_shortlist_input.csv` to generate the source data for Figure 7c, Figures S2–S3 and Tables S9 and S11–S13.
