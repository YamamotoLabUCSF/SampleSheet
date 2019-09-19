# SampleSheet.py
Create an Illumina® Sample Sheet, the comma-separated text document required by Illumina® sequencing systems to specify (1) sequencing parameters and (2) sample-barcode relationships. Customize [Header], [Reads], and [Data] sections; in particular, draw on up to 192 8-bp barcodes (96x i7 &amp; 96x i5 indices) to specify up to 9,216 sample-barcode relationships for multiplexed amplicon sequencing.
 <br/><br/> 
 
## Table of contents
* [Background](#background)
* [Features](#features)
* [Setup](#setup)
* [Requirements](#requirements)
* [Synopsis](#synopsis)
* [Operation notes](#operationnotes)
* [Input notes](#input%20notes)
* [Output notes](#output%20notes)
* [Visual summary of key script operations](#visual%20summary%20of%20key%20script%20operations)
* [Status](#status)
* [Contact](#contact)
  

## Background
<img src="SampleSheet_img/SampleSheet_thumbnail.png" align="left" width="600">  
Sequencing by synthesis (SBS) collects millions to billions of DNA sequence reads *en masse*. DNA templates from tens to thousands of independent sample sources can be barcoded, pooled, and sequenced on a common flow cell. Unique indices (barcode sequences) allow pooled reads to be assigned to cognate sample sources (demultiplexed).  
<br/><br/>

**This script automates creation of an Illumina® Sample Sheet, the comma-separated text document required by Illumina® sequencing systems to specify (1) sequencing parameters and (2) sample-barcode relationships.** With this script, a Sample Sheet with up to 9,216 sample-barcode relationships can be automatically generated in <1 second, following user entry of a single simplified list containing up to 96 sample prefixes assigned to an i7 index range and unique i5 index (each sample prefix to be expanded to up to 96 individual samples, suffixed by well ID (*i.e.*, A01-H12) of a 96-well plate).


## Features
* Automates data entry into 3 Illumina® Sample Sheet sections, based on command-line input provided by a user:
	* \[Header] (InvestigatorName, ProjectName)
	* \[Reads] (# of reads)
	* \[Data] (Sample ID, i7 index, i5 index)

## Setup
Code is available as a Jupyter Notebook file (**SampleSheet.ipynb**) or as a Python program file (**SampleSheet.py**) for direct use, or pre-packaged with all dependencies as an Open Virtualization Format file for virtual machines (**Alleles\_and\_altered\_motifs.ovf**).  
 
To use: (1) fork and clone this repository to a local destination, or (2) download the file SampleSheet.ipynb or SampleSheet.py (GitHub) or Alleles\_and\_altered\_motifs.ovf (Zenodo, DOI 10.5281/zenodo.3406862).  
  
Jupyter Notebook file requires *SampleSheet_img* directory containing five image files to be available in the directory from which the Jupyter Notebook was opened.  

## Requirements
* Python 3.7 or higher
* For CLI script (suggested): download & install PrettyTable (<a href="https://pypi.org/project/PrettyTable/">PyPi link</a> or <a href="https://github.com/jazzband/prettytable">GitHub link</a>)
  

## Synopsis
**This script returns a Sample Sheet file compatible with Illumina® sequencing platforms.** 

**Users are asked for the path to an output directory in which a Sample Sheet will be created, along with user-specific variables for Sample Sheet \[Header\], \[Reads\], and \[Data\] sections.**

**For [Data] relationships between sample names and i7+i5 indices, SampleSheet.py draws upon a set of 192 custom primers with unique 8-bp barcodes compatible with Illumina® sequencing platforms; these indices allow up to 9,216 samples to be arrayed in 96-well (or 384-well) format with unique barcodes for pooled sequencing.**

>(see 'Input notes' for details).
    
Note on index usage: In this script, each i7 index identifies an individual well within a 96-well plate format (each well is uniquely barcoded by a single i7 index), whereas a single i5 index defines all wells of a specific plate (up to 96 wells in a single plate are barcoded by a common i5 index).  Primer sequences (and indices used by SampleSheet.py) can be found in associated files, i7\_barcode\_primers.xls and i5\_barcode\_primers.xls.

For further usage details, please refer to the following manuscript:  
>*Ehmsen, Knuesel, Martinez, Asahina, Aridomi, Yamamoto (2019)*
    
Please cite usage as:  
>SampleSheet.py  
>*Ehmsen, Knuesel, Martinez, Asahina, Aridomi, Yamamoto (2019)*
 


## Operation notes
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

## Input notes
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

## Output notes
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
 
## Visual summary of key script operations
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


## Status
Project is:  _finished_, _open for further contributions_


## Contact
Created by kirk.ehmsen[at]gmail.com - feel free to contact me!    
Keith Yamamoto laboratory, UCSF, San Francisco, CA.