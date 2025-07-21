# Data processing methods and code for deadwood dynamics census data, Barro Colorado Island, Panama.

This repository contains example datasets and code for cleaning, processing, and analyzing woody debris data from the 50-ha plot on Barro Colorado Island, Panama. It provides a streamlined workflow for handling the raw data, including functions for preprocessing, calculations to estimate annual woody debris stocks and fluxes, and visualization. 

## Citation and License
The code publication, which should be cited as follows:
*Lily F. Pitcher, and Helene C. Muller-Landau. 2025. Data processing methods and code for deadwood dynamics census data, Barro Colorado Island, Panama , Zenodo*

It is licensed under CC BY 4.0.

The data used in this is part of the annual woody debris census data for a spatially stratified sample of the 50-ha plot on Barro Colorado Island, Panama. This census has been conducted annually from 2009 through 2025, with the exception of 2011. The data included in this repository are solely for the purpose of demonstrating the workflow for handling annual woody debris census data. These datasets, covering the years 2017–2024, are not for analysis and publication. 
For complete datasets please refer to the following data publication:

*Pablo Ramos, Paulino Villarreal, Lily F. Pitcher, and Helene C. Muller-Landau. 2025. Annual woody debris dynamics census data for the 50-ha plot on Barro Colorado Island, Panama. Smithsonian Research Data Repository. https://doi.org/10.60635/C3C884*

The aim of this census is to quantify the volume and mass of standing and fallen coarse woody debris (abbreviated CWD, pieces at least 200 mm in diameter) and standing fine woody debris, (abbreviated FWD, pieces less than 200 mm in diameter).  Censuses took place in 100 subplots, each 40 x 40 m.  Fallen woody debris were censused using the line-intersect method in four 40-m transects divided into 10-m subsections.  Standing woody debris was censused with area-based methods, with CWD censused in the entire subplot, and FWD censused in a circular area in the center with a 5 m radius.  Methods followed the ForestGEO CWD Dynamics protocol which can be found here:
https://forestgeo.si.edu/protocols/woody-debris

File structure and details about data processing are described below.

## Files and Folders
### Code & Data files
### **clean0_bci50deadwood_rawtoformatted.rmd**

The aim of this Rmarkdown file is to format raw .xlx data; removing special characters, changing column heading to be consistent across years, fixing errors with values having been converted to dates, checking for duplicate entries etc. The code outputs individual .txt files for each census (Fallen CWD, Standing CWD and Standing FWD) for each year. The .xlx files for each year are read in individually.

