# proteomics-fdr-and-quant-workflow



### 1. Peptide Identification & Quality Control
**Script Name:** `calculate_fdr.R`
This script handles the statistical validation of Comet search results.
*   **Key Functions:**
    *   Loads the Comet results (tab-separated text).
    *   Sorts Peptide-Spectrum Matches (PSMs) by **e-value** (low to high).
    *   Identifies "decoy" hits (looking for the "DECOY" string in protein IDs).
    *   Calculates the running **False Discovery Rate (FDR)** using the formula: $FDR = \frac{FP}{TP + FP}$.
    *   **Output:** A "True Positives vs FDR" plot to visually justify your 1% FDR cutoff.

### 2. Data Cleaning & Normalisation
**Script Name:** `normalize_tmt_data.R` (Source Reference: `TAKE_THE_NORM_FIRST.R`)
This script prepares the raw reporter ion intensities for analysis.
*   **Key Functions:**
    *   **Zero Handling:** Replaces 0 values with 0.1 to allow for valid log transformation.
    *   **Log Transformation:** Applies log transformation to smooth the data distribution.
    *   **Median Normalisation:** Calculates the median of a reference channel (e.g., S1_36_A) and adjusts all other channels (Mock, S2) to remove technical variance (pipetting errors).
    *   **QC Plots:** Generates **Box Plots** comparing data distribution before and after normalisation to demonstrate the removal of systematic bias.

### 3. Differential Expression Analysis
**Script Name:** `differential_expression.R` (Source Reference: `life_754_proteomics_t_test...`)
This script identifies significantly changing phosphopeptides between infected and mock samples.
*   **Key Functions:**
    *   **Filtering:** Subsets the data to include only peptides with "Phospho" modifications.
    *   **Fold Change:** Calculates Log2 Fold Change (Log2FC) for SARS-CoV-1/2 vs Mock.
    *   **Hypothesis Testing:** Runs **unpaired t-tests** for each peptide (3 replicates vs 3 replicates).
    *   **Correction:** Applies **Benjamini-Hochberg (BH)** adjustment to p-values to control the FDR at the quantitative level.

### 4. Visualisation
**Script Name:** `generate_volcano_plots.R`
*   **Key Functions:**
    *   Uses `ggplot2` to plot Log2 Fold Change vs -Log10 p-value.
    *   Adds colour coding: Red for significant upregulation, Blue for significant downregulation (based on p < 0.05).

### 5. Validation Assets (Non-Script)
**Folder:** `validation_images/`
*   **Lorikeet screenshots** here.
*   Shows annotated **b-ions and y-ions** that confirm the site localisation of your specific phosphopeptide (e.g., verifying a specific Serine phosphorylation over a nearby Threonine).

***
