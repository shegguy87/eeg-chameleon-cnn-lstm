# EEG-Derived State Discovery Using Correlation-Based CHAMELEON and CNN--LSTM

This repository is intended to accompany the manuscript:

**"Data-driven Discovery of EEG-derived States using Correlation-based
CHAMELEON Clustering and CNN-LSTM Classification."**

It documents a research workflow for exploring EEG-derived states from
the SAM40 dataset using participant-specific relaxation baselines,
multidomain EEG features, correlation-based CHAMELEON clustering, and
downstream deep-learning classification.

> **Repository status:** Starter repository structure and documentation.
> Replace the placeholder scripts and configuration details with the
> actual analysis code used for the manuscript before describing this
> repository as a complete reproduction package. Do not report results
> as reproducible until the scripts have been tested from raw data to
> final tables.

## Study overview

The manuscript analyses EEG recordings from 40 participants in the
publicly available SAM40 dataset. The cognitive-task conditions are
arithmetic, mirror-image recognition, and the Stroop Colour--Word task.
Relaxation recordings are used as participant-specific baselines.

The described feature representation contains 20 time-domain,
frequency-domain, and nonlinear features for each of 32 EEG channels
(640 features per task observation). Candidate CHAMELEON solutions with
(K=2), (K=3), and (K=4) are evaluated using cluster separation,
subsampling stability, feature differentiation, downstream
classification, and comparison with perceived stress ratings.

The deep-learning analysis considers CNN, LSTM, and CNN--LSTM models.
These models classify the discovered cluster assignments; they do
**not** directly predict validated psychological stress levels.

## Important interpretation and reproducibility notes

-   The manuscript describes an initial observation-level stratified
    split and a subsequent participant-wise evaluation for the selected
    (K=2) solution.
-   The manuscript states that feature standardisation and clustering
    were performed before the participant-level split. Therefore, the
    subject-wise results should not be presented as an independent
    evaluation of the complete state-discovery pipeline.
-   A stronger subject-independent evaluation should fit preprocessing,
    scaling, and clustering using training participants only and specify
    how an unseen participant is assigned to a cluster.
-   Record all preprocessing parameters, feature definitions and
    ordering, software versions, random seeds, participant split files,
    prediction outputs, and confusion matrices.
-   Do not invent missing parameters or claim exact reproduction unless
    the implementation and outputs have been verified.

## Dataset

The study uses the publicly available **SAM40** EEG dataset:

-   Figshare dataset: https://doi.org/10.6084/m9.figshare.14562090.v1
-   Dataset article: https://doi.org/10.1016/j.dib.2021.107046

Please verify the dataset citation and licence on the dataset landing
page before publishing derived files. Cite the original dataset authors
in publications and in this repository.

**Recommended practice:** Initially link to the official dataset instead
of uploading a duplicate of the full dataset. Only redistribute the
original files if the dataset licence and terms explicitly permit
redistribution. Do not upload participant-identifying information or any
data that you are not authorised to share.

## Suggested repository structure

``` text
eeg-chameleon-cnn-lstm/
├── README.md
├── CITATION.cff
├── LICENSE
├── requirements.txt
├── .gitignore
├── configs/
│   └── analysis_config.yaml
├── data/
│   ├── README.md
│   ├── raw/                 # not tracked by Git; obtain from official dataset
│   └── processed/           # derived files; include only if permitted
├── notebooks/
│   └── 01_exploratory_analysis.ipynb
├── src/
│   ├── preprocessing.py
│   ├── feature_extraction.py
│   ├── baseline_reference.py
│   ├── chameleon_clustering.py
│   ├── train_models.py
│   ├── evaluate_models.py
│   └── utils.py
├── scripts/
│   ├── run_pipeline.py
│   └── generate_tables.py
├── splits/
│   ├── observation_level_split.csv
│   └── participant_level_split.csv
├── results/
│   ├── tables/
│   ├── confusion_matrices/
│   └── predictions/
└── docs/
    └── reproducibility_notes.md
```

