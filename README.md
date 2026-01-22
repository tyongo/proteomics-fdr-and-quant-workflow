# proteomics-fdr-and-quant-workflow

## Problem
Identified and quantified differentially expressed phosphopeptides in SARS-CoV-2 infected human cells to elucidate changes in host cell signaling pathways compared to mock infections.

## Methods
*   **Peptide Identification:** Performed MS/MS database searching using the **Comet** search engine against the Human UniProt FASTA database.
*   **Statistical Validation:** Implemented a **Target-Decoy** search strategy to calculate global False Discovery Rates (FDR), filtering for high-confidence Peptide-Spectrum Matches (PSMs) at <1% FDR.
*   **Data Pre-processing:** Handled missing values (0 replaced with 0.1) and applied Log2 transformation to **TMT (Tandem Mass Tag)** reporter ion intensities.
*   **Normalization:** Applied **Median Normalization** to correct for technical variance (e.g., pipetting error/loading bias) across multiplexed channels.
*   **Differential Expression:** Conducted unpaired t-tests comparing infected (S1/S2) vs. mock samples (n=3 replicates) with **Benjamini-Hochberg** correction for multiple testing.

## Results
*   **Output table:** `results/tables/significant_phosphopeptides.csv` (List of differentially expressed peptides with p-values and Fold Changes).
*   **Figure(s):** `results/figures/fdr_curve.png` (True Positives vs FDR) and `results/figures/normalization_boxplots.png` (QC of TMT intensity distributions).
*   **Validation:** `results/images/lorikeet_validation.png` (Visual confirmation of phosphorylation site localisation using b/y ions).

## Reproducibility
```bash
# Create environment with R and core dependencies
conda env create -f environment.yml
conda activate proteomics_r_env

# Run FDR calculation script
Rscript scripts/calculate_fdr.R --input data/comet_results.txt --output results/tables/fdr_results.csv

# Run Normalization and Differential Expression
Rscript scripts/differential_expression.R --input data/tmt_quant_data.csv
```

## Skills demonstrated
**Statistical Validation (FDR)**
Implemented Target-Decoy database searching to control Type I errors in high-throughput MS data.

**Quantitative Proteomics (TMT)**
Executed normalization pipelines for multiplexed isobaric tagging data to mitigate technical variance and ratio compression.

**R Programming & Visualization**
Automated data wrangling (dplyr) and generated publication-quality QC visualizations (ggplot2) including Volcano and Box plots.

## Next Improvements
*   Automate the pipeline using **Nextflow** for better scalability on HPC clusters.
*   Integrate **Python** (Pandas/Scikit-learn) for machine-learning-based classification of disease states.
*   Add automated pathway enrichment analysis (linking to KEGG/DAVID APIs) to contextulise significant hits.

***
