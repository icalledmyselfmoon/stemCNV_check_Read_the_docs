Project setup
============


**Setting up our own data for analysis with StemCNV-check requires:**

- manifest and static files
- sample table

- config file

Array specific files sets matching your specific array platform and version: at least one, but
better both manifest files (‘.bpm’ and ‘.csv’, the latter can be omitted), matching your desired Genome assembly
(hg19/GRCh37 or hg38/GRCh38), and the clusterfiler (‘.egt). Illumina manifest files for hg19 usually end with ’A1’,
while those for hg38 end with ‘A2’. The cluster file is not specific to a genome build.
These files are generally provided as downloads by Illumina. However, for certain arrays with customisable probe
content you may need to contact the facility running your arrays to obtain manifest files, that allow utilisation of all
probes. Files for the popular Infinium Global Screening Array can for example be obtained from this Illumina website.


Empty example files for the sample table and config can be created with this command:
``stemcnv-check setup-files``

You will need to fill in the sample table with your own data and adjust the config file so that all entries marked as
“#REQUIRED” are filled in.

Sample table lists the samples to be analyzed and their properties. It is recommended to keep all samples of one project in a single table.

The config file created by this command will only include the absolute necessary settings to run the workflow. If
you are interested in setting additional parameters or changing the content of the report, you can add this flag
--config-details medium to the command (also available with ‘advanced’ or ‘complete’ instead of ‘medium’).