This is a suggested structure, not a statement that all these files
already exist. Rename or remove placeholders to match the actual
codebase.

## Analysis workflow

The intended workflow is:

1.  Download the SAM40 dataset from its official source and record the
    dataset version and access date.
2.  Identify the participant, condition, and trial files used in the
    study. Document the exact files included and excluded.
3.  Preprocess the EEG signals. Record the filter type, order, passband
    ripple, passband, ICA implementation, artifact/component rejection
    rules, and any channel-rejection criteria.
4.  Extract the 20 features from each of the 32 EEG channels. Document
    all feature formulas, parameter settings, units, and the exact
    feature ordering.
5.  Calculate each participant's relaxation baseline and subtract it
    from the corresponding cognitive-task feature vectors.
6.  Standardise the features and calculate Pearson correlation distances
    according to the documented procedure.
7.  Construct the weighted nearest-neighbour graph and run CHAMELEON
    clustering for (K=2,3,4). Record all graph and clustering
    parameters.
8.  Evaluate cluster separation, subsampling stability, feature
    differentiation, and the relationship between cluster assignments
    and perceived stress ratings.
9.  Train CNN, LSTM, and CNN--LSTM models to classify the cluster
    assignments. Record model configurations, random seeds, split
    membership, and software/hardware environment.
10. Export predictions, confusion matrices, classification reports, and
    the tables used in the manuscript.
11. Run the pipeline from a clean environment and compare generated
    outputs against the manuscript before claiming reproducibility.

## Installation

The exact dependencies must be taken from the original code. Once
`requirements.txt` has been completed, create a virtual environment and
install the listed packages.

### Windows (PowerShell)

``` powershell
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY

python -m venv .venv
.\.venv\Scripts\Activate.ps1

python -m pip install --upgrade pip
pip install -r requirements.txt
```

### macOS / Linux

``` bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY

python3 -m venv .venv
source .venv/bin/activate

python -m pip install --upgrade pip
pip install -r requirements.txt
```

Replace `YOUR-USERNAME/YOUR-REPOSITORY` with the actual GitHub owner and
repository name. These commands will work only after the repository
files and dependency list have been added.

## Running the analysis

The following commands are examples of the intended interface, not
executable commands until the corresponding scripts have been created
and their arguments implemented.

``` bash
python scripts/run_pipeline.py --config configs/analysis_config.yaml
python scripts/generate_tables.py
```

Document the real commands and expected outputs here after testing the
code.

## Reproducibility checklist

Before making a release, ensure that the repository contains:

-   [ ] The exact Python source code used to produce the manuscript
    results.
-   [ ] Python and package versions, plus operating system and relevant
    hardware details.
-   [ ] Dataset source, version/DOI, licence, file-selection rules, and
    data-preparation instructions.
-   [ ] Complete preprocessing parameters and artifact-removal criteria.
-   [ ] Feature formulas, feature order, and feature-extraction
    parameters.
-   [ ] CHAMELEON graph construction and merging parameters.
-   [ ] Random seeds and exact observation-level and participant-level
    split files.
-   [ ] Model architecture and training hyperparameters.
-   [ ] Per-observation predictions, confusion matrices, and
    classification reports for each reported result.
-   [ ] Scripts that regenerate every table and figure included in the
    manuscript.
-   [ ] A clean-environment test and a tagged release corresponding to
    the submitted manuscript.

## Citation

Please cite the manuscript and the original SAM40 dataset. Update the
following placeholder with the final bibliographic information when
available:

``` text
Authors. Data-driven Discovery of EEG-derived States using Correlation-based
CHAMELEON Clustering and CNN-LSTM Classification. [Journal, year, DOI to be added.]
```

## Licence

Choose a licence for the **code in this repository** after confirming
that you have the right to publish it. A code licence does not
automatically grant permission to redistribute the SAM40 dataset or
third-party files. If you are unsure, keep the repository private until
the rights and dataset terms have been checked.

## Contact

For questions about the code, contact the corresponding author listed in
the associated manuscript.
