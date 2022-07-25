library(tidyverse)
library(sf)
library(janitor)
install.packages("plyr")
library(plyr)

#Uploading the 2015 NYC Street Tree Census
trees <- read_csv("2015_Street_Tree_Census_-_Tree_Data.csv") %>%
  clean_names()

#selecting which variables I want to analyze (it is a pretty big dataset), and then grouping the tree count by each borough
tree_data_test <- select(trees, spc_common, borocode, borough, nta_name, latitude, longitude)
total_by_boro <- tree_data_test %>%
  group_by(borough) %>%
  dplyr:: summarize(spc_common = n())


complaint <- read_csv("complaint_types.csv")
rm(complaint)

#importing the NYC Green Infrastructure dataset from NYC Open Data
green_infrastructure <- read_csv ("GI_assets_publicMap_construct (1).csv") %>%
  clean_names() %>%
  select (asset_type, asset_id, status, borough, dep_contra)
#we don't need all the variables, so choosing the ones that I will probably need

#below code is grouping the green infrastructure project by borough and summing up all the green infrastructure 
# projects in each borough to get a preliminary look at what we are working with.
GI <- green_infrastructure %>%
  group_by(borough) %>%
  dplyr::summarise(asset_type = n())

#Looking at number of green infrastructure projects that are in each phase (design, construction, completed)
all_boroughs <- ddply(green_infrastructure, .(status), nrow) %>%
  adorn_totals("row") %>%
  mutate (pct = V1 / 14248 * 100)


#want to look at the statuses of all the projects by borough and compare the boroughs,
#the code below subsets each borough from the larger GI dataframe and then counts how many 
# projects of each status  each borough has.

# the code that begins with "project" for each borough breaks down and sums the TYPES of projects throughout 
# each borough so that I can see if some types of projects are more common than others in certain boroughs.
QUEENS <- subset(green_infrastructure, borough == 'Queens') 
Queens <- ddply(QUEENS, .(status), nrow) %>%
  adorn_totals("row") %>%
  mutate (pct = V1 / 7349 * 100)
Projects_Queens <- ddply(QUEENS, .(asset_type), nrow)
rm(QUEENS)

BK <- subset(green_infrastructure, borough == 'Brooklyn') 
Brooklyn <- ddply(BK, .(status), nrow) %>%
  adorn_totals("row") %>%
  mutate (pct = V1 / 5804 * 100)
Projects_BK <- ddply(BK, .(asset_type), nrow)
rm(BK)
rm(brooklyn)

SI <- subset(green_infrastructure, borough == 'Staten Island') 
Staten_Island <- ddply(SI, .(status), nrow)%>%
  adorn_totals("row")  %>%
  mutate (pct = V1 / 39 * 100)

projects_SI <- ddply(SI, .(asset_type), nrow)
rm(SI)

test <- left_join (SI, projects_SI, by ="V1")
rm(test)

Bronx1 <- subset(green_infrastructure, borough == 'Bronx')
Bronx <- ddply(Bronx, .(status), nrow) %>%
  adorn_totals("row")%>%
  mutate (pct = V1 / 936 * 100 )
Projects_Bronx <- ddply(Bronx1, .(asset_type), nrow)
rm(Bronx1)
rm(bronx)

Manhattan1 <- subset(green_infrastructure, borough == 'Manhattan') 
  Manhattan <- ddply(Manhattan1, .(status), nrow) %>%
  adorn_totals("row") %>%
  mutate (pct = V1 / 120 * 100)
Projects_Manhattan <- ddply(Manhattan1, .(asset_type), nrow)
rm(Manhattan1)

#importing street flooding complaints from a subset of the 311 dataset to see if the number of complaints has any correlation
# to the number of green infrastructure projects per borough

flooding_complaints <- read_csv ("Street_Flooding.csv") %>%
  clean_names() %>%
  select(created_date, closed_date, agency, complaint_type, descriptor, incident_address, city, community_board, borough)%>%
  filter(is.na(borough) == FALSE)

complaints <- flooding_complaints %>%
  group_by(borough) %>%
  dplyr::summarise(complaint_type = n())

complaints <- complaints %>%
  mutate(borough = recode(borough, BRONX = 'Bronx', BROOKLYN = 'Brooklyn', MANHATTAN =  'Manhattan', QUEENS = 'Queens'))

