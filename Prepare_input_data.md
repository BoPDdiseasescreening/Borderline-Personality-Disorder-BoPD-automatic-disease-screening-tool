
### INITIAL INPUT: datasets from EHR database
Necessary fields to include: Unique subject identifier, diagnosis codes, visit dates, visit type, demographic information
This process starts with integrating multiple datasets in EHR database. Usually, EHR data is stored as a relational database. Different information are stored in different datasets. For example, demographic information in one dataset, diagnosis information in another dataset, etc. Hence, those pieces of useful information need to be pulled from different tables.

For this screening tool, the essential information includes demographic (age, gender), visit/encounter date (start date and end date), diagnosis codes in each encounter/visit and encounter type. Therefore, the initial step is to locate the necessary information for later use.

#### Step 1: Initial inclusion and exclusion
* Step 1.1: Get the patient list 
Once datasets with all needed information are identified, these fields can be pulled together and the initial inclusion and exclusion criteria can be applied.
Since EHR database has a large population, the first filter of inclusion needs to be applied to select the subjects who ever had the diagnosis codes related to the study since 2017-01-01. The inclusion list is provided in [Appendix_1](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/Appendix_1.xlsx). Based on our experience, approximately 10% to 15% of the total population may be retained from this step. However, this percentage might differ in different EHR databases. 
In addition, all encounters between age 18 to 65 are included in the dataset.
From the subjects who met the inclusion rule, those who ever had an ICD-10-CM diagnosis code of F60.3, F00-F09, F70-F79 need to be excluded. (F60.3: Borderline personality disorder; F00-F09: Mental disorder due to known physiological condition; F70-F79: Intellectual disabilities)
#### Programming Note
1.	In Appendix_1, the ICD-10 codes are in a format of combination of letters and numbers without any symbols, i.e. F603, however, in EHR systems, ICD-10 codes usually have ‘.’ between numbers, e.g. ‘F60.3’. So, when using Appendix_1, please remove any ‘.’ in your dataset to match the diagnosis code in Appendix_1.
2.	Appendix_1 shall be imported into your database for further data integration and selection. The first column contains all the ICD_10 codes in the inclusion list. The second column is diagnosis_description which won’t be used. It is included here for better understanding the meaning of codes.
3.	Use SQL queries to merge different tables by INNER JOIN on unique keys to create the tables.
4.	Add inclusion conditions in “WHERE” statement of the query. It should contain 18<=age<= 65 and discharged date >= 2017-01-01
5.	Please make sure the discharged_date is in a date format 

#### Sample code
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
#### Step 1.2 Gather all needed information
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
#### Step 2: Data cleaning
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


#### Step 3: Filter subjects with sufficient history
In this step, subjects with sufficient clinical history are retained for the screening tool. These subjects need to have at least five encounters on different discharge dates or at least two emergency visits on different discharge dates. 

#### Programming Note
1.	For condition of “at least five encounters on different dates”, select the patients from previous steps and use the condition” having count(distinct(discharged_date))>=5”, create patient-list 1
2.	For condition of “at least two emergency visits”, first identify the visits which are emergency, put them into “emergency group” or set a flag to it. Then select the patients having count(distinct(discharged_date))>=2 when the visit type is emergency, create patient list 2
3.	There might be some overlap in patient-list 1 and 2. Then get a UNION list of these two.

#### Sample code
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

### OUTPUT: input dataset for the screening tool
Once all the previous steps are completed, you would already have a patient list who meets all those filtering requirements. Please then merge all the required information of these patients into one single dataset and clean up all the abnormities as specified in the previous section. 
The data specification and a sample dataset can be found in [Appendix_2](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/Appendix_2.xlsx). 
This dataset will be used as the input dataset for the screening tool. Please save it as “.csv” file and place it in the local environment under screening tool folder “BoPDScreeningTool/Application Demo” (refer to next section). 
