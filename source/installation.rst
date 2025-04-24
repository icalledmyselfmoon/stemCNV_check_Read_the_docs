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

**Install StemCNV-check**

**Clone the StemCNV-check git repository:**

.. code:: bash

   git clone https://github.com/bihealth/StemCNV-check.git



**Install dependencies**

   - for Linux ``conda install python=3.12``
   
   - for WSL ``conda install python=3.12 "gcc_linux-64<14" apptainer fuse-overlayfs``


**Set up conda environment only for StemCNV-check**

.. code:: bash

   conda create -n stemcnv-check python=3.12; conda activate stemcnv-check
   cd StemCNV-check   #change into the StemCNV-check directory
   pip install -e .   #install StemCNV-check and its dependencies with pip

 


## 1.1 (Windows only) Install WSL 

Please also consult the [official instructions](https://learn.microsoft.com/en-us/windows/wsl/install) for installing WSL, 
especially if you encounter any problems. 

In short:

 - Open PowerShell or Windows Command Prompt in administrator mode by right-clicking and selecting "Run as administrator" 
 - Enter: `wsl --install` 
 - Follow the installation instructions
   - You will (likely) be asked to set a username and password for the linux environment. Do remember those.
 
You can now start a linux environment using the WSL programm (ie. wsl.exe)

> *Important*: Please note that all following commands in these instructions should be executed in the WSL console (and not in i.e. the windows powershell).


## 1.2 Install conda

Conda is software that facilitates the distribution and installation of primarily scientific software with the ability 
to control which specific versions are used. StemCNV-check utilises this for almost all steps of the workflow and 
as such depends on a working conda setup. In principle any conda setup can be used, but for anyone not familiar 
we recommend the following: 

Install the [miniforge conda](https://github.com/conda-forge/miniforge). In short, use these commands: 
\footnotesize
```cmd
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
``` 
\normalsize

During the installation follow instructions and suggestions that are displayed in the terminal. When asked about updating 
the shell profile to the allow automatic initialisation of conda, do answer/type: `yes` instead of the default no, 
in all other cases the default answers should let you continue.

After successful installation of conda you either have to restart WSL (close and reopen the window) or reload the updated 
.bashrc of WSL: `source ~/.bashrc`.


## 1.3 Install StemCNV-check

> *Note*: we plan to release StemCNV-check via bioconda in the future, which will simplify this step

<!---
Future Todo:
`conda install -c bioconda stemcnv-check`
--->

Perform the following steps to install and setup up the development version of StemCNV-check:

- Clone the StemCNV-check git repository:  
  `git clone https://github.com/bihealth/StemCNV-check.git`
- Prepare the conda environment: `conda install python=3.12`
  - ${\color{orange}\text{Important for WSL users}}$: the development version assumes you have a 'full' linux version. 
    On WSL you will need additional dependencies, so use this command **instead** of the above command:  
    `conda install python=3.12 "gcc_linux-64<14" apptainer fuse-overlayfs`
<!---
Future Todo:
gcc14 and datrie have issues, can unpin gcc once those are fixed
https://github.com/pytries/datrie/issues/101
https://github.com/pytries/datrie/pull/99
--->
  - ${\color{orange}\text{Experienced users}}$ of conda, or those who use conda for other projects should prefer 
    to use a specific environment only for StemCNV-check:  
    `conda create -n stemcnv-check python=3.12; conda activate stemcnv-check`  
- Change into the StemCNV-check directory: `cd StemCNV-check`
- Install StemCNV-check and its dependencies with pip: `pip install -e .`

${\color{orange}\text{Updating StemCNV-check}}$  
As long as you are in the StemCNV-check directory you can update the development version of StemCNV-check with this 
command:  
`git pull; pip install -e .` 

<!---
Future Todo:
Instructions on how to make this executable from Windows?
--->







