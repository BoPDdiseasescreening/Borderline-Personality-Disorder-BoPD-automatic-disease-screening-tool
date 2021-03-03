# Borderline Personality Disorder (BoPD) automatic disease screening tool

# Table of Contents
- [1. Introduction](#1-introduction)
	- [1.1 BoPD overview](#11-bopd-overview)
	- [1.2 BoPD Automatic Screening Initiative](#12-BoPD-Automatic-Screening-Initiative)
	- [1.3 Develop environment](#13-Develop-environment)
	- [1.4 Data used to develop the algorithm (Nan)](#14-Data-used-to-develop-the-algorithm-Nan)
- [2. Preparation of input dataset for the screening tool](#2-Preparation-of-dataset-for-the-screening-tool)
- [3. Execution of BoPDscreen](#3-Execution-of-the-screening-tool)
	- [3.1 Portable version](#31-Portable-version)
	- [3.2 Headless version](#32-Headless-version)
- [4. Reference](#4-Reference)
- [5. License](#5-License)
- [6. Publication](#6-Publication)




## 1. Overview

Borderline personality disorder (BoPD) is a serious psychological condition that affects 1–2% of the general population, and approximately 10% and 20% of psychiatric outpatients and inpatients, respectively [1, 2]. The disorder is characterized by chronic disinhibition, extreme sensitivity, volatile emotions, self-harm, impulsive behaviors and high mortality rate due to suicide (up to 10%) [3, 4]. Indeed, the suicide rate amongst patients with BoPD is approximately 50 times higher than that of the general population [2]. In addition, BoPD is a complex personality disorder that has many comorbid conditions, such as anxiety, depression, and bipolar disorder [5-7]. As such, patients with BoPD are often misdiagnosed, with almost 40% reporting a previous misdiagnosis; the corresponding proportion among patients with other psychological disorders is only 10% [8]. 
Misdiagnosis of BoPD has serious clinical implications given distinct therapies are developed for each disorder [8], which can result in patients receiving inappropriate treatment. 

To advance the timely identification of misdiagnosed or under-diagnosed patients with BoPD, we have developed a machine learning algorithm utilizing electronic health record (EHR) data to automatically screen patients likely to have BoPD, but without a formal diagnosis. Our overarching goal is to facilitate and inform real-world clinical decision-making by enabling earlier identification of BoPD through automated screening of EHRs. 

De-identified EHRs from the US-based Cerner Health Facts database were used in the screening algorithm development. A clinical expert in BoPD has reviewed 456 records from the database and rated patients on their likelihood of having BoPD (these patient did not have formal BoPD diagnosis). The deidentified EHRs and the clinical expert's ratings are the data foundation to the screening algorithm. Details of the algorithm development and data can be found in the publications.

In addition, we have integrated the algorithm into a screening tool in both portable version ([section 3.1](#31-Portable-version))) and headless version ([3.1 Portable version](#31-Portable-version)) for implementation. The portable version is developed in WinPython with graphical interface, where WinPython is a portable distribution of Python programming language for Windows, and therefore it only requires Windows operating system without the need to install Python. The headless version includes executable Python source code and therefore it requires an environment to execute Python (version 3.8). Prior to using either version of the tool, patient EHR data needs to be preprocessed in specific format as the input dataset for the screening tool (details see [2. Preparing input dataset for the screening tool](#2-Preparing-input-dataset-for-the-screening-tool)). 

This screening tool has been rolled out and is being piloted at sites in US. 

## 2. Preparation of input dataset for the screening tool
The key steps are demonstrated in the flowchart below.![Flow chart ](/images/flowchart.png)
More details are provided with sample SQL codes in the data preparation manual [Prepare_input_data.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/Prepare_input_data.md). The SQL codes are for demonstration purpose and please adjust them based on your database.



## 3. Execution of the screening tool
### 3.1 Portable version

The screening tool aims at providing a centralized place to process built-in functions and deliver patient screening results. It is based on WinPython, a portable distribution of Python programming language for Windows and be implemented on a regular PC with Windows 8 or higher version, with >=4GB free space on C drive. Additional C drive space is needed for placing the dataset prepared from the previous section and it depends on the size of the dataset. The memory requirement depends on the volume of data and details can be found in the next section.

The zip file attached contains full WinPython application and supporting document needed for execution. Please see the tool maunal [tool_instruction.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/tool_instruction.md) for detailed execution instructions.

 ***Note**: The downloading can take approximate 1 hour to 3 hours depending on the internet speed; to unzip the file, overnight unzipping is recommended.* 

### 3.2 Headless version

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









