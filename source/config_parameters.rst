Config file options
============

Setting analysis parameters or changing them requires editing the text in  generated default config file. Start by opening the config.yaml in text editor. Then type in or change the necessary parameters


**Array definition**
**genome_version options:**
-hg38 or GRCh38
-hg19 or GRCh37


Config file
-----------
The config file (default: config.yaml) defines all settings for the analysis and inherits from the inbuilt default, as 
well as system-wide array definitions if those exist. While most of the settings can be left on default, the input files 
need to be defined. Among those are also the files for the definition of the array platform, which are the primary 
required settings apart from raw data locations, that can not have defined defaults in the config file created by the 
setup-files command. The array definition needed for StemCNV-check includes the following files:

The default config file (config.yaml) defines all settings for the analysis and inherits from the inbuilt default.

*Adjust the config file* so that all entries marked as
``“#REQUIRED”`` are filled in.

**Define  files specific to the used array platform and genome build:**

- **egt_cluster_file**: the illumina cluster file (.egt) for the array platform, available from Illumina or the provider running the array

- **bpm_manifest_file**: the beadpool manifest file (.bpm) for the array platform, available from Illumina or the provider running the array
- **csv_manifest_file** (optional): the manifest file in csv format, available from Illumina or the provider running the array


The file paths for these files need to be entered in the config under the 'array_definition' section. In this section 
you also need to give your array a name (that needs to match the 'Array_Name' column in the sample table) and define a 
genome version (hg19 or hg38). Please note that the Illumina bpm and csv manifest files are also specific to a certain 
genome version, usually files for hg19 end in 'A1' and those for hg38 end in 'A2' (the egt cluster file is not specific 
and can be used for any genome version).  
Other array specific files mentioned in the config can be auto-generated (see next step below).

**The config file needs to define the following paths:**

- **raw_data_folder**: path to the input directory under which the raw data (.idat) can be found. Ths folder should contain subfolders that match the Chip_Name column in the sample table (containing the array chip IDs)

- **data_path**: the output of StemCNV-check will be written to this path
- **log_path**: the log files of StemCNV-check will be written to this path

**Advanced options**

The config file created by this command will only include the absolute necessary settings to run the workflow. If
you are interested in setting additional parameters or changing the content of the report, you can add this flag
--config-details medium to the command (also available with ‘advanced’ or ‘complete’ instead of ‘medium’)


Sample table
-----------

The sample table (default: sample_table.tsv) is a tab-separated file describing all samples to be analyzed. Excel or tsv formats are supported.
Empty example files for the sample table and config can be created with this command:

``stemcnv-check setup-files``

If you prefer to use an xlsx file here you can create an example by using:  

``stemcnv-check setup-files --sampletable-format xlsx``

**Fill in the sample table with your data**

- Required columns: Sample_ID, Chip_Name, Chip_Pos, Array_Name, Sex, Reference_Sample
- Optional (reserved) columns: Regions_of_Interest

You can also use your own Excel file, if the following criteria are met:

- The actual sampletable is in the first sheet of the file and this sheet *only* contains columns for the sample table 
  (optionally with commented lines starting with a '#')

- All required columns are present and correctly named (the order of columns is not important)
  - It is possible to deviate from the standard column names, but the expected column names need be contained in the acutal 
    column names and there needs to a singular way to extract them (via regex). 
  - In this case you need to use the ``--column-remove-regex`` option to tell the pipeline how to modify your column names 
    to derive the expected names. If used without an explicit regex (for expert users) spaces and anything following 
    them will be removed from your column names.

  - A simple example with ``--column-remove-regex`` (default) option would be to use i.e:  
    'Sample_ID for pipeline', 'Chip_Name (Sentrix Barcode)', 'Chip_Pos (Sentrix Position)'

								
.. list-table::  Example Sample table
   :widths: 15 15 10 10 10 10 10 10 10 
   :header-rows: 1
								
   * - Sample_ID 
     - Chip_Name
     - Chip_Pos
     - Array_Name
     - Sex
     - Reference_Sample
     - Regions_of_Interest
     - Sample_Group
     - Coriell_ID
   * - HG001
     - 207521920117
     - R09C02
     - ExampleArray
     - female
     -
     -
     - 
     - NA12878
   * - HG002
     - 207521920117
     - R05C02
     - ExampleArray
     - male
     -
     -
     - 
     - NA24385
   * - HG004
     - 207521920117
     - R07C02
     - ExampleArray
     - female				
     -
     -
     - 
     - NA24143
   * - HG005
     - 207521920117
     - R01C02
     - ExampleArray
     - male
     -
     -
     - HG006
     - NA24631


**Extended sample table. Description of the data types contained in the columns.**

- Sample_ID
Include bank ID when possible, only: - or _, do not use special characters: (), {}, /, \, ~,*, & Name has to be UNIQUE.
This column has auto-formatting enabled, so that the IDs will work with the CNV-pipeline:
	- red entries are either duplicate or contain not-allowed characters (/ and .\)
	- orange entries contain characters that the pipeline will remove (since they can cause issues if used in file names):  :,;()[]{}!?* and <space>

- Line family (iPSC line names without the clone part)	
- DNA ID/ Barcode (CORE)	
- Gender	
- Passage	
- Gene edited (yes/no)	
- Passages after editing	
- Type of editing	
- `Modification <https://scc-docs.charite.de/openkm/kcenter/#/browser/uuid/6f505d68-4e61-4f2d-a46d-4ad434ea94d5>`_ . Check Gene Editing Overview table to input correct modification
- Chromosome	
- ROI for StemCNV-Check	
- Bank	(Only use: MBXX WBXX seed primary)
- Cell type (iPSC/reference)
- latest parental CONTROL sample (patient cells or preceeding Bank MB/WB/Seed). If it is not 'reference' then sample name chosen for this column MUST exist in the first column
- earliest parental CONTROL (patient cells or MB). If it is not 'reference' then sample name chosen for this column MUST exist in the first column
- AG (resp user)	
- Service request ID openIRIS	
- Responsible person (CORE)	
- Batch group	
- Additional references (e.g. for dendrogram). This column works the same as the "Parental Control" one, except that you can add multiple references separated by commas (in the same field). Excel can not do conditional formatting for that.
- Send to L&B (date)	
- Data received (date)	
- Sample_Name (L&B)	
- Chip/Sentrix Barcode (L&B)	
- SentrixPosition (L&B)	
- Chip Type (L&B)	
- Manifest Version	
- Pass/fail (Use pass/fail ONLY for non-reference samples!!)
- Analysis by	


