Output
============

StemCNV-check will produce the following output files for each sample, when run with default settings:

- ``data_path/{sample}/{sample}.annotated-SNP-data.{filter}-filter.vcf.gz``  
The filtered, processed and annotated SNP data of the array in vcf format

- ``data_path/{sample}/{sample}.CNV_calls.CBS.vcf.gz``  
  The CNV calls for the sample from the CBS (Circular Binary Segmentation) algorithm in vcf format
- ``data_path/{sample}/{sample}.CNV_calls.PennCNV.vcf.gz``  
  The CNV calls for the sample from the PennCNV caller, in vcf format
- ``data_path/{sample}/{sample}.CNV_calls.combined-annotated.vcf.gz``  
  The CNV calls processed, combined and annotated by StemCNV-check, in vcf format. 
  Annotation includes comparison against reference sample; gene annotation; hotspots for stem cells, cancer and dosage 
  sensitivity; call scoring; and call labelling (i.e. as Critical de-novo call).
- ``data_path/{sample}/extra_files``  
  Folder in which additional QC log files are stored.
- ``data_path/{sample}/{sample}.summary-stats.xlsx``  
  An Excel file with summary information for the sample. The first sheets contains quality summary statistics, including 
  array quality measures, number of CNV and LOH calls, the number of calls above Check_Score thresholds. The further 
  sheets have more details from individual CNV callers or sample comparisons.
- ``data_path/{sample}/{sample}.SNV-analysis.xlsx`` 
  An Excel file with the results from analysis on annotated SNVs (from the SNP probes). This includes a list of all SNVs 
  with an annotated impact on a gene (including the gene name and categorisation based on predicted impact, known hPSC 
  reference SNV hotspots, match and call reliability), coverage of known hotspots by the utilised array, a distance 
  matrix of the sample to other selected samples (based on config settings), and a chromosome based summary of where SNPs occur.
- ``data_path/{sample}/{sample}.StemCNV-check-report.html``; ... 
  Html report containing summary statistics, QC statistics, lists of CNV calls sorted by annotation score, 
  plots of most/all CNVs and sample comparison. The default 'StemCNV-check-report' only contains plots for the top20 
  calls or calls above a user defined Check_Score threshold. A fully self-contained report can easily be enabled in the config.yaml. 
  The content of either the default or any additional reports can also be fine-tuned through the config.yaml file.

- ``data_path/{sample}/{sample}.StemCNV-check-report-html_images``  
  Folder containing all images included in the html report. 

Furthermore, the following collated summary tables can be created. 
(Optionally with a date prefix, or as tsv instead of xlsx files):
- ``data_path/[YYYY-MM-DD_]summary-overview.{xlsx,tsv}`` 
  A table that contains the information of the sample wise "summary-stats", but combined for all samples.
  Additionally, more information derived from the sampletable columns can be included. This output is 
  included in the default 'complete' target.
- ``data_path/[YYYY-MM-DD_]combined-cnv-calls.{xlsx,tsv}``  
  A table that contains all CNV calls from all samples, that meet config defined filter criteria (By default: 
  everything except calls with a minimum probe/size/density flag). This output is *not* included in the default 
  'complete' target, but can be created with the 'collate-cnv-calls' target.



Config file options
============

Making changes in the config file allows to edit analysis settings.
 
- open the config.yaml in text editor 
- type in or change the necessary variables

Array definition 
--------------
**genome_version options:**
-hg38 or GRCh38
-hg19 or GRCh37


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