Input files:
- **Data0_Raw/**: This Folder contains the eight raw .xlx files for the woody debris census for each year (2017-2024). Each file contains multiple sheets including a seperate sheet for Fallen CWD, Standing CWD and Standing FWD. These raw files are inputed into the .rmd file to be formatted individually, the code does not run automated through every filen in the folder as column heading and structure vary for each file. Within the code the raw file must be specified.
- **Corrections/colnamechange**: This folder contains eight .txt files with the origional names of column headings in the raw data and corresponding new column headings. These files are inputed into the .rmd to be used to change the column headings of the raw data.

Output:
- **Data1_Formatted**: This folder contains the formatted  files  in the form of .txt generated from the clean0_bci50deadwood_rawtoformatted.rmd. There is an individual file for each census (Fallen CWD, Standing CWD and Standing FWD) for each year. 

### **clean1_bci50deadwood_formattedtocorrected.rmd**

The aim of this file is to apply corrections to the formatted .txt files and merge data accross multiple censuses by sample codes (code_of_piece) for each census type, generating three .csv files, one for fallen CWD, one for standing CWD and  one for standing FWD. Corrections include, fixing typos and moving data to the correct field, aligning data types, removing duplicates, removing occurances where the sample was <200mm or had fully decomposed from CWD censuses etc. Additional codes are assigned to pieces based on descriptive terms in the notes column. Further details of corrections applied can be found the inputted corrections file (see below).


Input:
- **Data1_Formatted/** : As desribed previously this folder contains the formatted files in the form of .txt generated from the clean0_bci50deadwood_rawtoformatted.rmd. There is an individual .csv file for each census (Fallen CWD, Standing CWD and Standing FWD) for each year. 
- **Corrections/bciCDW40Corrections_All.csv**: Contains individual row by row corrections for data across years. This file is inputed into the .rmd to be used to apply the corrections.
  - *file* column specifies the formatted .txt file the correction is being applied to.
  - *year* column specifies the year of the error.
  - *uniqid* column identifies the uniq ID given to each row within that data file.
  - *Field* column specifies the column the error to be corrected occured.
  - *OldValue* the exisiting value that needs to be changed.
  - *NewValue* the correct value to be inserted.
  - *Blank* specifies whether row needs to be removed entirely. blank=FALSE, Y=TRUE.
  - *Observer* initials of person who identified the error and applied the change.
  - *Comment* details of the cause of the error, why it must be changed.
- **Corrections/masscorrect_deadwood_bci50ha.csv**: Contains corrections to applied for indivudal columns across whole dataset for cases where certain errors appear multiple times across the datasets. This file is inputed into the .rmd to be used to apply the corrections.
  - *file* column specifies the formatted .txt file the correction is being applied to.
  - *colname* column specifies the column the error to be corrected occured.
  - *OldValue* the exisiting value that needs to be changed.
  - *NewValue* the correct value to be inserted.
  - *notes* details of the cause of the error, why it must be changed.
- **Corrections/newcodesadd_bci50ha.csv**:The file contains new descriptive codes for woodydebris samples translated from recurring descriptive notes in the raw data. This file is inputed into the .rmd and adds a new code columns to the input data named coded_notes. A letter based code is pasted where it is TRUE, as specified in the newcodesadd_bci50ha.csv file.
  - EX- The piece was not found
  - DP- The piece diameter is below 200mm or fully decomposed.
  - PN- The piece was present but could not be measured that year. i.e it was covered by another piece
  - AA- The piece has cracked and is opening
  - SD- The piece is breaking into little pieces
  - CH- The piece is hollow
  - CAIDO- For standing samples- the sample has now fallen
  - VIVO- the sample is actually alive
  - BUTTRESS- for standing pieces, the roots are buttressed and the DBH is measured above 1.3m
- **Corrections/datatype_dyanmicWD.csv**: This file re-assigns data types of columns in the data across years. This ensures data is treated correctly and consistent across years so data can be merged. This file is inputed into the .rmd and formats the data into the correct data type.
  
 Output:
- **Data2_Corrected**: This folder contains the formatted and cleaned data in the form of three .csv for the three census types across years. As described above these files are generated from the clean1_bci50deadwood_formattedtocorrected.rmd. This data can now be used for further calculation and analysis of interannual deadwood stocks and fluxes. Folder also contains **data_dictionary_woodydebrisBCI.csv** which defines the column heading for each census.

### **density_calculation_bci50deadwood_correctedtoprocessed.rmd**

The aim of this file is to carry out a series of calculations that allow for estimations of CWD (excludes FWD) volume and mass from the diameter measurements. A detailed explanation of all the calculations involved can be found in **CalculationExplanation_for_deadwoodStocks&Fluxes.pdf**.

Input:
- **Data2_Corrected**: This folder contains the formatted and cleaned in the form of three .csv for the three census types across years. The two files for CWD only (both fallen and standing) are used for further calculation and analysis
- **Dat0_Raw/LongT_CWD_2010.csv**: This folder contains data from a census in 2010 where samples were destructively sampled to measure and calculate dry and wet density and mass. This value is used in the .rmd to model the relationship between sample density and penetrometer measurements of samples in the field. This  allows density to be predicted for census data from pentrometer measurements. See **CalculationExplanation_for_deadwoodStocks&Fluxes.pdf** for further details.
Output:
- **Data3_Processed**: This folder contains the processed data with the outputs for calculations to estimate coarse woody debris mass and volume conducted in **density_calculation_bci50deadwood_correctedtoprocessed.rmd**. This is intermediate data, the purpose of this data is to be fed directly into the **report_bci50CWD_2017-24.rmd** to generate a report of internanual varaitions in stocks and fluxes. Definitions of new columns generated in these data files can be found in the Appendix of **CalculationExplanation_for_deadwoodStocks&Fluxes.pdf**.

### **report_bci50CWD_2017-24.rmd**

This file generates a report, including figures and tables reporting average estimates of coarse standing and fallen woody debris stocks and inputs per ha for each subplot and across the 50 ha plot, BCI for each year. Subplot averages and annual averages are also outputted as .txt files, see below.

Input:
- **Data3_Processed**: This folder contains the processed data with the outputs for calculations to estimate CWD mass and volume with the purpose of being inputted directly into report file for further calculations and reporting.

Output:
- **report_bci50CWD_2017-24.html**: The html report file generated from the rmd that prints figures and tables that report estimates of standing and falling CWD stocks and inputs per ha per year.
- **Output/bci_CWD40_annual_estimation_17to24.txt**: A txt file of subplot level estimates for woody debris stocks and inputs.
    - subplot_code: coordinates for the centre point (using local 20 m coordinate system) of the 40x40 m subplot within the BCI 50-ha plot.
    - yearcol: year.
    - mass.Mg.ha: Estimated mass of woody debris in Mg per ha.
    - vol.m3.ha: Estimated volume of woody debris in m3 per ha.
    - input.mass.Mgha: Estimated annual input mass of woody debris in Mg per ha.
    - input.vol.m3ha: Estimated annua; input volume of woody debris in m3 per ha.
    - type: whether the estimate is for fallen or standing stocks.
- **Output/bci_CWD40_subplot_estimation_17to24.txt**: txt file of whole plot (50 ha) level estimates for woody debris stocks and inputs
    - yearcol: year.
    - mass.Mg.ha: Estimated mass of woody debris in Mg per ha.
    - vol.m3.ha: Estimated volume of woody debris in m3 per ha.
    - input.mass.Mgha: Estimated annual input mass of woody debris in Mg per ha.
    - input.vol.m3ha: Estimated annual input volume of woody debris in m3 per ha.
    - type: whether the estimate is for fallen or standing stocks.

## Other

**CWD_Dynamics_Protocol.pdf** contains the census protocol followed in collecting the data 

**CalculationExplanation_for_deadwoodStocks&Fluxes.pdf** contains a desription of the steps involved in calculating deadwood stocks and fluxes that is conducted in **density_calculation_bci50deadwood_correctedtoprocessed.rmd** and **report_bci50CWD_2017-24.html** using the clean data in Data2_Corrected.
  
