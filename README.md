# Project: Web Data Pipeline


## Overview


Goal of this project was to build a pipeline for retrieval, processing, and analysis of a chosen data set to answer a specific research question.


### Part 1: Data retrival, unpacking, and organization
Using this pipeline data from a variety of different cancer studies are retrieved from the National Cancer Institute (NCI) Genomic Data Commons (GDC) data portal using an api ('https://api.gdc.cancer.gov/cases'). The following variables are retrieved:

- submitter_id
- case_id
- primary_site
- disease_type
- diagnoses.vital_status
- demographic.gender
- demographic.ethnicity
- demographic.race
- demographic.state
- demographic.year_of_birth
- demographic.year_of_death
- exposures.alcohol_history
- exposures.alcohol_intensity
- exposures.bmi
- exposures.height
- exposures.weight
- exposures.years_smoked
- exposures.cigarettes_per_day
- created_datetime
- project.project_id

#### get_cases function
At the time of building the pipeline (June 20, 2019) the database held 33605 cases (reflected by the variable max_count).  The variable 'max_count' will need to be updated to retrieve all cases recorded if using the pipeline at a later point in time (current case numbers are listed on the GDC webpage).

Variables such as 'gender' ('demographic.gender') are stored as key-value pairs within dictionaries in a single column under the main variable 'demographic' in the retrieved data frames. Therefore, each data frame holds nine columns/main variables (exceptions exist).
Cases are retrieved in bundles of 1000 (max size per request) and stored in pandas data frames. Dependent on the study the variable 'exposures' was recorded or not and the individual data frames contain nine or eight (exception) columns, respectively. The 'get_cases()' function checks the presence of the eventually missing variable and if necessary inserts an additional row into the respective data frame. Print statements can be used to monitor the dimensions of the individual data frames before and after eventual insertion of the missing column.

#### merge_dfs function
This function merges individual data frames into one main data frame.

#### unpack_dictionaries function
This function unpacks variables in dictionaries and combines the resulting columns with the original data frame
main variables: demographic, exposures, project

#### drop_reorder_columns function
This function drops columns that held the now unpacked dictionaries. Columns of the combined data frame are reordered:

- case_id
- disease_type
- primary_site
- year_of_birth
- year_of_death
- gender
- race
- ethnicity
- state
- height
- weight
- bmi
- alcohol_history
- alcohol_intensity
- years_smoked
- cigarettes_per_day
- created_datetime
- project_id
- id
- submitter_id

#### save_csv function
The combined, reordered data frame is saved as a csv file ('cancer_ordered_raw'). At the time of building this pipeline the combined, reordered data frame combines 47 studies/projects and consists of > 600K data points.


### Part 2: Data cleaning/subsetting, analysis, and visualization

This part of the pipeline enables the user to analyze the data in respect to cancer type and alcohol history of the patient.

#### read_data function
This function reads the data (csv format) and converts it into a pandas data frame.

#### graph_cases_alc_history function
This function outputs a bar graph showing the numbers of entries listing the alcohol history of a patient as 'yes' or 'no', respectively. 

#### numbers_cases_alc_history function
This function outputs the numbers of entries listing the alcohol history of a patient as 'yes' or 'no', respectively (as previously displayed graphically), as well as the number of entries in which alcohol history was recorded.

#### subset_df_alc_hist function
This function subsets the dataset and returns a data frame containing only cases in which the alcohol history of a patient was recorded, i.e. it is specifically denoted as 'yes' or 'no'.

#### plot_top15_primary_sites function
The top 15 primary cancer sites recorded in the main dataset are plotted as a pie chart.

#### plot_top15_primary_sites_alc_hist function
The top 15 primary cancer sites recorded in the alcohol history data subset are plotted as a pie chart.

#### plot_counts_prim_site function
Case counts by primary site (not normalized to number of patients within each group) are returned as a bar graph.

#### normalize_alc_hist function
Returns a data frame listing percentages of patients with alcohol history = 'yes' and alcohol history = 'no' normalized to number of patients within each group.

#### plot_counts_prim_site_normalized function
Percentages of case counts by primary site (normalized to number of patients within each group as displayed in the data frame returned by the previous function) are returned as a bar graph.
