# Data processing methods and code for deadwood dynamics census data, Barro Colorado Island, Panama” 

This repository contains example datasets and code for cleaning, processing, and analyzing woody debris data from BCI50ha, Panama. It provides a streamlined workflow for handling the raw data, including functions for preprocessing, calculations to estimate intennual woody debris stocks and fluxes and visualization. 

## Citation and License
The code publication, which should be cited as follows:

It is licensed under CC BY 4.0.

The data used in this is part of the annual woody debris census data for a spatially stratified sample of the 50-ha plot on Barro Colorado Island, Panama. This census has been conducted annually since 2009 through 2024 (excluding 2011). The data included in this repository are solely for the purpose of demonstrating the workflow for handling annual woody debris census data. These datasets, covering the years 2017–2024, are not for analytical and publication. 
For complete datasets please refer to the following data publication:

*Pablo Ramos, Paulino Villarreal, Lily F. Pitcher, and Helene C. Muller-Landau. 2025. Annual woody debris dynamics census data for the 50-ha plot on Barro Colorado Island, Panama. Smithsonian Research Data Repository. https://doi.org/10.60635/C3C884*

The aim of this census is to quantify the volume and mass of standing and fallen coarse woody debris (abbreviated CWD, pieces at least 200 mm in diameter) and standing fine woody debris, (abbreviated FWD, pieces less than 200 mm in diameter).  Censuses took place in 100 subplots, each 40 x 40 m.  Fallen woody debris were censused using the line-intersect method in four 40-m transects divided into 10-m subsections.  Standing woody debris was censused with area-based methods, with CWD censused in the entire subplot, and FWD censused in a circular area in the center with a 5 m radius.  Methods followed the ForestGEO CWD Dynamics protocol which can be found here:
https://forestgeo.si.edu/protocols/woody-debris

File structure and details about data processing are given in the readme file. 

## Files and Folders
### Code & Data files
**clean0_bci50deadwood_rawtoformatted.rmd**

The aim of this file is to format raw .xlx data including; removing special characters, changing column heading to be consistent across years, fixing errors with values having been converted to dates, checking for duplicate entries etc. and outputs individual .txt files for each census (Fallen CWD, Standing CWD and Standing FWD) for each year.

Input files:
- **Data0_Raw/**: This Folder contains the eight raw .xlx files for the woody debris census for each year (2017-2024). Each file contains multiple sheets including a seperate sheet for Fallen CWD, Standing CWD and Standing FWD. These raw files are inputed into the rmd file to be formatted
- **Corrections/colnamechange**: This folder contains eight .txt files containing the origional names of column headings which can be found in the raw data and corresponding new column headings. These files are inputed into the rmd to be used to change the column headings of the raw data.

Output:
- **Data1_Formatted**: This folder contains the formatted  files  in the form of .txt generated from the clean0_bci50deadwood_rawtoformatted.rmd. There is an individual file for each census (Fallen CWD, Standing CWD and Standing FWD) for each year. 

**clean1_bci50deadwood_formattedtocorrected.rmd**

The aim of this file is to apply corrections to the formatted .txt files and merge data accross multiple censuses by sample codes (code_of_piece) for each census type (Fallen CWD, Standing CWD and Standing FWD), generating three .csv files, one for fallen CWD, one for standing CWD and  one for standing FWD. Corrections include, fixing typos and moving data which had been inputted incorrectly to the correct field, aligning data types, removing duplicates, removing occurances where the sample was <200mm from coarse woddy debris censuses or had fully decomposed etc. Additional codes are assigned to pieces based on descriptive terms in the notes column. Further details of corrections applied can be found the inputted corrections file (see below)


Input:
- **Data1_Formatted/** : As desribed above this folder contains the formatted  files in the form of .txt generated from the clean0_bci50deadwood_rawtoformatted.rmd. There is an individual file for each census (Fallen CWD, Standing CWD and Standing FWD) for each year. 
- **Corrections/bciCDW40Corrections_All.csv**:
- **Corrections/masscorrect_deadwood_bci50ha.csv**
- **Corrections/newcodesadd_bci50ha.csv**
- **Corrections/datatype_dyanmicWD.csv**
 Output:
- **Data2_Corrected**

**density_calculation_bci50deadwood_correctedtoprocessed.rmd**

Definition:

Input:
- **Data2_Corrected**
- t
Output:
- **Data3_Processed**

**report_bci50CWD_2017-24.rmd**
Definition:

Input:
- **Data3_Processed**
- t
Output:
- **report_bci50CWD_2017-24.html**
- **bci_CWD40_annual_estimation_17to24.txt**
- **bci_CWD40_subplot_estimation_17to24.txt**

## Other

**protocol**

**equation explanation**
  
