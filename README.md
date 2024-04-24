# Analysis-Of-Renewable-Energy-Approval-Projects-Using-SQL

## Table of Content
 
 - [Introduction](#introduction)
 - [Data Source](#data-source)
 - [Tools](#tools)
 - [Data Cleaning](#data-cleaning)
 - [Data Analysis](#data-analysis)
 - [Conclusion](#conclusion)
   
### Introduction
This data analysis project aims to:
1.	find out the main renewable energy project type carried out by the Ministry of Environment and Climate Change (MOECC)
2.	identify the various projects and project type that were not approved as at when this data was compiled
3.	know the renewable energy projects whose applications were either returned by the Ministry of Environment and Climate Change (MOECC) or withdrawn by the proponents
4.	determine list of regions that benefited from the renewable energy projects by Ministry of Environment and Climate Change (MOECC)

### Data Source
I have downloaded a CSV file titled “Renewable Energy Approval Projects” form the https://data.ontario.ca/ website. The file contains a listing of the renewable energy projects that have submitted an application to the Ministry of Environment and Climate Change (MOECC) for a Renewable Energy Approval. For my personal project I will be querying this file to extract all possible insights that can be of benefit. 

### Tools
- Excel -Data Cleaning
- SQL - Data Analysis
  
### Data Cleaning
To clean and prepare the CSV data for SQL, I performed some data cleaning tasks:
1.	I Renamed the columns to make them more SQL-friendly and human-readable
2.	I Checked for and remove duplicate rows where necessary.
3.	I Checked for missing values in the dataset and decided to remove rows with missing values, 
4.	I Performed some necessary data transformations like creating new columns, aggregating data, applying excel operations e.g. VLOOKUP, created more sheets and saved each sheet in a different Excel book to make importing into pgAdmin seamless
5.	Once I was done cleaning and preparing the data, I saved it to a new CSV file
6.	I then imported the cleaned CSV data into the PgAdminSQL database.

### Data Analysis

~~~sql
 select Distinct(Project_type) from Project
~~~

### Insight
These are the 3 main renewable energy project type carried out by the Ministry of Environment and Climate Change (MOECC)
- Bioenergy
- Solar
- Wind

  ~~~sql
  select  b.application_status, count(a.project_type) as Total_projects from project a
  inner join application b
  on a.project_id=b.project_id
  group by b.application_status
  ~~~

### Insight
This gives a little understanding of the job done by the ministry of renewable energy resources, the number of applications they have approved or rejected over time.
There are 196 Approved project, 2 refused, 8 projects applications were returned or withdrawn, 22 projects are undergoing screening for completeness and 4 projects are under technical review. This findings can be used to assay the organizations effectiveness in handling projects  when comparing the present year to the previous.                  

 ~~~sql
select project_name, project_type from project
where project_id in
	(select project_id from application
	where application_status='Approved')
 ~~~

### Insight
The total list of approved projects  and project types as at when this data was compiled

 ~~~sql
select count(*) as Total_approved from project
where project_id in
	(select project_id from application
	where application_status='Approved')
 ~~~
### Insight
The total number of approved projects across the 3 project types was 196

~~~sql
select a.project_name, a.project_type, b.application_status from project a
inner join application b
on a.project_id=b.project_id
where application_status like '%Returned%' and project_type='Wind'
~~~

### Insight
This query is further specific as it reveals the Wind renewable energy projects that has their applications either returned by the Ministry of Environment and Climate Change (MOECC) or withdrawn by the proponents. A total of 5 projects.

~~~Sql
select project_name, project_description, Project_type
from project where project_description like '10 MW%'
~~~
### Insight
Here I try to know the list and number of solar projects that had 10 MW solar installed for the proponent.

## Conclusion:
1.	The renewable energy projects carried out by the Ministry of Environment and Climate Change (MOECC) are of 3 types – wind, solar and Bioenergy
2.	The MOECC serves 5 regional zones in Ontario
3.	Out of 232 projects, 196 were approved as of when this data was compiled, and 2 were refused, 8 Returned, 22 screening for completeness and 4 under technical review.
