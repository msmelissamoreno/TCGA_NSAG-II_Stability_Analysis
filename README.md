
# TCGA NSGA-II Bioinformatics

## Multilevel Feature Stability Analysis

This repository contains the computational workflow developed for a bioinformatics study examining the stability and reproducibility of gene feature selection using a multi-objective NSGA-II framework applied to The Cancer Genome Atlas (TCGA) RNA-sequencing data.

The analysis extends previously completed TCGA kidney cancer and lower grade glioma (TCGA-LGG) modeling studies by evaluating an important question in high dimensional genomic machine learning: **when an evolutionary feature selection algorithm identifies different gene subsets under different analytical conditions, do those subsets nevertheless represent reproducible predictive and biological information?**

<img width="1491" height="1055" alt="TCGA NSGA2 Bioinformatics Stability Analysis" src="https://github.com/user-attachments/assets/f8d61690-baf4-440d-87b3-0fcc185fdb1b" />

~

Rather than defining stability solely by exact gene overlap, this workflow evaluates feature reproducibility at multiple levels, including exact gene recurrence, correlation-adjusted similarity, preservation of gene relationships in held-out patients, network structure, predictive performance, analytical representation, and perturbation of the patient sample.

The notebook uses previously generated and fixed modeling inputs rather than reacquiring or reprocessing TCGA data. This preserves the original training/validation partitions, candidate gene universes, and selected NSGA-II panels and allows the analyses to focus specifically on feature stability.

### Analyses Included

The workflow contains three complementary stability experiments:

1. **TCGA-LGG cross-seed stability**  
   Five independently initialized NSGA-II searches are compared using exact overlap, pairwise Jaccard similarity, the Nogueira feature stability estimator, correlation-adjusted one-to-one gene matching, null models, chance-corrected stability, cross-seed correlation networks, connected components, bootstrap robustness, held-out correlation preservation, and predictive reproducibility.

2. **TCGA kidney TPM-versus-CPM stability**  
   Gene panels produced from separately processed TPM and CPM RNA-seq representations are compared to determine whether changes in expression normalization alter the genes selected by NSGA-II. Analyses include candidate-pool overlap, final-panel overlap, cross-representation correlation, one-to-one correlation-adjusted matching, training-to-validation preservation, null simulations, and fixed-model predictive comparison.

3. **TCGA-LGG patient-resampling stability**  
   New NSGA-II searches are performed on repeated stratified 80% subsamples of the original LGG training cohort while holding the candidate gene universe, evolutionary seed, fitness classifier, and optimization settings constant. Twenty publication-level resampling runs are used to evaluate the effect of patient composition on selected gene panels and downstream predictive performance.

### Reproducibility and Validation

The workflow was designed to separate feature discovery from evaluation. Correlation-based matching and network construction are derived from training data, after which the same gene relationships are evaluated in untouched held-out patients without rematching.

Random-panel null experiments are used to determine whether observed similarity exceeds what would be expected by chance. For the LGG multiseed analysis, the null framework also reproduces the complete five-panel experimental design to evaluate whether observed network and component structure is stronger than expected from random 50-gene panels.

Predictive reproducibility is evaluated using a fixed histogram gradient boosting classifier so that performance differences reflect the selected feature panels rather than changes in model architecture.

### Computational Design

The notebook is configured for a **Google Colab front end connected to a Windows local runtime** and supports controlled multicore processing for computationally intensive operations.

The patient-resampling experiment uses restartable NSGA-II execution with serialized checkpoints, allowing interrupted publication runs to resume from the most recent valid generation rather than restarting from the beginning. Completed runs are automatically detected and skipped.

### Outputs

The workflow generates manuscript-ready outputs including:

- feature stability summary tables;
- exact and correlation-adjusted similarity measures;
- Nogueira stability estimates;
- chance-corrected stability results;
- null-model distributions;
- cross-seed gene correlation networks;
- connected-component summaries;
- bootstrap robustness results;
- held-out preservation analyses;
- predictive reproducibility metrics;
- patient-resampling stability analyses;
- gene-recurrence tables;
- CSV result files; and
- publication-quality figures.

All generated figures are additionally consolidated into a top-level `FIGURES` directory with a manifest linking each figure to its original analysis folder.

### Study Objective

The broader objective of this work is to distinguish **gene-level instability** from **biological and predictive instability**. In high dimensional transcriptomic data, multiple correlated genes may encode similar molecular information. Consequently, different NSGA-II runs may select different individual genes while still identifying reproducible molecular structure and maintaining similar predictive performance.

This repository provides a reproducible framework for evaluating that distinction across evolutionary randomness, RNA-seq analytical representation, and patient-sampling perturbation.
