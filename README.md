
# ShaplEIG: Bayesian Experimental Design for Shapley Value Estimation

This repository is the research codebase accompanying the paper **“ShaplEIG: Bayesian Experimental Design for Shapley Value Estimation.”** It contains the implementation and experiments used in the paper and is provided primarily for reproducibility. For applying ShaplEIG in practice, please use the maintained implementation in the [`shapiq` package](https://shapiq.readthedocs.io/en/stable/generated/shapiq.approximator.ShaplEIG.html#shapiq-approximator-shapleig).

For citations, please use the following BibTeX:

```bibtex
@inproceedings{
rundel2026textttshapleig,
title={\${\textbackslash}texttt\{Shapl{EIG}\}\$: Bayesian Experimental Design for Shapley Value Estimation},
author={David Rundel and Fabian Fumagalli and Maximilian Muschalik and Bernd Bischl and Matthias Feurer},
booktitle={Forty-third International Conference on Machine Learning},
year={2026},
url={https://openreview.net/forum?id=ub9PwBtHqD}
}
```

This code is released under the MIT License. See `LICENSE` for details.

## Installation

This project requires **Python 3.11.13** and uses [Poetry](https://python-poetry.org/) for dependency management. Make sure you have Poetry installed and available in your `PATH` before proceeding. 

1. **Navigate to the provided folder:**
   ```bash
   cd shapleig
   ```

2. **Install dependencies:**
   Poetry will use `pyproject.toml` and `poetry.lock` to create a reproducible environment. All required packages and their exact versions can be found in these files.

   ```bash
   poetry install
   ```
   - If you want to update dependencies, modify `pyproject.toml` and run `poetry update`.
   - The `poetry.lock` file ensures exact versions for reproducibility.

3. **Activate the environment:**
   ```bash
   poetry shell
   ```

## Reproducing Experiments

All experiments from the paper can be reproduced using the config files provided in the `src/xac/experiments/conf` folder. Each experiment has its own config file:

### Experiment Configs

 - `src/xac/experiments/conf/shapleig_crv_tabpfn_8p.yaml` and `src/xac/experiments/conf/shapleig_crv_tabpfn_10p.yaml` for the feature importance (FI) experiments using TabPFN (parallel execution). 

 - `src/xac/experiments/conf/shapleig_crv_hypershap_8p.yaml` and `src/xac/experiments/conf/shapleig_crv_hypershap_16p.yaml` for the hyperparameter importance (HPI) experiments using YAHPO Gym Surrogates (parallel execution).

 - `src/xac/experiments/conf/shapleig_crv_shapiq_9p.yaml`, `src/xac/experiments/conf/shapleig_crv_shapiq_dv_10p.yaml`, and `src/xac/experiments/conf/shapleig_crv_shapiq_16p.yaml` for the dataset valuation (DV) and local explanation (LE) experiments using **shapiq** (parallel execution).

  - `src/xac/experiments/conf/shapleig_crv_tree_corrgroup.yaml`, `src/xac/experiments/conf/shapleig_crv_tree_crime.yaml`, and `src/xac/experiments/conf/shapleig_crv_tree_nhanesi.yaml` for LE experiments using TreeSHAP (parallel execution).

### Running Experiments

To run an experiment, use the following command:
```bash
poetry run python -m xac.experiments.cli --config-path conf --config-name <config_file_name_without_extension>
```
For example:
```bash
poetry run python -m xac.experiments.cli --config-path conf --config-name shapleig_crv_tabpfn_8p
```

The results of each experiment, including all presented plots from the paper, will be found in the `multirun/` folder.

Depending on the experiment, it might be necessary to load data from other sources, as will be discussed in the following section.

### Data Sources

#### YAHPO Gym Surrogates for HPI Games

When working with the HPI games using YAHPO Gym surrogates, one must install **yahpo_gym** using the following [instructions](https://github.com/slds-lmu/yahpo_gym/blob/main/yahpo_gym/notebooks/using_yahpo_gym.ipynb). This requires manually downloading [metadata](https://github.com/slds-lmu/yahpo_data) for surrogate models and storing it under `data/yahpo_surrogates/yahpo_data`.

#### TabPFN Game Precomputation for FI Games

When working with the FI games using TabPFN, the script `src/xac/experiments/conf/shapley_precomputer.yaml` must be run and the results stored under `data/shapiq_games/tabpfn` in the folders `tid15`, `tid37`, and `tiddiabreg` respectively.

#### shapiq Precomputed Games

When working with the precomputed games from **shapiq** in the context of local explanation and data valuation games, you need to manually download the following files from the [repository](https://github.com/mmschlk/shapiq/tree/main/data/precomputed_games) and store them at the respective positions:

 - These [files](https://github.com/mmschlk/shapiq/tree/main/data/precomputed_games/ImageClassifier_Game/14) for the LE game on the ResNet model. The files must manually be moved into `data/shapiq_games/resnet/14`.

 - These [files](https://github.com/mmschlk/shapiq/tree/main/data/precomputed_games/ImageClassifier_Game/16) for the LE game on the ViT model with 16 patches. The files must manually be moved into `data/shapiq_games/vit/16`.

 - These [files](https://github.com/mmschlk/shapiq/tree/main/data/precomputed_games/ImageClassifier_Game/9) for the LE game on the ViT model with 9 patches. The files must manually be moved into `data/shapiq_games/vit/9`.

 - These [files](https://github.com/mmschlk/shapiq/tree/main/data/precomputed_games/BikeSharing_DatasetValuation_Game/10) for the DV game on the Bikesharing Dataset. A subset of the files must manually be moved into `data/shapiq_games/dvbsgb/10` and `data/shapiq_games/dvbsrf/10` according to the file names indicating the respective model (GP= gradient boosting, RF= Random forest).

 - These [files](https://github.com/mmschlk/shapiq/tree/main/data/precomputed_games/CaliforniaHousing_DatasetValuation_Game/10) for the DV game on the California Housing Dataset. A subset of the files must manually be moved into `data/shapiq_games/dvchgb/10` according to the file names indicating the respective model.


