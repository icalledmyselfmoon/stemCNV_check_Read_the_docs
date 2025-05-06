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
