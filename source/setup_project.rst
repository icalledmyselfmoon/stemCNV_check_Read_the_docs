Project setup
============


**Setting up our own data for analysis with StemCNV-check requires:**

- manifest and static files 
- sample table

- config file

**Manifest and static files**

These are array specific files sets matching your specific array platform and version.
At least one, but better both manifest files (‘.bpm’ and ‘.csv’, the latter can be omitted), matching your desired Genome assembly
(hg19/GRCh37 or hg38/GRCh38), and the clusterfiler (‘.egt). Illumina manifest files for hg19 usually end with ’A1’,
while those for hg38 end with ‘A2’. The cluster file is not specific to a genome build.
These files are generally provided as downloads by Illumina. However, for certain arrays with customisable probe
content you may need to contact the facility running your arrays to obtain manifest files, that allow utilisation of all
probes. Files for the popular Infinium Global Screening Array can for example be obtained from this Illumina website.

**Fill in the sample table with your data**

Empty example files for the sample table and config can be created with this command:
``stemcnv-check setup-files``

You will need to fill in the sample table with your own data.
Sample table lists the samples to be analyzed and their properties. It is recommended to keep all samples of one project in a single table.
								
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
															
**Adjust the config file**

adjust the config file so that all entries marked as
``“#REQUIRED”`` are filled in.

The config file created by this command will only include the absolute necessary settings to run the workflow. If
you are interested in setting additional parameters or changing the content of the report, you can add this flag
--config-details medium to the command (also available with ‘advanced’ or ‘complete’ instead of ‘medium’).
