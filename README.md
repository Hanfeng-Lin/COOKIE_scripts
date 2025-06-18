# COOKIE-Pro: High-Throughput Covalent Ligand Kinetic Analysis

This repository contains the data analysis scripts for the publication: "COOKIE-Pro: Covalent Inhibitor Binding Kinetics Profiling on the Proteome Scale".

The scripts provided here focus on the high-throughput applications of the COOKIE-Pro data analysis framework.

## Description

This repository contains scripts to perform two main analyses described in our manuscript:
1.  **Reanalysis of SLC-ABPP dataset**: A script to convert single-point competition ratios (CR) from the SLC-ABPP dataset published by Kuljanin et al., 2021 into intrinsic non-covalent binding affinities ($K_I$).
2.  **Two-Point COOKIE-Pro Analysis**: A script to process data from a two-concentration covalent fragment screen, which includes fitting data to multiple kinetic models and using AIC/BIC for model selection to determine kinetic parameters. The input `PeptideGroups` dataset from our screening experiment is also included.

## Citation

If you use this code or the COOKIE-Pro method in your research, please cite our publication:

Lin, H., Yang, B., Ding, L., Holt, M. V., Jung, S. Y., Zhang, B., Wang, M. C., & Wang, J. (2024). COOKIE-Pro: Covalent Inhibitor Binding Kinetics Profiling on the Proteome Scale. *Journal Name*, Volume(Issue), pages. [DOI]


## System Requirements
* Python 3.9.7
* Jupyter Notebook

## Installation

1.  Clone the repository to your local machine:
    ```bash
    git clone [https://github.com/Hanfeng-Lin/COOKIE_scripts.git](https://github.com/Hanfeng-Lin/COOKIE_scripts.git)
    cd COOKIE_scripts
    ```

2.  It is recommended to create a virtual environment:
    ```bash
    python -m venv cookie_env
    source cookie_env/bin/activate  # On Windows use `cookie_env\Scripts\activate`
    ```

3.  Install the required packages using pip:
    ```bash
    pip install pandas numpy scipy matplotlib jupyter
    ```

## Usage

The repository contains scripts for two distinct high-throughput analyses.

### 1. Reanalysis of SLC-ABPP Dataset

This analysis converts single-point, experimental condition-dependent competition ratios (CR) into thermodynamic inactivation constants ($K_I$).

**Workflow:**
* The script assumes that fragments within the same warhead class share a similar electrophilic reactivity ($k_{inact}$).
* Median $k_{inact}$ values for chloroacetamide and acrylamide warheads, derived from the CovalentinDB, are used as class-specific constants.
* Using the experimental time and concentration from the SLC-ABPP study, the script applies the COOKIE-Pro equations to calculate a $K_I$ value for each fragment-cysteine pair with a CR > 2.
* Downstream metrics like Ligand Efficiency (LE) and Ligand Lipophilic Efficiency (LLE) are then calculated from the derived $K_I$ values.

### 2. Two-Point COOKIE-Pro Fragment Screening

This analysis processes data from the included `PeptideGroups` dataset, which was generated from a screen of 16 fragments at two concentrations (20 µM and 50 µM).

**Workflow:**
* The script calculates the apparent rate constant ($k_{obs}$) for each fragment-peptide pair at the two concentrations.
* Three distinct kinetic models are fitted to the two ($[I], k_{obs}$) data points:
    * **Linear Model**: For interactions where $[I] \ll K_I$.
    * **Hyperbolic Model**: The full Michaelis-Menton model.
    * **Constant Model**: For interactions where $[I] \gg K_I$.
* To select the most appropriate model and avoid overfitting, the Akaike Information Criterion (AIC) and Bayesian Information Criterion (BIC) are calculated for each fit. The model with the lowest score is chosen.
* This model selection allows for the most reliable estimation of kinetic parameters, providing either the inactivation efficiency ($k_{inact}/K_I$), the individual $k_{inact}$ and $K_I$ values, or an estimate of $k_{inact}$ depending on the best-fit model.

To run either analysis, launch Jupyter Notebook, open the relevant notebook file, modify file paths as needed, and execute the cells sequentially.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contact

For questions about the code or the COOKIE-Pro method, please contact Jin Wang at `wangj@bcm.edu`.
