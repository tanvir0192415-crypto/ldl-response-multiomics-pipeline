# LDL Cholesterol Response Prediction: A Multi-Omics + Genetics Pipeline

**Simulated proof-of-concept pipeline** for predicting individual LDL cholesterol
response to dietary restriction, integrating genetic, clinical, and lifestyle data.
Built as a demonstration project aligned with the ERC Proof-of-Concept project
LDL-ACT (BSRC Alexander Fleming, Dimas Group).

> **Note on data:** This project uses simulated data (n=300 individuals, 200 SNPs),
> not real biological data from FastBio or any other cohort. The purpose is to
> demonstrate the full analytical workflow — QTL analysis, genetic risk scoring,
> and multi-omics-style predictive modelling — end-to-end, with ground-truth
> causal variants embedded to validate that the pipeline correctly recovers
> known signals.

## Pipeline

1. **Cohort simulation**: 300 individuals, 200 SNPs (MAF 0.05–0.5), clinical
   covariates (age, sex, BMI, baseline LDL), and an LDL-change outcome with
   5 embedded causal SNPs (true effects: -1 to -4 mg/dL per allele copy).
2. **QTL association analysis**: linear regression (`statsmodels`) of LDL change
   on genotype, adjusting for age, sex, BMI; Benjamini-Hochberg FDR correction
   across 200 tests.
3. **Genetic risk score (GRS)**: beta-weighted sum of top 20 associated SNPs.
4. **Predictive modelling**: Elastic Net and Random Forest regression, comparing
   clinical-only vs. clinical+GRS models on a held-out test set (25%).

## Key results

| Model | R² (test) | MAE |
|---|---|---|
| Clinical only | 0.298 | 5.92 |
| Clinical + GRS (Elastic Net) | **0.467** | **5.33** |
| Clinical + GRS (Random Forest) | 0.237 | 6.49 |

Adding a genetic risk score improved variance explained by **57% relative** to
clinical variables alone. With n=300, only the strongest causal SNP (true effect
-2.99 mg/dL/allele) survived FDR correction — a realistic illustration of the
power limitations of QTL studies at modest sample sizes, and a motivation for
polygenic/multi-omic approaches over single-SNP analysis. Elastic Net outperformed
Random Forest, consistent with expectations for small-n, low-feature-count
genomic prediction tasks.

## Relevance to LDL-ACT

This pipeline demonstrates the core analytical components described in the
LDL-ACT project: QTL/genetic association analysis, genetic risk score
construction, and machine learning-based prediction of individual cardiovascular
risk marker (LDL) response — built to be extended to real multi-omics data
(transcriptomic, proteomic, metabolomic, gut microbiome) as described in the
FastBio/LDL-ACT framework.

## Repository

`Genomics.ipynb` — full analysis notebook (cohort simulation, QTL analysis,
genetic risk score, predictive modelling, figures), runnable in Google Colab.
