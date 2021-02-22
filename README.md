# Borderline Personality Disorder (BoPD) automatic disease screening tool

# Table of Contents
- [1. Introduction](#1-introduction)
	- [1.1 BoPD overview](#11-bopd-overview)
	- [1.2 BoPD Automatic Screening Initiative](#12-BoPD-Automatic-Screening-Initiative)
	- [1.3 Develop environment](#13-Develop-environment)
	- [1.4 Data used to develop the algorithm (Nan)](#14-Data-used-to-develop-the-algorithm-Nan)
- [2. Preparing input dataset for the screening tool](#2-Preparing-input-dataset-for-the-screening-tool)
- [3. Execution of BoPDscreen](#3-Execution-of-BoPDscreen)
- [4. Reference](#4-Reference)
- [5. License](#5-License)
- [6. Publication](#6-Publication)




## 1. Introduction

### 1.1 BoPD overview
Borderline personality disorder (BoPD) is a psychological disorder that begins in early adulthood and is characterized by ≥5 of the following symptoms: frantic efforts to avoid real/imagined abandonment, a pattern of unstable/intense personal relationships, identity disturbance, impulsivity that is potentially self-damaging, unstable mood, chronic feelings of emptiness, inappropriate/intense anger, paranoid ideation/severe dissociation and recurrent suicidal or self-harming tendencies(American Psychiatric Association, 2013; Limandri, 2018). BoPD affects approximately 1–2% of the general population, and it is estimated that approximately 10% of psychiatric outpatients and 20% of psychiatric inpatients are affected by the disorder (Ellison, Rosenstein, Morgan, & Zimmerman, 2018; Lieb, Zanarini, Schmahl, Linehan, & Bohus, 2004).

BoPD is a complex disorder which has been associated with many comorbidities such as depression, bipolar disorder and anxiety disorders (American Psychiatric Association, 2013; Zanarini et al., 1998a, 1998b). Due to this, patients with BoPD are often misdiagnosed and there is evidence that approximately 40% of patients have received a previous misdiagnosis of bipolar disorder compared with only 10% of patients without BoPD (Ruggero, Zimmerman, Chelminski, & Young, 2010). This highlights an unmet need for effective BoPD screening methods that enable an accurate and timely diagnosis.

### 1.2 BoPD Automatic Screening Initiative

To address the unmet need for effective BoPD screening methods, an initiative has been formed to develop a machine learning algorithm that utilizes electronic health record (EHR) databases to automatically identify patients likely to have BoPD, but without formal diagnosis. Such an algorithm will facilitate efficient real-world clinical decision making for patients with BoPD.

The objective of the borderline personality disorder (BoPD) Automatic Screening Initiative is to advance timely identification of potential BoPD patients. This was made possible by the development of a machine learning algorithm. We built a knowledge-enriched semi-supervised learning framework to learn a classification model.  The algorithm screens electronic health record (EHR) data and automatically identifies patients with highly likely BoPD diagnosis for confirmation of clinical diagnosis.

The algorithm has been developed with two methods of implementation.  The first one is implementation through executable python source code (headless version).  The second one is implementation through WinPython, a portable distribution of Python programming language for Windows.


### 1.3 Develop environment
The portable WinPython can be implemented under windows operation system (Microsoft Windows 10 Enterprise).

### 1.4 Data used to develop the algorithm (Nan)
We use de-identified Electronic Health Record (EHR) from Cerner Health Fact data. Data in Health Facts is directly extracted from Electronic Medical Records (EMR) from hospitals who has data use agreement with Cerner. Encounters may include pharmacy, clinical and microbiology laboratory, admission, and billing information from affiliated patient care locations.  Date and time stamps are included for all admissions, medication orders and dispensing, laboratory orders and specimens, providing a temporal relationship between treatment patterns and clinical information. Cerner Corporation has established operating policies ensure that all data in the Health Facts database are fully de-identified in compliance with the Health Insurance Portability and Accountability Act. Currently Health Facts database contains information from approximately 69 million US patients, with data collected from 2000 to 2018 (most between 2009 and 2018).

## 2. Preparing input dataset for the screening tool
The key steps are demonstrated in the flowchart below.![Flow chart ](/images/flowchart.png)
In next section, more details are provided with sample SQL codes. The SQL codes are for demonstration purpose and please adjust them based on your database.
> INITIAL INPUT: datasets from EHR database
Necessary fields to include: Unique subject identifier, diagnosis codes, visit dates, visit type, demographic information
This process starts with integrating multiple datasets in EHR database. Usually, EHR data is stored as a relational database. Different information are stored in different datasets. For example, demographic information in one dataset, diagnosis information in another dataset, etc. Hence, those pieces of useful information need to be pulled from different tables.

For this screening tool, the essential information includes demographic (age, gender), visit/encounter date (start date and end date), diagnosis codes in each encounter/visit and encounter type. Therefore, the initial step is to locate the necessary information for later use.
> Step 1: Initial inclusion and exclusion
* Step 1.1: Get the patient list 
Once datasets with all needed information are identified, these fields can be pulled together and the initial inclusion and exclusion criteria can be applied.
Since EHR database has a large population, the first filter of inclusion needs to be applied to select the subjects who ever had the diagnosis codes related to the study since 2017-01-01. The inclusion list is provided in [Appendix_1](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/Appendix_1.xlsx). Based on our experience, approximately 10% to 15% of the total population may be retained from this step. However, this percentage might differ in different EHR databases. 
In addition, all encounters between age 18 to 65 are included in the dataset.
From the subjects who met the inclusion rule, those who ever had an ICD-10-CM diagnosis code of F60.3, F00-F09, F70-F79 need to be excluded. (F60.3: Borderline personality disorder; F00-F09: Mental disorder due to known physiological condition; F70-F79: Intellectual disabilities)
> Programming Note
1.	In Appendix_1, the ICD-10 codes are in a format of combination of letters and numbers without any symbols, i.e. F603, however, in EHR systems, ICD-10 codes usually have ‘.’ between numbers, e.g. ‘F60.3’. So, when using Appendix_1, please remove any ‘.’ in your dataset to match the diagnosis code in Appendix_1.
2.	Appendix_1 shall be imported into your database for further data integration and selection. The first column contains all the ICD_10 codes in the inclusion list. The second column is diagnosis_description which won’t be used. It is included here for better understanding the meaning of codes.
3.	Use SQL queries to merge different tables by INNER JOIN on unique keys to create the tables.
4.	Add inclusion conditions in “WHERE” statement of the query. It should contain 18<=age<= 65 and discharged date >= 2017-01-01
5.	Please make sure the discharged_date is in a date format 

>Sample code
```mySQL
/* inclusion step */
create table inclusion_patient_list as
select distinct a.patient_sk
from database.table1 as a 
inner join database.table2 as b 
on a.key1=b.key1  /* link the tables with unique key */                     
inner join database.table3 as c on b.key2=c.key2 
where (translate(a.diagnosis_code,'.','') in (inclusion_list)
 /* inclusion list is the first column of appendix_1 */
 and   a.diagnosis_type = 'ICD10-CM')	   
 and   18<= b.age_in_years <=65 
 and   c.discharged_date >=2017-01-01
;
/* Exclusion step */
/* Create exclusion patient list by putting the exclusion condition in where statement */
create table exclusion_patient_list as 
select distinct a.patient_sk
from database.table1 as a 
inner join database.table2 as b 
on a.key1=b.key1  /* link the tables with unique key */                     
inner join database.table3 as c on b.key2=c.key2 
    where  
 (diagnosis_code = 'F60.3' and diagnosis_type = 'ICD10-CM')
 or 
( substr(dd.diagnosis_code,1,3) in ( 'F00','F01','F02','F03','F04','F05','F06','F07','F08','F09'
 ,'F70','F71','F72','F73','F74','F75','F76','F77','F78','F79' )
 and dd.diagnosis_type = 'ICD10-CM' );  
 
/* Use left join and where b.patient_sk is NULL to exclude the patients in  table 1 */ 
create table patient_list_of_step1 as
        select a.patient_sk
	 from inclusion_patient_list as a	
	 left join
	 exclusion_patient_list as  b
	 on a.patient_sk=b.patient_sk
	 where b.patient_sk is null
```
* Step 1.2 Gather all needed information
Once the patient list is obtained from the step 1.1, the next step is to gather all diagnosis codes since 2017-01-01. You can use similar SQL to select all visits/encounters and their non-missing diagnosis codes by using the patient_sk (unique patient identifier) you get from step 1.1. 
After step 1.2, the output dataset should look exactly like the sample data in [Appendix_2](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/Appendix_2.xlsx).
> Sample code
```mySQL
/* Gather all diagnosis information */
create table your_dataset_of_step_1 as
select distinct pl.patient_sk, a.column1, b.column2
               ,c.column3, …
             from database.patient_list_of_step1 as pl
       inner join database.table1 as a on pl.patient_sk=a.patient_sk
       inner join database.table2 as b on a.key1=b.key1                      
	    inner join database.table3 as c on b.key2=c.key2 
           where a.diagnosis_code is NOT NULL
       and a.diagnosis_type = 'ICD10-CM')	   
	     and   18<= b.age_in_years <=65 
       and   discharged_date >=2017-01-01
```
> Step 2: Data cleaning
Once all diagnosis codes, visit dates, and demographic information are gathered in one dataset, you can start investigating potential abnormalities. Usually, in the EHR system, the demographic information was entered or updated at each visit. Therefore, inconsistency may exist in gender and  year of birth. Another abnormality can happen to the admitted date and discharged date of each visit if wrong dates was entered. And appropriate data cleaning is needed.
Below are possible abnormities to check:
-	Issue 1: Admitted date>discharged date
-	Issue 2: Different subjects were corresponding to the same encounter_id
-	Issue 3: Different birth year was recorded in different visits
-	Issue 4: Different gender was recorded in different visits
-	Issue 5: Missing values

Please follow the data cleaning suggestions as below:
A.	For issue 1 and 2, exclude the records by encounter level. It means only remove the specific rows with those issues.
B.	For issue 3, calculate the year of birth at each encounter by discharged date and age at each encounter (birth year = year of discharged date – age at that visit), if the max(year of birth)- min(year of birth) >3, then exclude this patient.
C.	If there is more than one gender, then use "majority vote" (Normally the gender information is recorded at each visit, and it should be consistent. However, for some instances, different genders are observed over time. Therefore, majority vote should be applied); In addition, if equal vote happens, exclude the patient from the analysis. In addition, the possible values for gender are “Female” and “Male”, other values should be excluded.
D.	Records with missing values in any columns should be removed except for “patient_type_desc” where the Null/missing is allowed. 


> Step 3: Filter subjects with sufficient history
In this step, subjects with sufficient clinical history are retained for the screening tool. These subjects need to have at least five encounters on different discharge dates or at least two emergency visits on different discharge dates. 

>Programming Note
1.	For condition of “at least five encounters on different dates”, select the patients from previous steps and use the condition” having count(distinct(discharged_date))>=5”, create patient-list 1
2.	For condition of “at least two emergency visits”, first identify the visits which are emergency, put them into “emergency group” or set a flag to it. Then select the patients having count(distinct(discharged_date))>=2 when the visit type is emergency, create patient list 2
3.	There might be some overlap in patient-list 1 and 2. Then get a UNION list of these two.

>Sample code
```mySQL
/* create a list of 5+ encounters */
/* patient_sk is the unique patient identifier */
create table pt_of_5plus as
                  select distinct patient_sk
                  from your_dataset_of_step2
                  group by patient_sk
                  having count(distinct(discharged_date))>=5 ;

/* this is the list of 2+ emergency visits */
create table pt_of_2emer as
                  select distinct patient_sk
                  from your_dataset_of_step2
                  where patient_type_desc in ('Emergency')
                  group by patient_sk
                  having count(distinct(discharged_date))>=2;
                 
/* Union above two lists to get list of patients with sufficient clinical history */
create table pt_list_of_step3 as
                  select a.*
                  from pt_of_5plus as a
                  union 
                  select b.* 
                  from pt_of_2emer as b;
/* use this patient list to merge back with previous dataset to get other columns */
create table your_dataset_of_step3 as
                  select b.*
                  from pt_list_of_step3 as a
                  inner join your_dataset_of_step2 as b
                  on a.patient_sk=b.patient_sk ;
```

>	OUTPUT: input dataset for the screening tool
Once all the previous steps are completed, you would already have a patient list who meets all those filtering requirements. Please then merge all the required information of these patients into one single dataset and clean up all the abnormities as specified in the previous section. 
The data specification and a sample dataset can be found in [Appendix_2](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/Appendix_2.xlsx). 
This dataset will be used as the input dataset for the screening tool. Please save it as “.csv” file and place it in the local environment under screening tool folder “BoPDScreeningTool/Application Demo” (refer to step 6 in section 3.2 below). 


## 3. Execution of BoPDscreen 
### 1. Portable version

The screening tool aims at providing a centralized place to process built-in functions and deliver patient screening results. It is based on WinPython, a portable distribution of Python programming language for Windows and be implemented on a regular PC with Windows 8 or higher version, with >=4GB free space on C drive. Additional C drive space is needed for placing the dataset prepared from the previous section and it depends on the size of the dataset. The memory requirement depends on the volume of data and details can be found in the next section.

The zip file attached contains full WinPython application and supporting document needed for execution. Please see the tool maunal [tool_instruction.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/tool_instruction.md) for detailed execution instructions.

 ***Note**: The downloading can take approximate 1 hour to 3 hours depending on the internet speed; to unzip the file, overnight unzipping is recommended.* 

### 2. Headless version

After preparing the input dataset as instructed, set the location of input dataset as default directory path, download attached CCSR coding document (v2020-2) to the same location and run the code file for screening result. Here is a built-in check for input data standards. If data quality check does not pass, here will be a warning message indicating the questionable part and the program will be terminated. Please revisit, fix it and rerun. 

Main parts of the package: 

I.	Load in Dataset

II.	 Data quality check

III.	Data Preprocessing

IV.	Apply pre-defined inclusion/ exclusion criteria.

V.	Feature Engineering

VI.	Screening Model Implementation


## 4. License 

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/LICENSE.md) file for details.

## 5. References

American Psychiatric Association. (2013). *Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition*. Arlington VA: American Psychiatric Association.

Ellison, W. D., Rosenstein, L. K., Morgan, T. A., & Zimmerman, M. (2018). Community and Clinical Epidemiology of Borderline Personality Disorder. *Psychiatr Clin North Am, 41*(4), 561-573. doi:10.1016/j.psc.2018.07.008

Limandri, B. J. (2018). Psychopharmacology for Borderline Personality Disorder. *J Psychosoc Nurs Ment Health Serv, 56*(4), 8-11. doi:10.3928/02793695-20180319-01

Lieb, K., Zanarini, M. C., Schmahl, C., Linehan, M. M., & Bohus, M. (2004). Borderline personality disorder. *Lancet, 364*(9432), 453-461. doi:10.1016/s0140-6736(04)16770-6

Ruggero, C. J., Zimmerman, M., Chelminski, I., & Young, D. (2010). Borderline personality disorder and the misdiagnosis of bipolar disorder. *J Psychiatr Res, 44*(6), 405-408. doi:10.1016/j.jpsychires.2009.09.011

Zanarini, M. C., Frankenburg, F. R., Dubo, E. D., Sickel, A. E., Trikha, A., Levin, A., & Reynolds, V. (1998a). Axis I comorbidity of borderline personality disorder. *Am J Psychiatry, 155*(12), 1733-1739. doi:10.1176/ajp.155.12.1733

Zanarini, M. C., Frankenburg, F. R., Dubo, E. D., Sickel, A. E., Trikha, A., Levin, A., & Reynolds, V. (1998b). Axis II comorbidity of borderline personality disorder. *Compr Psychiatry, 39*(5), 296-302. doi:10.1016/s0010-440x(98)90038-4

## 6. Publication

Characterization of borderline personality disorder using a large electronic health record database: potential for the development of a disease screening algorithm. (In preparation)

Identifying potentially undiagnosed borderline personality disorder from a large electronic health record database to aid the development of an automatic screening initiative. (In preparation)

Screening Borderline Personality Disorder Patients from Real-World Data under Limited Supervision. (In preparation)









