# Gut bacteriome and mycobiome in glioblastoma

Analysis code for:

> Herz M, Schmid K, Ankenbrand M, et al. Exploring the gut-brain axis in glioblastoma: a prospective case-control study of the gut bacteriome and mycobiome.

The notebook reproduces all statistical analyses, tables and figures of the manuscript, based on 16S rRNA gene (genus level) and ITS (species level) sequencing of rectal swabs from 23 patients with newly diagnosed IDH-wildtype glioblastoma and 23 healthy controls.

## Contents

1. Import and filtering of the 16S and ITS datasets (samples with ≥ 1000 reads)
2. Clinical metadata, serum cytokines and glia markers; anonymization
3. Descriptive statistics (Table 1, Table 2)
4. Survival analysis: Kaplan-Meier estimate, Cox regression with model diagnostics and sensitivity analyses
5. Microbiome analyses for patients vs. controls and for patients by overall survival (split at the cohort median):
   alpha diversity (DivNet, `testDiversity`), beta diversity (Bray-Curtis, `testBetaDiversity`), differential abundance (radEmu)
6. Composite main figures (Figures 2–4) and supplementary figures (Figures S1–S3)

## Data availability

Raw sequencing data are available from the European Nucleotide Archive under accession PRJEB91343. The file `sample_mapping.csv` assigns each sequencing library to its study group and, for patients, to the overall survival group. The column participant corresponds to the sample alias in ENA (PRJEB91343); run aliases follow the pattern webin-reads-<participant>_<marker>. Libraries not listed were excluded during quality filtering (< 1,000 reads). Clinical metadata cannot be shared publicly for data protection reasons and are available from the corresponding author on reasonable request.

Expected input structure:

    data/
    ├── ITS/    taxonomy.vsearch, asv_table.merge.txt, samples.csv
    ├── 16S/    taxonomy.vsearch, asv_table.merge.txt, samples.csv
    ├── metadataGBM_v2.csv, Medikamente_v2.csv
    ├── Serumproben_umformatiert.csv, Microglia_Darm-Hirn.csv
    └── iba1_case_*.tif

ASV and taxonomy tables were generated with the `metabarcoding_pipeline` by Alexander Keller (https://github.com/chiras/metabarcoding_pipeline); the helper functions in the Setup section are taken from this pipeline.

## Requirements

R 4.4.1 with the IRkernel. CRAN packages are loaded in the Setup section; `conflicted` must also be installed. DivNet (v0.4.1) and radEmu (v2.2.1.0) were installed from GitHub.

    remotes::install_github("adw96/DivNet")
    remotes::install_github("statdivlab/radEmu")
    BiocManager::install("phyloseq", version = "3.19")

Exact package versions are listed in `sessionInfo.txt`.

## Notes

- All outputs are written to `output_glioblastoma/`, which is created on first run.
- DivNet and radEmu fits are computationally intensive; random seeds are set before each DivNet fit.
- The group labels used internally (`c`/`p`, `lowerOS`/`higherOS`) are mapped to display labels (Controls/Patients, Shorter OS/Longer OS) in the Setup section.

## License

MIT, see `LICENSE`.  `sample_mapping.csv` is released under CC BY 4.0.
Helper functions in the Setup section are adapted from metabarcoding_pipeline by Alexander Keller (MIT License).
