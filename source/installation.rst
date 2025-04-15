Installation
============

Step-by-step instructions for setting up and running StemCNV-check on Windows.

Prerequisite: installation of WSL (Windows Subsystem for Linux) and Conda.

`wsl --install`

`wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"`
`bash Miniforge3-$(uname)-$(uname -m).sh`


Install StemCNV-check

• Clone the StemCNV-check git repository:

`git clone https://github.com/bihealth/StemCNV-check.git`

• Prepare the conda environment: `conda install python=3.12`
– Important for WSL users: the development version assumes you have a ‘full’ linux version. On WSL you
will need additional dependencies, so use this command instead of the above command:
`conda install python=3.12 "gcc_linux-64<14" apptainer fuse-overlayfs`
– Experienced users of conda, or those who use conda for other projects should prefer to use a specific
environment only for StemCNV-check:
`conda create -n stemcnv-check python=3.12; conda activate stemcnv-check`
• Change into the StemCNV-check directory: cd StemCNV-check
• Install StemCNV-check and its dependencies with pip: `pip install -e .`




