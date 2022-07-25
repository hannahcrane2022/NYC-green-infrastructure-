# NYC-green-Infrastructure
This repository includes all the code for the report and policy brief, Analyzing Green Infrastructure (GI) Progress and Trends in New York City by Hannah Crane for Policy and Data Studio Summer 2022.
# Project Description
Green infrastructure is an array of practices that use or mimic natural systems to manage stormwater runoff, and is critical to lessening the impact of rainstorms and floods on New York City's infrastructure. This analysis uses data analysis and visualizaition programs to observe how New York City is progressing on its green infrastructure commitments, which types of green infrastructure projects are most common, and where most projects are taking place, by borough and by neighborhood tabulation area (NTA). In the analysis, I  included data on street flooding complaints and median income to see how (and if) green infrastructure projects were correlated to those variables.
# Running the Analysis

Step 1 - Installing Programs
To replicate this analysis, you will need to install R and R Studio, QGIS. For the visualizations, I used Data Wrapper (online program) and Tableau due to personal preference and knowledge of the programs. 

Step 2 - Downloading the Data to your local computer:

[NYC DEP Green Infrastructure 
](https://data.cityofnewyork.us/Environment/DEP-Green-Infrastructure/spjh-pz7h)

[NYC 311 Dataset filtered for Street Flooding](https://data.cityofnewyork.us/widgets/wymi-u6i8?mobile_redirect=true) 

[American Communities Survey by NTA 
](https://www1.nyc.gov/site/planning/data-maps/open-data/dwn-acs-nta.page)


[NTA Shapefile](https://data.cityofnewyork.us/City-Government/2010-Neighborhood-Tabulation-Areas-NTAs-/cpf4-rkhq) 

Step 3 - Running R Script
Run the analysis in R using the script.

Please note that on line 130, I used QGIS to layer greate a CSV of green infrastructure counts by NTA. To do this, I imported a shapefile of NYC NTAs and a cleaned Green Infrastructure CSV into QGIS. I used the count points in polygon tool under "Analysis Tools" in QGIS to get the count of complaints by NTA, then exported that as a CSV. 
This step is necessary to performt the NTA level analysis.
I also performed the same actions to sum the complaint counts by NTA to perform the analysis on line 156.

Step 4 - Visualizations
After performing the analysis and data cleaning in R, I exported the dataframes of interest and used [Data Wrapper](https://www.datawrapper.de/) and [Tableau](https://www.tableau.com/) to create visualizations.



# Questions about the data or analysis? Contact Hannah Crane at hic2017@nyu.edu
