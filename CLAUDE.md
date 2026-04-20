# CLAUDE.md

## Repository overview
This repository contains an **R-based hybrid cancer modeling framework** that combines a **Markov model** with a **cohort component projection model (CCPM)** to estimate the **costs** and **health impacts** of comprehensive cancer-control strategies.

The current version focuses primarily on **cancer treatment interventions**. Prevention and palliation are noted in the interface but are not yet available in this version.

## Core purpose
Use this repository to:
- preprocess baseline and input data,
- define intervention scenarios,
- run the cancer simulation model,
- analyze projected health and cost outputs.

## Main workflow
Run the project in this order:

1. **Preprocess data**  
   Run `R/scripts/00_preprocess_data.R` to prepare inherent input parameters and generate baseline objects.

2. **Set scenario assumptions**
   Update `sim_scen_inputs.xlsx` (sheet `ui`) with the desired user inputs, including:
   - target country,
   - intervention scale-up start year,
   - intervention scale-up end year,
   - target cancer types,
   - baseline treatment adherence coverage,
   - target treatment adherence coverage.

3. **Run the main simulation**  
   Run `R/scripts/01_main_sim_script.R` to execute the cancer model.

4. **Analyze outputs**  
   Run `R/scripts/02_analysis_outputs.R` to reproduce results and export analysis outputs.

## Important model assumptions
The current implementation includes several key defaults:
- The **Markov model** starts in **2000** and ends in **2050**.
- Baseline assumptions hold **2019 TP values constant** into future years unless changed in the main simulation script.
- Population projections run from **2020** to **2050**.

If you change these assumptions, update the relevant values directly in `01_main_sim_script.R`.

## Repository structure
Top-level structure:

- `Diagrams/` — model and workflow diagrams.
- `R/` — all modeling code and data workflow assets.
- `.gitignore` — git ignore rules.
- `README.md` — project introduction and run instructions.
- `settings.yml` — project-level configuration.
- `cancer-markov-ccpm.Rproj` — RStudio project file.

Within `R/`:
- `fnx/` — reusable functions.
- `inputs/` — input files and intermediate inputs.
- `outputs/` — model outputs and exported results.
- `scripts/` — main executable scripts.
- `library.R` — package loading and common setup.
- `scratchpad.R` — exploratory code and ad hoc testing.

## Environment and dependencies
At minimum, install:

```r
install.packages("tidyverse")
install.packages("openxlsx")
install.packages("yaml")   # used by library.R to read settings.yml
install.packages("curl")   # used by library.R to check internet connectivity
```

This project is intended to be run in **RStudio** using the included `.Rproj` file.

`library.R` also loads utility functions from [Mohamed-Albirair/my-R-functions](https://github.com/Mohamed-Albirair/my-R-functions) when internet is available, with local fallback paths defined in `settings.yml`.

## Expected coding conventions
When editing this repository:
- keep all reusable functions inside `R/fnx/` or clearly named script files,
- avoid placing production logic in `scratchpad.R`,
- preserve the sequential script structure unless you are explicitly refactoring,
- write outputs to `R/outputs/`,
- keep input assumptions transparent and centralized.

## Guidance for code assistants
When helping with this repository, prefer the following behavior:
- assume this is an **R modeling repository**, not a package,
- preserve existing script numbering and workflow order,
- make minimal, explicit changes unless a broader refactor is requested,
- document any changed assumptions,
- avoid introducing unnecessary dependencies,
- keep file paths relative to the project root,
- maintain compatibility with RStudio project execution.

## Common tasks
Examples of useful support tasks:
- adding or revising intervention scenarios,
- improving parameter handling from Excel inputs,
- refactoring model scripts into reusable functions,
- validating output tables and plots,
- improving reproducibility and documentation,
- creating sensitivity-analysis workflows.

## Notes
- The preprocessed baseline data object is `all-param.RData`, stored at `R/inputs/all-param.RData`. The README also references `all-baseline-data.RData` as an older name — if the file is not present locally, run `00_preprocess_data.R` first or restore from the Google Drive link in the README.
- The GitHub remote for this repository is: https://github.com/wrgarciag/cancer-control

## Environment notes
- **Shell**: The bash shell may not function correctly when the project is on a OneDrive path with spaces (Windows/MINGW64). Use the RStudio terminal or run scripts directly in RStudio instead.
- **Working directory**: Always open the project via `cancer-markov-ccpm.Rproj` in RStudio to ensure the working directory is set correctly to the project root.
