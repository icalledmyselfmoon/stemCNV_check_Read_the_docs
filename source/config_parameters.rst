Config file options
============

Setting analysis parameters or changing them requires editing the text in  generated default config file. Start by opening the config.yaml in text editor. Then type in or change the necessary parameters. 

The default config file (config.yaml) defines all settings for the analysis and inherits from the inbuilt default.

Adjust the config file so that all entries marked as ``“#REQUIRED”`` are filled in.


**Array_definition**

Multiple arrays can be defined here, but arrays defined in global config saved in the cache are also available. The config file will take precedence over the global config, unless the file names here can not be used. Each array needs all required entries, but the `stemcnv-check make-staticdata` command will generate files marked as auto-generatable. By default both the files and an update to a global array definition file will be written into the cache directory (unless --no-cache is used). By default this file is at  ~/.cache/stemcnv-check/global_array_definitions.yaml

Once the array definitions are in the global file, you need to either delete the 'array_definition' block here or also update it with the information written out by `stemcnv-check make-staticdata` (which is the same as the entry written into the global array definition config), since this config takes precedence over the global file.

If no global config was used during the `make-staticdata` run, i.e. due to the --no-cache flag the array definitions will instead be written to a local file, i.e. 'ExampleArray_config.yaml' in the current working directory. In this case you will need to copy the contents of that file into this one, or alternatively into a global array definition file, that can still be created.


The config file (default: config.yaml) defines all settings for the analysis and inherits from the inbuilt default, as 
well as system-wide array definitions if those exist. While most of the settings can be left on default, the input files 
need to be defined. The file paths for these files need to be entered in the config under the 'array_definition' section. In this section 
you also need to give your array a name (that needs to match the 'Array_Name' column in the sample table) and define a 
genome version (hg19 or hg38). Please note that the Illumina bpm and csv manifest files are also specific to a certain 
genome version, usually files for hg19 end in 'A1' and those for hg38 end in 'A2' (the egt cluster file is not specific 
and can be used for any genome version).  
Other array specific files mentioned in the config can be auto-generated (see next step below).


**genome_version options:** hg38/GRCh38 or hg19/GRCh37

- **egt_cluster_file**: the illumina cluster file (.egt) for the array platform, available from Illumina or the provider running the array

- **bpm_manifest_file**: the beadpool manifest file (.bpm) for the array platform, available from Illumina or the provider running the array
- **csv_manifest_file** (optional): the manifest file in csv format, available from Illumina or the provider running the array

- **raw_data_folder**: path to the input directory under which the raw data (.idat) can be found. Ths folder should contain subfolders that match the Chip_Name column in the sample table (containing the array chip IDs)

- **data_path**: the output of StemCNV-check will be written to this path
- **log_path**: the log files of StemCNV-check will be written to this path

**Advanced options**

The config file created by this command will only include the absolute necessary settings to run the workflow. If
you are interested in setting additional parameters or changing the content of the report, you can add this flag
--config-details medium to the command (also available with ‘advanced’ or ‘complete’ instead of ‘medium’)


**Specification of labels (and their report colors) assigned to sample level QC measures**
sample_labels:
    OK: green
    unusual: yellow
    warning: orange
    high concern: red

# Default labels for CNVs (more can be added by users)
CNV_labels:
    # This is used to count critical CNVs & LOHs
    - Critical de-novo
    # This is used to count reportable CNVs & LOHs
    - Reportable de-novo
    - de-novo call
    - Reference genotype
    - Excluded call

# possible labels for SNVs
SNV_labels:
    - critical
    - reportable
    - unreliable impact
    - de-novo SNV
    - reference genotype

**Label for CNVs merged from multiple callers**

combined_cnvs: 'combined-call'

**The following lists are primarily used by the check_config functions**

Possible/Defined FILTERs applied to CNV calls (vcf style)

vcf_filters:
    - probe_gap
    - high_probe_dens 
    - min_size 
    - min_probes
    - min_density

**Possible/Defined categories for SNVs, each category can be assigned critical or reportable**
SNV_category_labels:
    - ROI-overlap
    - hotspot-match
    - hotspot-gene
    - protein-ablation
    - protein-changing
    - other

**Possible/Defined QC measures on sample level**
sample_qc_measures:
    - call_rate
    - computed_gender
    - SNPs_post_filter
    - SNP_pairwise_distance_to_reference
    - loss_gain_log2ratio
    - total_calls_CNV
    - total_calls_LOH
    - reportable_calls_CNV
    - reportable_calls_LOH
    - reportable_SNVs
    - critical_calls_CNV
    - critical_calls_LOH
    - critical_SNVs
  
**Possible/Defined report sections**
report_sections:
  - sample.information
  - QC.summary
  - QC.GenCall
  - QC.PennCNV
  - QC.CBS
  - QC.settings
  - SNV.table
  - SNV.hotspot.coverage
  - SNV.QC.details
  - denovo_calls.table
  - denovo_calls.plots
  - reference_gt_calls.table
  - reference_gt_calls.plots
  - regions.of.interest
  - SNP.dendrogram
  - genome.overview

**Possible/Defined subsections in the CNV plot sections**
report_plotsections:
  - denovo
  - reference_gt
  - regions_of_interest


Probe filtering
 --------------

Filter settings are defined in the StemCNV-check config
 StemCNV-check allows the definition of probe filter settings including:
(1) handling of probes at the same position (keep all, fully remove, keep only one probe with highest GC or GT),
(2) handling probes in the pseudo-autosomal regions (keep all, remove all, remove only from male samples),
(3) minimum score for both GenCall and (4) GenTrain scores.  

recommended default settings for probe filtering in StemCNV-check 
- minimum GC and GT scores of 0.15
- keeping only highest GC probes from duplicate positions 
- removing probes in the pseudo-autosmal (PAR1, PAR2) and X-translocated (XTR) region from male samples, probes on the Y chromosome are always filtered for samples annotated as female. 

.. list-table::  Filters 
   :widths: 15 15 
   :header-rows: 1
								
   * - Basic filters (minimal filters) 
     - min_size
     - min_probes
     - min_density

   * - “Extended” filters
     - high_probe_dens
     - probe_gap


Minimum call size (Mb/kb) - smallest region of copy number variation (CNV) that an array can reliably detect 
Minimum probes - minimum number of probes that need to be involved in a CNV call to ensure reliability.
some platforms require a 5-probe call for CNV detection, others 25
Probe density  number of probes per Mb or kb. 
- high probe density - better resolution and the ability to detect smaller CNVs, mean spacing of SNP probes is 200 per Mb. 
minimum call size 1kb, minimum 5 probe positions, minium probe density <10SNP/Mb
Probe gap - gap in probe coverage. 
In general, CNV call regions with a probe density matching the central 95% of the array are much more often judged as true positives than those with higher or lower densities (~30% vs max 5%). Furthermore, CNV regions with very low densities are quite often annotated as having a noticeable gap in probe coverage, even if the rate of true positive call annotation is comparable between calls with or without annotated probe gaps.

Calls that do not meet minimal criteria are flagged with soft filters: 
CNV shorter than 1000bp - “min_size” filter flag
CNV ≤ 5 probes - “min_probes” filter flag
probe density  ≤  10 probes per 1Mb - “min_density” filter flag



