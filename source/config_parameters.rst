First analysis
============
Before the first analysis sample table and config file need to be set up (see above). Unless otherwise specified, stemcnv-check defaults to look for a "sample_table.tsv" (or .xlsx) and "config.yaml" file.


Config file settings
============

Setting analysis parameters or changing them requires editing the text in the generated default config file. Start by opening the config.yaml in text editor. Then type in or change the necessary parameters. 

The default config file (config.yaml) defines all settings for the analysis and inherits from the inbuilt default.

Adjust the config file so that all entries marked as ``“#REQUIRED”`` are filled in.


**Array definition**
-------------

The config file (default: config.yaml) defines all settings for the analysis and inherits from the inbuilt default, as 
well as system-wide array definitions if those exist. While most of the settings can be left on default, the input files 
need to be defined. The file paths for these files need to be entered in the config under the 'array_definition' section. In this section 
you also need to give your array a name (that needs to match the 'Array_Name' column in the sample table) and define a 
genome version (hg19 or hg38). Please note that the Illumina bpm and csv manifest files are also specific to a certain 
genome version, usually files for hg19 end in 'A1' and those for hg38 end in 'A2' (the egt cluster file is not specific 
and can be used for any genome version).  
Other array specific files mentioned in the config can be auto-generated (see next step below).


- **'ExampleArray'** should to be renamed to the actual array name

- **genome_version options:** hg38/GRCh38 or hg19/GRCh37

- **egt_cluster_file**: the illumina cluster file (.egt) for the array platform, available from Illumina or the provider running the array

- **bpm_manifest_file**: the beadpool manifest file (.bpm) for the array platform, available from Illumina or the provider running the array
- **csv_manifest_file** (optional): the manifest file in csv format, available from Illumina or the provider running the array

- **raw_data_folder**: input folder, path to the input directory under which the raw data (.idat) can be found. Ths folder should contain subfolders that match the Chip_Name column in the sample table (containing the array chip IDs). **idat files should be grouped in a subfolder per array-chip (sentrix_name).**

- **data_path**: the output of StemCNV-check will be written to this path
- **log_path**:  output folder, stemcnv-check will write log filesthe log files of StemCNV-check to this path

.. code:: bash

   array_definition:
        GSAMD-24v3-0:
          genome_version: 'hg19'
          bpm_manifest_file: 'static-data/ExampleArray/GSAMD-24v3-0-EA_20034606_A1.bpm'  #REQUIRED
          csv_manifest_file: 'static-data/ExampleArray/GSAMD-24v3-0-EA_20034606_A1.csv.gz'
          egt_cluster_file: 'static-data/ExampleArray/GSAMD-24v3-0-EA_20034606_A1.csv.gz'
          #Optional (leave empty if not used)
          penncnv_GCmodel_file: 'static-data/ExampleArray/GSAMD-24v3-0-EA_20034606_A1.egt'
          array_density_file: static-data/ExampleArray/density_hg19_ExampleArray.bed
          array_gaps_file: static-data/ExampleArray/gaps_hg19_ExampleArray.bed
          penncnv_pfb_file: static-data/ExampleArray/PennCNV-PFB_hg19_ExampleArray.pfb
    
   raw_data_folder: ../RAW_DATA #REQUIRED
   data_path: data_scoring     #REQUIRED
   log_path: logs    #REQUIRED

   reports:
        StemCNV-check-report:
          file_type: 'html'


**Evaluation settings**
---------------

All CNV calls are given a label based on their check score, filters and reference match. The labels described here are always available, but can be changed or new labels can be added. If not other category fits (which should not occur with default settings), then the last defined "Exclude call" label will always be assigned.

  •  Possible values for the "not_allowed_vcf_filters" list are: **probe_gap, high_probe_dens, min_size, min_probes, min_density**

  

.. list-table::  CNV_call_labels
   :widths: 25 25 25 25  
   :header-rows: 1

   * - CNV_call_labels
     - minimum_check_score
     - not_allowed_vcf_filters
     - reference_match

   * - Critical de-novo
     - 55
     -  ['high_probe_dens', 'probe_gap', 'min_size', 'min_probes', 'min_density']
     - FALSE
   * - Reportable de-novo
     - 55
     -  ['min_size', 'min_probes', 'min_density']
     - FALSE
   * - de-novo call
     - 0
     - ['min_size', 'min_probes', 'min_density']
     - FALSE
   * - Reference genotype
     - 0
     - []
     - TRUE
   * - Excluded call
     - 0
     - []
     - FALSE


**Labelling system**

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


Sample table 
============


Required Columns are: Sample_ID, Chip_Name, Chip_Pos, Array_Name, Sex, Reference_Sample, Regions_of_Interest, Sample_Group
Any number of additional columns can be added to the sample table as well, unless referred to in the config they will be ignored.
Specific explanations for columns:
 - Sample_ID:
       The folder and samples names for samples are derived from this entry. All entries *must* be unique. 
       To prevent issues with filenames only alphanumeric characters (all letters and number) and the characters -_
       (dash and underscore) are allowed.
 - Chip_Name and Chip_Pos:
       These entries must match the Sentrix name (usually a 12 digit number) and position (usually R..C..) on the Illumina array
 - Array_Name
       The name of the array used for the sample. This needs to match one of the arrays defined in the config under `array_definition`
 - Sex
       The sex of the sample is needed for analysis and mandatory. Allowed: f[emale]/m[ale] (not case sensitive)
 - Reference_Sample
       This column should refer to the (exact) Sample_ID of reference sample (i.e. a parental fibroblast line or master bank)
      If there is no usable or applicable reference sample the entry should be empty
 - Regions_of_Interest
       Definition of regions for which plots are always generated in the report (i.e. gene edited sites)
       The syntax for regions of interest is `NAME|region`, the `NAME|` part is optional and mainly useful for 
       labeling or describing the region.
       The `{region}` part is mandatory and can be one of the following:
       1) Position, "chrN:start-end": `chrN` can be i.e. 'chr3' or just '3',
          start and end are coordinates (which are genome build specific!)
       2) Genomic band, i.e. "4q21.3": a cytogenetic band, both full bands (q21) and subbands (q21.3) are allowed
       3) Gene symbol, i.e. "TP53": The gene name (or symbol) needs to exactly match the reference annotation (UCSC gtf)
       Multiple regions for a single sample should all be in one column entry and be separated by a `;`
 - Sample_Group
       This column can be used for annotation samples is used by default to select samples for clustering by SNPs.  


Static files generation
============

This step takes place after the  sample data for that array is available, sample table and the config file have been set up.

**Array & genome-build specific static files** are automatic generated. 

.. code:: bash

   stemcnv-check make-staticdata [-s <sample_table>] [-c <config_file>]


*Notes:* This step will also include **download of fasta and gtf** file for the reference genome build.**
Array specific files and an updated array_definition block for the config will be written into the cache directory (default: '~/.cache/stemcnv-check'). However, you still need to update or remove the array_definition from your config.yaml file, otherwise the cached definitions and files will not be used.


