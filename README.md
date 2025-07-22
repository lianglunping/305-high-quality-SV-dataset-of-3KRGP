1. Overview
This repository collects R scripts and shell launchers for genomic‑selection (GS) analysis based on the BWGS package.
Three marker strategies are supported:

Folder	Description
SNP/	SNP‑only workflows
SV/	SV‑only workflows (raw / genic / intergenic subsets)
Both/	Combined SNP + SV workflows

Each sub‑folder contains:

One or more core R scripts (BWGS_with_*.R) that run 5‑fold × 10‑repeat cross‑validation on up to ten GS models (GBLUP, RF, EGBLUP, RR, BRR, LASSO, EN, BL, BB, RKHS).

Shell wrappers (run_*.sh) that set the working directory and call the corresponding R script for a specific GWAS filtering threshold (p < 0.05 / 0.01 / 0.001 or “whole” set).

2. Directory Structure
bash
复制
编辑
BWGS/
├── Both/
│   ├── BWGS_with_SV_and_SNP.R
│   ├── BWGS_with_SV_and_SNP_glm_0.01.R
│   ├── BWGS_with_SV_and_SNP_glm_0.05.R
│   ├── BWGS_with_SV_and_SNP_glm_0.001.R
│   ├── run_305_whole.sh                 # no GWAS filter
│   ├── run_305_glm_filter_0.01.sh       # p < 0.01
│   ├── run_305_glm_filter_0.05.sh       # p < 0.05
│   └── run_305_glm_filter_0.001.sh      # p < 0.001
├── SV/
│   ├── BWGS_with_SV.R                   # all SVs
│   ├── BWGS_with_glm_filtered_SV_0.01.R
│   ├── BWGS_with_glm_filtered_SV_0.05.R
│   ├── BWGS_with_glm_filtered_SV_0.001.R
│   ├── BWGS_with_glm_filtered_SV_0.01_intergenetic.R
│   ├── run_305_glm_filter_0.01.sh
│   ├── run_305_glm_filter_0.05.sh
│   ├── run_305_glm_filter_0.001.sh
│   ├── run_305_glm_intergenic_sv_filter_0.01.sh
│   └── run_305_glm_genic_sv_filter_0.01.sh
├── SNP/
│   ├── BWGS_with_SNP.R                  # all SNPs
│   ├── BWGS_with_glm_filtered_SNP_0.01_predicted.R
│   ├── BWGS_with_SV_and_SNP_0.01.R      # optional mixed run
│   ├── BWGS_with_SV_and_SNP_0.05.R
│   ├── BWGS_with_SV_and_SNP_0.001.R
│   ├── run_305_filter_0.01.sh
│   ├── run_305_filter_0.05.sh
│   ├── run_305_filter_0.001.sh
│   ├── run_305_mlm_filter_SNP_0.01_predicted.sh
│   ├── run_305.sh                       # full pipeline launcher
│   └── preprare.sh                      # helper for file preparation
└── (Auxiliary .DS_Store / __MACOSX files may be ignored)
3. Prerequisites
R ≥ 4.1 with packages:
pacman, bruceR, rrBLUP, BGLR, brnn, glmnet, e1071, randomForest, BWGS, argparse, data.table, dplyr, tidyr, purrr, GenomicRanges, rtracklayer
(install once with install.packages() or pacman::p_load()).

A Unix‑like shell (bash/zsh) to execute the run_*.sh scripts.

Input data placed under <workdir>/input/, typically:

305_SV_Geno.RData / 305_SNP_Geno.RData

305_phe.RData (phenotype matrix; sample IDs in first column)

Trait list TXT (trait.list.txt)

Pre‑computed GWAS result CSVs in input/SV_GLM/ or input/SNP_GLM/

4. Quick Start
# 1. Unzip and move into a workflow folder
unzip BWGS.zip
cd BWGS/SV

# 2. Edit run_305_glm_filter_0.01.sh to set WORK=/abs/path/to/your/project
nano run_305_glm_filter_0.01.sh   # or any editor

# 3. Launch the analysis
bash run_305_glm_filter_0.01.sh
Results appear in <workdir>/output/ as:

*_ModelName_ac.csv – CV results for each GS model

*_all_model_ac.csv – combined matrix

glm_filtered_sv_0.01_counts.csv – number of SVs kept per trait

5. Customisation
Modify BWGS_with_*.R scripts to change filters, heritability estimates, model list, or CV scheme.

If running on HPC, wrap each run_*.sh in your scheduler’s job script.

6. Contact
For questions please contact lunping.liang lunping98@gmail.com.