complaint_count <- complaint %>%
  group_by(borough)%>%
  dplyr::summarise(complaint_type == n()) %>%
  filter(is.na(borough)== FALSE) 
  
  rm(sq_miles)
sq_miles <- read_csv("borough_sq_miles.csv") %>%
  clean_names()

# joining GIcount by borough, tree count by borough, and complaint count by borough then normalizaing 
# the data by dividing each by square miles and rounding to 2 decimal places for clarity and tog get a more 
# accurate look at the data. I also ended up not including the tree census information because I did not think it helped
# the overall analysis and the numbers were so large that they ruined the visualizations.
df1 <- left_join (GI, total_by_boro, by ="borough")
df2 <- left_join(df1, complaint, by = "borough")
df3 <- merge(x= df2, y = sq_miles, by = "borough")
df4 <-  df3 %>%
  mutate(projects_per_sq_mile = asset_type / square_miles,
         Projects = round(projects_per_sq_mile, digits =2),
         complaints_sq_miles = complaint_type/square_miles,
         Complaints = round(complaints_sq_miles, digits = 2))

normalized_data <- df4 %>%
  select(borough, Complaints, Projects) 


rm(df1, df2, df3, df4, complaint_count, chart_1, chart_2)


#Breaking down green infrastructure projects by NTA, I used QGIS to layer the GI dataset on to a map of NTAs and then exported as CSV
# so I could look at the data at a more granular level.
GI_nta <- read_csv("NTA_GI.csv") %>%
  clean_names() %>%
  select(ntaname, numpoints) 
  
  names(GI_nta)[names(GI_nta) == "numpoints"] <- "GI_Count" 
names(GI_nta)[names(GI_nta) == "ntaname"] <- "NTA" 

#Importing income data by NTA to see if there is any correlation between median income in the NTA and the number of green infrasutructure
# projects. This is an environmnetal justice aspect of my analysis, I am curious to see if income plays a role in deciding where the city 
# carries out these projects. I also wanted to do look at demographic data (race and ethnicity) but I did not have enough time.
income_data <- read_csv("econ_2016acs5yr_nta.csv") %>%
  clean_names() 
  
  income_data <- income_data %>%
  select(ntaname, md_hh_inc_e) %>%
  filter(is.na(md_hh_inc_e )== FALSE)
names(income_data)[names(income_data) == "md_hh_inc_e"] <- "Median_Income" 
names(income_data)[names(income_data) == "ntaname"] <- "NTA" 



#joining the median income and the GI data frames into one data frame  
income_projects_nta <- left_join (GI_nta, income_data, by ="NTA") 

#I went back into QGIS to break the flooding complaint dataset down into NTA level counts. Thank goodness both
# the GI and complaint datasets had lat and long! 
nta_complaints <- read_csv("nta_complaints.csv") %>%
  clean_names() %>%
  select(borough, ntaname, numpoints)
names(nta_complaints)[names(nta_complaints) == "numpoints"] <- "Complaint_Count" 
names(nta_complaints)[names(nta_complaints) == "ntaname"] <- "NTA" 

complaint_gi_nta <- left_join (nta_complaints, GI_nta, by ="NTA") 
low_GI <- complaint_gi_nta %>%
  filter(Complaint_Count > 100, GI_Count < 50 )

74/195

#Merging the complaints, GI projects, and median income, now de-aggregated into NTA level counts of each
# so I can compare them and see if I can find any meaningful conclusions. 
complaints_projects_nta <- merge( x= nta_complaints , y = income_projects_nta, by = "NTA") %>%
  select(NTA, Complaint_Count, GI_Count, Median_Income) 
  
  #removing random data frames from my R panel because it got super messy.
  rm(new_queens, select_df4, newdata, test, test1, test2, time, trees, tree_data_test)
rm(x, df, f2)

# Exporting all of the data frames I created and  want to turn into vizualizations in Data Wrapper and Tableau
write_csv(all_boroughs, "all_boroughs.csv")
write_csv(Bronx, "Bronx.csv")
write_csv(Brooklyn, "Brooklyn.csv")
write_csv(Manhattan, "Manhattan.csv")
write_csv(Queens, "Queens.csv")
write_csv(Staten_Island, "Staten_Island")

write_csv(normalized_data, "Normalized Data.csv")
write_csv(complaints_projects_nta, "Complaints and Projects by NTA.csv")







