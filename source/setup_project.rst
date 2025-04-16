Project setup
============


**Setting up our own data for analysis with StemCNV-check requires:**

- config file
- manifest and static files (defined by the user in the config)
- sample table



Config file
-----------

The default config file (config.yaml) defines all settings for the analysis and inherits from the inbuilt default.

*Adjust the config file*

adjust the config file so that all entries marked as
``“#REQUIRED”`` are filled in.

**Define  files specific to the used array platform and genome build:**

- egt_cluster_file: the illumina cluster file (.egt) for the array platform, available from Illumina or the provider running the array

- bpm_manifest_file: the beadpool manifest file (.bpm) for the array platform, available from Illumina or the provider running the array
- csv_manifest_file (optional): the manifest file in csv format, available from Illumina or the provider running the array

**The config file needs to define the following paths:**

- raw_data_folder: path to the input directory under which the raw data (.idat) can be found. Ths folder should contain subfolders that match the Chip_Name column in the sample table (containing the array chip IDs)

- data_path: the output of StemCNV-check will be written to this path
- log_path: the log files of StemCNV-check will be written to this path

**Advanced options**

The config file created by this command will only include the absolute necessary settings to run the workflow. If
you are interested in setting additional parameters or changing the content of the report, you can add this flag
--config-details medium to the command (also available with ‘advanced’ or ‘complete’ instead of ‘medium’).





Sample table
-----------

Empty example files for the sample table and config can be created with this command:
``stemcnv-check setup-files``

**Fill in the sample table with your data**

and their properties. It is recommended to keep all samples of one project in a single table.
The sample table (default: sample_table.tsv) is a tab-separated file describing all samples to be analyzed:

- Required columns: Sample_ID, Chip_Name, Chip_Pos, Array_Name, Sex, Reference_Sample
- Optional (reserved) columns: Regions_of_Interest

								
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
   * - HG006
     - 207521920117
     - R03C02
     - ExampleArray
     - male
     -
     -
     - 
     - NA24694
   * - HG007
     - 207521920117
     - R11C02
     - ExampleArray
     - female
     -
     -
     - 
     - NA24695

