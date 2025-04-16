.. stemCNV-check documentation master file, created by
   sphinx-quickstart on Mon Apr 14 21:18:34 2025.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

stemCNV-check Manual
===========================

StemCNV-check is a tool written to simplify copy number variation (CNV) analysis of SNP array data, specifically for quality control of (pluripotent) stem cell lines. StemCNV-check uses snakemake to run the complete analysis from raw data (.idat) up report generation for all defined samples with a single command. Samples need to be defined in a (tabular) sample table and the workflow settings are defined through a yaml file.

**Requirements:**

- linux environment (or WSL on windows) 
- working conda (or mamba) 

**stemCNV-check functionalities:**

- quality control of hPSC genetic integrity based on CNV detection in SNP-array data 
- detection of loss of heterozygosity
- detection of CNVs/SNPsresponsible for changes in amino acid sequence
- identification of hPSC line and detection of swaps or cross-contamination based on comparison to reference samples,  analysis based on samples SNP distance which indicates hPSC line identity. This allows sample identification and detection of swaps  or cross-contamination 

- evaluation of SNPs in coding regions of interest, detect  on/off target genomic changes generated after genetic engineering procedures.

.. toctree::
    :maxdepth: 2
    :caption: Contents:

.. toctree::
    :maxdepth: 2
    :caption: Installation
    :name: intro
    :hidden:

    installation
    
