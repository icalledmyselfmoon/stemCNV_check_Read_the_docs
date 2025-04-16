Installation
============

Step-by-step instructions for setting up and running StemCNV-check on Windows. 

**WSL and Conda**

**Installation of WSL (Windows Subsystem for Linux)**

.. code:: bash

    wsl --install

**Installation of Conda**

.. code:: bash

    wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
    bash Miniforge3-$(uname)-$(uname -m).sh

**Install StemCNV-check and prepare the conda environment**

.. code:: bash
   #Clone the StemCNV-check git repository:
   git clone https://github.com/bihealth/StemCNV-check.git



**Install dependencies**

   - for Linux

    ``conda install python=3.12``
   
   - for WSL

    ``conda install python=3.12 "gcc_linux-64<14" apptainer fuse-overlayfs``


**Set up conda environment only for StemCNV-check**

.. code:: bash

   conda create -n stemcnv-check python=3.12; conda activate stemcnv-check
   cd StemCNV-check   #change into the StemCNV-check directory
   pip install -e .   #install StemCNV-check and its dependencies with pip

 






