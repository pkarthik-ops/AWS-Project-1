<h1 align="center">AWS Project 1</h1>


# Project Description: Implementation and Analysis of Animal Control Data
* This project involves setting up a comprehensive data analytics platform using AWS services to manage the City of Vancouver’s animal control data.

## Project Title: AWS Data Analytic Platform for the City of Vancouver - Part 1
* This project involves setting up a comprehensive data analytics platform using AWS services to the rate of animals found Matched in the year 2023 and add an Average for count of Matched animals feature additionally to support your analysis.
## Project Objective:
* In this analysis, I have chosen to perform both the descriptive and the predictive analysis using the “Animal Inventory -Lost and Found” dataset from the City of Vancouver which is available in the open data. Self-explanatory In the descriptive analysis, we are working on the percentage of count where from the Total count of Animals noted we obtain Matched count in the year 2024.
* Data Sourse : The data is sourced from external datasets. For example, the dataset for animal control inventory lost and found is accessed via an open data portal. This involves identifying and accessing relevant datasets that will be used for analysis.
![Screenshot (46)](https://github.com/user-attachments/assets/64e8d3ec-c9a7-44f7-a86b-e3e0353e2244)




  
## Methodology:
* The process of DAP designing and implementation is as follows.
### 1. Data Ingestion
Key analytical questions driving the project include:
- 1.	What is the rate of animals found Matched in the year 2023 and add an Average for count of Matched animals feature additionally to support your analysis.
- First we need to create a bucket in S3 and put the raw data inside it which we get from City of Vancouver.
![Screenshot (47)](https://github.com/user-attachments/assets/3eb7338d-8a58-4c4a-befb-59f752eb0956)



- Raw, transformed and curated Data have been created in a new bucket in Aws at respective folder locations. Now let us push the dataset into the raw folder generated on the bucket described earlier.
![Screenshot (49)](https://github.com/user-attachments/assets/5884b76e-5e9a-4c10-9650-d7c92077017f)






### 2. Data Profiling
Shown below is the data profiling and cleaning which we perform to store the new transformed data in the new transformed bucket
![Screenshot (65)](https://github.com/user-attachments/assets/dddb1dd2-5a0c-428a-b86f-2e95485fe981)



After ingestion we are going to focus on data profiling which will indicate to the summary of our data set any missing value or any relation that we can explore. To accomplish this first it will be necessary to develop a new transform bucket for all of this information. Then we have to navigate to AWS Glue Data brew, there we will have to create a new dataset wherein the data profiling will be run to get all the profiling data required for cleaning the data. 
![Screenshot (79)](https://github.com/user-attachments/assets/115ed820-560b-4ca2-b856-69146a535e8a)
- Result of profiling from the dataset

![Screenshot (66)](https://github.com/user-attachments/assets/7d8fe05c-95b5-4f37-9b89-7df1bdb0c9e3)


### 3. Data Cleaning
- When we complete doing the profiling there are certain irregularities we have to correct and can easily discern from the results of profiling.
 We make a project from the dataset and in this project all type of rules of data cleaning should be applied to make out data and to generate a new recipe.
![Screenshot (67)](https://github.com/user-attachments/assets/a7e639e8-44b0-436a-828c-87b4d445dd3d)


You must create a job after you have formulated a recipe for your data and then execute the job. Within the job you have to say what kind of output you want and where to put it at. 
We do produce two types of output one type of output which is in CSV format this is output that is friendly to the user and the second type of output which is friendly to the system, and this is in PARQUET format.
![Screenshot (69)](https://github.com/user-attachments/assets/b8c63901-d150-4c8b-aee2-f7aa89113142)


Cleaned system data
![Screenshot (70)](https://github.com/user-attachments/assets/7d702e79-585e-474f-a278-7976b9288315)


![Screenshot (71)](https://github.com/user-attachments/assets/81f998d5-3da8-44a7-acce-0b579f1a9987)



### 4. Data Pipeline Design
- AWS Glue is used to create the data pipeline, automating the ETL process.
- Finally, after carrying out the data profiling and data cleaning steps; we have our final transformed data which have no null or invalid value in it. Now we are ready in the next step of our descriptive analysis part where our task first is to find out in percentage how many washrooms has wheel access and secondly to look them individually on the basis of type.
Count of Animals found = (Matched_state_count/right_Total_state_count)*100
We find that it is necessary to obtain the percentage of count of animals lost. Now let it be our turn to design our Day 2 ETL pipeline to help answer this question.
- Draw.io diagram for the whole process until the ETL pipeline implementation
![Screenshot (72)](https://github.com/user-attachments/assets/d2aacd63-29ce-4024-b0d1-13391516a149)


* Now we need to go to AWS Glue and start building our visual ETL pipeline
1.	Extract the Status of animals data from cleaned data folder in S3 bucket.
2.	Transformed the schema of the table by Dropping Extra Column from the table.
3.	Then we have divided the pipeline in 2 parts: one with the filtered data & another  one with total data.
4.	Then we put a filter transformation in which we filter out the count of animals that were found Matched
5.	Then we transform it by aggregating the Matched animals and then grouping it by its breed.
6.	Then we rename the aggregated column as “Matched_Count” to make it more suitable for our analysis.
7.	On the other hand we aggregate the total number of animals Matched and lost.
8.	Then we rename the aggregate column as “Total_State_Count” to make it more suitable for our analysis.
9.	Then we again rename the join key by adding the right_ as a prefix before joining.
10.	Then we merge the data sets as an outer join to include all the cells.
11.	Then we transform the data to calculate the % of count of animals found Matched count (Matched_Count / Total_State_Count)* 100.
12.	Now we load the data for separate types one for user in csv format and one for system in Parquet format.
13.	Then we save and run the pipeline.

- The data pipeline is designed to facilitate the smooth flow and transformation of data throughout the system.
![Screenshot (73)](https://github.com/user-attachments/assets/d00ac6a5-9d4d-4c4e-acde-1b4c8c66e13c)

- The job for storing the ETL pipeline result
![Screenshot (74)](https://github.com/user-attachments/assets/fb841cc2-09eb-4819-8fa9-5f6c18919923)



### 5. Exploratory Analysis
- In the exploratory analysis we are calculating the average of Count of animals that were found Matched.
Let’s go through the steps for the exploratory analysis.
- Step 1: Data Ingestion
Same as the one we did above for descriptive analysis.
- Step 2: Data Profiling
Same as descriptive analysis
- Step 3: Data Cleaning
This step is like what we did for out descriptive analysis.
- Step 4: Data Pipeline Design
So after doing data profiling and data cleaning the final transformed data which is cleaned does not contain any null or invalid values. Now we can go to our descriptive analysis part where we have to know how many parking tickets have been committed and also individually based on their types.
Average = (Matched_count/Total_State_Count)/ 2
Now we need to create the alterations for our ETL pipeline to answer this question.
- Draw.io diagram for the whole process until the ETL pipeline implementation
![Screenshot (75)](https://github.com/user-attachments/assets/730c95d4-bc6d-4c21-ba82-701c3b8f4e92)


- The average we got after running the pipeline is 0.5
Below is the ETL Visual Pipeline:
- Second ETL pipeline implementation
![Screenshot (76)](https://github.com/user-attachments/assets/3ecb518c-ced7-4c9f-9434-5d6ab8a8b7ed)

- Second job for storing the ETL pipeline result
![Screenshot (77)](https://github.com/user-attachments/assets/c64a7402-6a59-4ab1-af34-0a373ac047e4)



### DAP Estimated Cost
-The AWS Pricing Calculator gives an accurate estimate of cost of a number of AWS services out of the many available. Or if you want to observe a scenario for a single user the cost computed for a monthly operation is approximately $1.43
![Screenshot (78)](https://github.com/user-attachments/assets/0d4a4185-f850-444c-9eba-bad042fa78b7)










