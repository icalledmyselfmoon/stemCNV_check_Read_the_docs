Project setup
============

Setting up our own data for analysis with StemCNV-check follows similar steps to the example data.
You will need to acquire array specific files sets matching your specific array platform and version: at least one, but
better both manifest files (‘.bpm’ and ‘.csv’, the latter can be omitted), matching your desired Genome assmebly
(hg19/GRCh37 or hg38/GRCh38), and the clusterfiler (‘.egt). Illumina manifest files for hg19 usually end with ’A1’,
while those for hg38 end with ‘A2’. The cluster file is not specific to a genome build.
These files are generally provided as downloads by Illumina. However, for certain arrays with customisable probe
content you may need to contact the facility running your arrays to obtain manifest files, that allow utilisation of all
probes.
Files for the popular Infinium Global Screening Array can for example be obtained from this Illumina website.
Both the manifest and cluster files and other ‘static’ options like the genome version are defined in the array_definition
section of the config file for StemCNV-check. Apart from the manifest and static files and a few base options for
input and output file paths no changes to the default config are necessary to run StemCNV-check. However, the
config file can be used to set and customise nearly any part of the workflow, including analysis settings and the
content of the report.
In addition to the config, every run of StemCNV-check requires a sample table, which list the samples to be analyzed
and their properties. It is recommended to keep all samples of one project in a single table.
Empty example files for the sample table and config can be created with this command: stemcnv-check setup-files .
You will need to fill in the sample table with your own data and adjust the config file so that all entries marked as
“#REQUIRED” are filled in.
The config file created by this command will only include the absolute necessary settings to run the workflow. If
you are interested in setting additional parameters or changing the content of the report, you can add this flag
--config-details medium to the command (also available with ‘advanced’ or ‘complete’ instead of ‘medium’).