# <span style="color:mediumblue">SampleSheet.py</span>
Create an Illumina® Sample Sheet, the comma-separated text document required by Illumina® sequencing systems to specify (1) sequencing parameters and (2) sample-barcode relationships. Customize [Header], [Reads], and [Data] sections; in particular, draw on up to 192 8-bp barcodes (96x i7 &amp; 96x i5 indices) to specify up to 9,216 sample-barcode relationships for multiplexed amplicon sequencing.
 <br/><br/> 
 
## <span style="color:blue">Table of contents</span>
* [Background](#background)
* [Features](#features)  
* [Requirements](#requirements)
* [Synopsis](#synopsis)
* [System setup](#system%20setup)
* [Code launch notes](#codelaunchnotes)
* [Operation notes](#operationnotes)
* [Input notes](#input%20notes)
* [Output notes](#output%20notes)
* [Visual summary of key script operations](#visual%20summary%20of%20key%20script%20operations)
* [Status](#status)
* [Contact](#contact)
  

## <span style="color:blue">Background</span>
<img src="SampleSheet_img/SampleSheet_thumbnail.png" align="left" width="600">  
Sequencing by synthesis (SBS) collects millions to billions of DNA sequence reads *en masse*. DNA templates from tens to thousands of independent sample sources can be barcoded, pooled, and sequenced on a common flow cell. Unique indices (barcode sequences) allow pooled reads to be assigned to cognate sample sources (demultiplexed).  
<br/><br/>

**This script automates creation of an Illumina® Sample Sheet, the comma-separated text document required by Illumina® sequencing systems to specify (1) sequencing parameters and (2) sample-barcode relationships.** With this script, a Sample Sheet with up to 9,216 sample-barcode relationships can be automatically generated in <1 second, following user entry of a single simplified list containing up to 96 sample prefixes assigned to an i7 index range and unique i5 index (each sample prefix to be expanded to up to 96 individual samples, suffixed by well ID (*i.e.*, A01-H12) of a 96-well plate).


## <span style="color:blue">Features</span>
* Automates data entry into 3 Illumina® Sample Sheet sections, based on command-line input provided by a user:
	* \[Header] (InvestigatorName, ProjectName)
	* \[Reads] (# of reads)
	* \[Data] (Sample ID, i7 index, i5 index)

## <span style="color:blue">Requirements</span>
* Python 3.7 or higher
* For CLI script (suggested): download & install PrettyTable (<a href="https://pypi.org/project/PrettyTable/">PyPi link</a> or <a href="https://github.com/jazzband/prettytable">GitHub link</a>)
  

## <span style="color:blue">Synopsis</span>
**This script returns a Sample Sheet file compatible with Illumina® sequencing platforms.** 

**Users are asked for the path to an output directory in which a Sample Sheet will be created, along with user-specific variables for Sample Sheet \[Header\], \[Reads\], and \[Data\] sections.**

**For [Data] relationships between sample names and i7+i5 indices, SampleSheet.py draws upon a set of 192 custom primers with unique 8-bp barcodes compatible with Illumina® sequencing platforms; these indices allow up to 9,216 samples to be arrayed in 96-well (or 384-well) format with unique barcodes for pooled sequencing.**

>(see 'Input notes' for details).
    
Note on index usage: In this script, each i7 index identifies an individual well within a 96-well plate format (each well is uniquely barcoded by a single i7 index), whereas a single i5 index defines all wells of a specific plate (up to 96 wells in a single plate are barcoded by a common i5 index).  Primer sequences (and indices used by SampleSheet.py) can be found in associated files, i7\_barcode\_primers.xls and i5\_barcode\_primers.xls.

For further usage details, please refer to the following manuscript:  
>*Ehmsen, Knuesel, Martinez, Asahina, Aridomi, Yamamoto (2020)*
    
Please cite usage as:  
>SampleSheet.py  
>*Ehmsen, Knuesel, Martinez, Asahina, Aridomi, Yamamoto (2020)*
 
## <span style="color:blue">System setup</span>
#### <span style="color:dodgerblue">1. Confirm that Python 3 and Jupyter Notebook are available on your system, or download & install</span>
##### Mac or Windows OS

**Python 3**  
Mac OS generally comes with Python pre-installed, but Windows OS does not.  Check on your system for the availability of Python version 3.7 or higher by following guidelines below:
 
- First open a console in Terminal (Mac OS) or Command Prompt/CMD (Windows OS), to access the command line interface.
 
-  Check to see which version of Python your OS counts as default by issuing the following command (here, `$` refers to your command-line prompt and is not a character to be typed):  
     
	`$ python --version` 
	 
 	- If the output reads `Python 3.7.3` or any version >=3.7, you are good to go and can proceed to **Jupyter Notebook**.
 
 	- If the output reads `Python 2.7.10` or anything below Python 3, this signifies that a Python version <3 is the default version, and you will need to check whether a Python version >=3.7 is available on your system.
 		- To check whether a Python version >=3.7 is available on your system, issue the following command:  
 
 			`$ python3 --version`  
 
 		- If the output finds a Python version >=3.7 (such as `Python 3.7.3`), you are good to go and can proceed to **Jupyter Notebook**.
		- If the output does *not* find a Python version >3.7, use one of the following two options to download and install Python version >=3.7 on your computer: 
  
 			1- *To install Python 3 alone (separately from Jupyter Notebook)*
 
 			[Python](https://www.python.org/downloads/) https://www.python.org/downloads/
 			- "Download the latest version for Mac OS X", and then follow installation guidelines and prompts when you double-click the downloaded package to complete installation.  
 
 			- Once you have downloaded and installed a Python 3 version >=3.7, double-check in your command-line that Python 3 can be found on your system by issuing the following command:  
 	
 				`$ python3 --version` (Mac OS)  
 				`$ python --version` (Windows OS)
  
 				The output should signify the Python version you just installed.  Proceed to **Jupyter Notebook**.
 
 			2- *To install Python 3 and Jupyter Notebook all at once* (**recommended**)
 
			[Anaconda (with Jupyter Notebook) Download & Installation](https://jupyter.readthedocs.io/en/latest/install/notebook-classic.html) https://jupyter.readthedocs.io/en/latest/install/notebook-classic.html
			https://www.anaconda.com/products/individual

 			- Download Anaconda with Python 3, and then follow installation guidelines and prompts when you double-click the downloaded package to complete installation.  
 
 			- Once you have downloaded and installed Python 3, double-check in your command-line that Python 3 can be found on your system by issuing the following command: 
  	
				`$ python3 --version` (Mac OS)  
				`$ python --version` (Windows OS)  

 			- Also double-check in your command-line that Jupyter Notebook can be found on your system by issuing the following command:  
 	
 				`$ which jupyter notebook`
 
			- If the output indicates that 'jupyter' is available in the path of your Python 3 installation (such as, `/Library/Frameworks/Python.framework/Versions/3.7/bin/jupyter`), you are good to go and can proceed to **2. Configure Python Virtual Environment for Jupyter Notebook**.  

**Jupyter Notebook**  
*Note, these steps are not required if you installed Anaconda with Jupyter Notebook as recommended above*.  Jupyter Notebook is *not* generally pre-installed on Mac OS.  To check whether Jupyter Notebook is available with Python 3 on your machine, issue the following command:
 
 	$ which jupyter notebook
 
If the output indicates that 'jupyter' is available in the path of your Python 3 installation (such as, `/Library/Frameworks/Python.framework/Versions/3.7/bin/jupyter`), you are good to go and can proceed to **2. Configure Python Virtual Environment for Jupyter Notebook**.  If instead you see an error message indicating that 'jupyter notebook' is not available, issue the following commands using the Python program installer (pip) to install Jupyter: 
  
 	$ pip3 install --upgrade pip  
	$ pip3 install jupyter


<br> 
#### <span style="color:dodgerblue">2. Download the SampleSheet repository (or program file subset) from GitHub</span>

SampleSheet.py can be accessed as a **Jupyter Notebook** or **Python program file** available at [YamamotoLabUCSF GitHub](https://github.com/YamamotoLabUCSF/SampleSheet) (https://github.com/YamamotoLabUCSF/SampleSheet).  Please note that access to the file through GitHub requires a personal GitHub account.  

* (a) **Create a personal GitHub account** (free)  
	* follow instructions available at [WikiHow: How to Create an Account on GitHub](https://www.wikihow.com/Create-an-Account-on-GitHub) https://www.wikihow.com/Create-an-Account-on-GitHub. 

* (b) **Navigate to the code repository for SampleSheet**
	* The code repository contains:
		* the **Jupyter Notebook** file (.ipynb)
		* the **Python program** file (.py)
		* **image files** associated with the Jupyter Notebook
		* a **requirements** file (SampleSheet_requirements.txt), used in the creation of a Python virtual environment to run SampleSheet (see **3. Configure Python Virtual Environment for Jupyter Notebook**, below)
		
* (c) **Download** or **clone** the repository to your personal computer:  

	**Download:**   
	* first click on the repository name	 to access the repository
	* then download the entire repository directory and its associated subdirectories and files (**green download icon, labeled "Code"**)
	* alternatively, download only the target files you need for your intended purposes 
	  * for example, download the **Jupyter Notebook** file (.ipynb), **image files directory**, and **requirements** file if you plan to use only the Jupyter Notebook

	**Clone:**   
	  
	* first click on the repository name	 to access the repository
	* then click on the right-hand arrow of the **green download icon, labeled "Code"**, to access the drop-down menu with **Clone** options (HTTPS or GitHub CLI).
	* If selecting the HTTPS option, copy the indicated URL and paste it at your command-line as an argument to the command 'git clone':  
	`$ git clone https://github.com/YamamotoLabUCSF/SampleSheet.git` 

* (d) **Choose a directory location** on your machine where you would like to store the downloaded or cloned repository and its files.  This can be any folder/directory location you like.  **Move the repository files from the directory into which they were downloaded or cloned, into this directory**, if you have not already downloaded or cloned the repository/files directly into your target directory.  

	*Example using command line code*  
	*(directory can be created and accessed using command line prompts)*   
	* For example, on Mac OS:  
	
		* To create an empty directory named 'SampleSheetCode', in the 'Documents' directory:*  
	`$ mkdir /Users/yourusername/Documents/SampleSheetCode`
		* To navigate to the directory named 'SampleSheet':	`$ cd /Users/yourusername/Documents/SampleSheetCode`

	* For example, on Windows OS:  
	 
		* To create an empty directory named 'SampleSheetCode', in the 'Documents' directory:  
	`$ mkdir C:\Documents\SampleSheet`
		* To navigate to the directory named 'SampleSheetCode':  
		`$ cd C:\Documents\SampleSheetCode`

* (e) With the SampleSheet repository files now present in this directory you've created and named, now navigate to the **SampleSheet** repository in the command line, by issuing the following command:  

	`$ cd /Users/yourusername/Documents/SampleSheetCode/SampleSheet` (Mac OS)  
	or  
	`$ cd C:\Documents\SampleSheetCode\SampleSheet` (Windows OS)
 
<br>
 
#### <span style="color:dodgerblue">3. Create a Python Virtual Environment for Jupyter Notebook</span>

You are now ready to install an **additional Python module** that SampleSheet.py requires for operation.  These Python modules can be installed from the Python Package Index repository ([PyPI](https://pypi.org/)) (https://pypi.org/) by individual download from the PyPI website and installation on your machine.  However, to simplify and streamline installation of these modules, we will create a Python **virtual environment** (self-contained 'directory' with all the Python modules needed to run SampleSheet.py), using the following approach:

1.  First, install the Python module **virtualenv** ([virtualenv](https://pypi.org/project/virtualenv/)) (https://pypi.org/project/virtualenv/), by issuing the following command at the command line: 
 
	`$ pip3 install virtualenv`  (Mac OS)  
	or  
	`$ pip install virtualenv`  (Windows OS)
	
	pip3 (Mac OS) or pip (Windows OS) is Python 3's installation manager, and as long as there is an internet connection available, pip3 (Mac OS) or pip (Windows OS) will access the specified module from PyPI (here, virtualenv) and install it for access by Python 3.

2. Next, choose a **directory location** on your machine where you would like to install the files associated with a virtual environment.  This can be any folder/directory location you like (for example, you may have a favorite directory where other Python virtual environments are stored).  Alternatively, simply create the Python virtual environment in the SampleSheet directory you created above (in section 2d).  At the command line, navigate to the location of this directory.  
	* For example, on Mac OS:  
		* To navigate to the directory named 'SampleSheetCode':	`$ cd /Users/yourusername/Documents/SampleSheetCode`

	* For example, on Windows OS:  

		* To navigate to the directory named 'SampleSheetCode':  
		`$ cd C:\Documents\SampleSheetCode`

3. With this directory set as your working location in the command line, now issue the following commands to **create a virtual environment**:  
	
	(a) Create a Python virtual environment named **SampleSheet\_env**, specifying that the environment will use Python 3:  
	`virtualenv -p python3 SampleSheet\_env` 
	
	(b) Activate SampleSheet\_env:   
	`source SampleSheet\_env/bin/activate`  (Mac OS)  
	or  
	`.\SampleSheet\_env\Scripts\activate`  (Windows OS) 	
	
	(c) You should now see that your command line prompt has changed to indicate that you are in the virtual environment, displaying something along the lines of:  
	`(SampleSheet\_env) $`
	
	(d) Now install the Python modules required by SampleSheet.py, using the requirements file named **SampleSheet_requirements.txt**, located in the SampleSheet repository:
	   
	`$ pip3 install -r SampleSheet/SampleSheet_requirements.txt` (Mac OS)   
	or  
	`$ pip install -r SampleSheet/SampleSheet_requirements.txt`  (Windows OS)  	
	
	(e) To check that the required SampleSheet.py Python modules were installed properly, now issue the follow command:
	
	`$ pip3 list`  (Mac OS)  
	or  
	`$ pip list`  (Windows OS)  	
	
	If the virtual environment was set up successfully, the output will read: 
	  
  **Package    Version**    
  pip         20.3.1  
  prettytable 0.7.2  
  setuptools  50.3.2  
  wheel       0.36.0    


4.  Finally, Jupyter Notebook needs to be made aware of the Python virtual environment you just created.  To accomplish this, issue the following commands:  

    (Mac OS)   
	```$ pip3 install ipykernel```   
	```$ python -m ipykernel install --name=SampleSheet\_env```  

    (Windows OS)   
	```$ pip install ipykernel```   
	```$ python -m ipykernel install --user --name SampleSheet\_env``` 


5.  You should now be ready to **access SampleSheet.py** in Jupyter Notebook or at the command line!  See **Using SampleSheet.py**.

6. Just for completeness, to **exit the Python virtual environment and return to your 'native' (default) environment**, simply issue the following command at the command line:

	``$ deactivate`` 
	
	To **re-enter the virtual environment** at any time in the future, you would use the command in 3b:
	
	`source SampleSheet\_env/bin/activate`  (Mac OS)  
	or  
	`.\SampleSheet\_env\Scripts\activate`  (Windows OS) 

7. Also, note that if you'd like **to remove the SampleSheet\_env at any time**, you can do so by issuing the following command:  

	```$ rm -rf SampleSheet\_env``` 

	This would delete the virtual environment from your machine. 



##<span style="color:blue">Code launch notes</span>  
Code is available as a Jupyter Notebook file (**SampleSheet.ipynb**) or as a Python program file (**SampleSheet.py**) for direct use, or pre-packaged with all dependencies as an Open Virtualization Format file for virtual machines (**Alleles\_and\_altered\_motifs.ovf**).  

#### <span style="color:dodgerblue">Jupyter Notebook or Python program file</span>

In **System Set-Up** above, you downloaded and installed Python 3, Jupyter Notebook, & the SampleSheet code repository, and created a Python virtual environment (SampleSheet\_env) containing the Python modules that SampleSheet.py needs in order to run.  To access SampleSheet.py (Jupyter Notebook or Python program file) for interactive work, proceed through guidelines indicated below.  

#### <span style="color:dodgerblue">Jupyter Notebook</span>

*Note*, Jupyter Notebook file requires *SampleSheet_img* directory containing five image files to be available in the directory from which the Jupyter Notebook will be opened.  

**Anaconda Navigator**

1.  If you downloaded **Anaconda (with Jupyter Notebook)** as recommended in System Setup above, launch the Anaconda-Navigator application.  Click on **Jupyter Notebook** to activate Jupyter Notebook.  

2.  You will see a new **tab open automatically in your default web browser** (such as Chrome), in which there is a **directory tree** illustrating the current file contents of the **current working** directory.  Navigate to the directory containing **SampleSheet.ipynb**.  Click on the **SampleSheet.ipynb** file to open it as a Jupyter Notebook.

3. In the Jupyter Notebook menu bar at the top of the newly opened SampleSheet.py Jupyter Notebook file, take the following steps to **specify SampleSheet\_env as your Python virtual environment** (see System Setup: 3. Create a Python Virtual Environment for Jupyter Notebook):  

	(a) click on the **'Kernel'** tab to open a drop-down menu  
	(b) hover your mouse over **'Change Kernel'** (bottom option of drop-down menu) to identify the kernels available to the Notebook  
	(c) choose **SampleSheet\_env** as the kernel  
	 
	 If SampleSheet\_env does not appear as a kernel option, troubleshooting is needed to either create the CollatedMoitfs_env virtual environment, or to make Jupyter Notebook aware of the existence of the SampleSheet\_env virtual environment.

4.  If these steps have been accomplished successfully, you are now ready to use the SampleSheet.py Jupyter Notebook.  Be prepared to provide required input variables as detailed below in **Input Notes**.
<br>

**Command Line**  

1.  If you plan to activate Jupyter Notebook from your command line, first navigate to the directory containing **SampleSheet.ipynb**.  For example, if you downloaded the code repository as **SampleSheet** and are not already in **SampleSheet** as your working (current) directory at the command line, **navigate** there at this time using commands similar to the following:

	* For example, on Mac OS:  
	`$ cd /Users/yourusername/Documents/SampleSheetCode/SampleSheet`

	* For example, on Windows OS:  
	`$ cd C:\Documents\SampleSheetCode/SampleSheet`
	
	You can **check your current working directory** by issuing the following command:  
	
	`$ pwd`

2.  To further confirm that you are in the repository directory at the command line, you can **check the files** present in the repository directory by issuing the following command:  

	`$ ls` (Mac OS)  
	`$ dir` (Windows OS)	
	
	You should see a list of files that includes the SampleSheet Jupyter Notebook and/or Python program file (depending on whether you downloaded the repository or individual program files), named **SampleSheet.ipynb** or  **SampleSheet.py**
	
3.  Activate Jupyter Notebook by issuing the following command:  

	`jupyter notebook`  (Mac OS, in Terminal)
	
4.  Refer to steps 2-4 under "Anaconda Navigator" approach above.  

#### <span style="color:dodgerblue">Python program file</span>
**Anaconda Navigator**

1.  If you downloaded **Anaconda (with Jupyter Notebook)** as recommended in System Setup above, launch the Anaconda-Navigator application.  Click on **Jupyter Notebook** to activate Jupyter Notebook.  

2.  You will see a new **tab open automatically in your default web browser** (such as Chrome), in which there is a **directory tree** illustrating the current file contents of the **current workin

**Command Line**  

1.  If you plan to run SampleSheet.py from your command line, first navigate to the directory containing **SampleSheet.py**.  Prepare access to a Python virtual environment containing appropriate packages required by SampleSheet.py, as described in System Setup.  

    (a) Activate SampleSheet\_env:   
	`source SampleSheet\_env/bin/activate`  (Mac OS)  
	or  
	`.\SampleSheet\_env\Scripts\activate`  (Windows OS) 	
	
	(b) You should now see that your command line prompt has changed to indicate that you are in the virtual environment, displaying something along the lines of:  
	`(SampleSheet\_env) $`

2.  To run SampleSheet.py, type **python3 SampleSheet.py** and hit 'Enter':
	`(SampleSheet\_env) $ python3 SampleSheet.py`  
	
3.  If these steps have been accomplished successfully, you will now encounter the first interactive prompts of SampleSheet.py.  Be prepared to provide required input variables as detailed below in **Input Notes**.



#### <span style="color:dodgerblue">Virtual machine (Alleles\_and\_altered\_motifs.ovf)</span>

1. To access the virtual machine that comes pre-installed with external dependencies, **download the file Alleles\_and\_altered\_motifs.ovf** (**Zenodo, DOI 10.5281/zenodo.3406862**). *Note: this requires virtualization software, such as Oracle VM VirtualBox*

2. Instructions for how to set up the virtual machine (including username and password for ovf file) can be found in the Zenodo repository.    




## <span style="color:blue">Operation notes</span>
*What does this script do?*

This script automates creation of a Sample Sheet compatible with Illumina® sequencing, based on custom i7 (96) and i5 (96) index sequences used to barcode up to 9,216 distinct samples. Specifically, the script performs these operations:

 1. **collect user input**
    - specify Dual Indexed Sequencing Workflow (A *vs.* B)
    - specify Investigator Name, Project Name
    - specify single index (SE) *vs.* dual-indexed (PE) barcode format
    - collect Data relationships (sample ID, i7 barcode range (1-96) and single i5 barcode (1-96)  
 
 
 2. **generate Sample Sheet**
    - populate \[Header\] and \[Reads\] sections based on user input for Investigator Name, Project Name, SE/PE format
    - populate \[Data\] section based on expansion of i7 barcode range and i5 barcode designation for each 96-well plate  
    (appropriate i7 and i5 index sequences are populated based on user-specified Workflow (A *vs.* B))

## <span style="color:blue">Input notes</span>
You will be prompted for the following user-specific information:

**Required** (4 strings):
      <ul>
      <li>where should output file go?</li>
          *absolute path to* **output directory** *and filename for Sample Sheet*
      <li>Investigator Name</li>
          *character string specifying name to be associated with Sample Sheet and sequencing run*         
      <li>Project Name</li>
          *character string specifying project name to be associated with Sample Sheet and sequencing run*
      <li>Run type specification: Single-end (SE) or Paired-end (PE) sequencing run?  
      *how many sequencing cycles (read length for R1 (Read 1) and R2 (Read 2))?*  
      *...comma-separated character string indicating SE vs. PE, # of sequencing cycles (R1), # of sequencing cycles (R2, if applicable)*
      <li>List of sample:barcode relationships</li>
      *single lines of comma-separated character strings specifying overarching sample prefix to assign to up to 96 samples arrayed in 96-well plate format (prefix is parsed to samples with well suffixes, e.g., -A01, -A02...-H12); barcode assignments (i7 and i5) are designated to individual samples based on integer range (i7) or integer (i5) assigned to plate*
      </ul>
  
Note on list of sample:barcode relationships: This is a list of plate names (prefixes), i7 index range, and i5 index.  
For example: 'DG-1, 1-96, 5' on a single line of text would indicate plate name/prefix 'DG-1' applied to up to 96 samples (uniqued identified by well position A01-H12, *e.g.*, DG-1-A01, DG-1-A02, ... DG-1-H12), range of i7 indices used to barcode individual wells in this 96-well plate (*e.g.*, A01-H12), and i5 index used across all wells of this plate (*e.g.*, A05).

## <span style="color:blue">Output notes</span>
In brief: Illumina® Sample Sheets accommodate up to 10 column fields, but only 5 of these (fields 2, 5-8) are required for a sequencing run (indicated below).  This script outputs only these 5 required column fields.  
 
10 Sample Sheet column fields:  
  - field 1: Sample\_ID,  
  - field 2: Sample\_Name,  
  - field 3: Sample\_Plate,  
  - field 4: Sample\_Well,  
  - field 5: I7\_Index\_ID, 
  - field 6: index,  
  - field 7: I5\_Index\_ID,  
  - field 8: index2, 
  - field 9: Sample\_Project,  
  - field 10: Description  

Fields customized and output by this script:  
  - field 2: Sample_Name,  
  - field 5: I7\_Index\_ID, 
  - field 6: I7 index sequence,  
  - field 7: I5\_Index\_ID,  
  - field 8: I5 index sequence  
 
## <span style="color:blue">Visual summary of key script operations</span>
In short, **brief user inputs** (*e.g.*, below), are converted to **Sample Sheet** contents compatible with Illumina® sequencing (**key output file**, below). In particular, a minimal list of up to 96 \[Data\] relationships is expanded in microseconds to a \[Data\] section containing up to 9,216 sample:barcode relationships.  

*example*  

------
**input:**  
*Path to Sample Sheet file name for creation:* /Users/name/SampleSheet.csv  
*Workflow:* A  
*Investigator Name, Project Name*: Dorothy Gale, Sequencing  
*SE vs. PE, cycle details*: PE, 151, 151  
*sample:barcoded relationships:*  
DG-1, 1-96, 4  
DG-2, 1-96, 9  
DG-2, 1-96, 78  
<br clear="all" />
**output:**
<br clear="all" />
<img src="SampleSheet_img/SampleSheet_example.png" align="mid" width="500">  
<div style=text-align:center>...etc.</div>


## <span style="color:blue">Status</span>
Project is:  _finished_, _open for further contributions_


## <span style="color:blue">Contact</span>
Created by kirk.ehmsen[at]gmail.com - feel free to contact me!    
Keith Yamamoto laboratory, UCSF, San Francisco, CA.