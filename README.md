# Borderline Personality Disorder (BoPD) automatic disease screening tool

# Table of Contents
- [1. Overview](#1-overview)
- [2. Preparation of input dataset for the screening tool](#2-preparation-of-input-dataset-for-the-screening-tool)
- [3. Execution of BoPDscreen](#3-Execution-of-the-screening-tool)
	- [3.1 Portable version](#31-Portable-version)
	- [3.2 Headless version](#32-Headless-version)
- [4. License](#4-License)
- [5. References](#5-References)
- [6. Publication & Conference](#6-publication--conference)
- [7. Acknowledgement](#7-acknowledgement)




## 1. Overview

Borderline personality disorder (BoPD) is a serious psychological condition that affects 1–2% of the general population, and approximately 10% and 20% of psychiatric outpatients and inpatients, respectively [1, 2]. The disorder is characterized by chronic disinhibition, extreme sensitivity, volatile emotions, self-harm, impulsive behaviors and high mortality rate due to suicide (up to 10%) [3, 4]. Indeed, the suicide rate amongst patients with BoPD is approximately 50 times higher than that of the general population [2]. In addition, BoPD is a complex personality disorder that has many comorbid conditions, such as anxiety, depression, and bipolar disorder [5-7]. As such, patients with BoPD are often misdiagnosed, with almost 40% reporting a previous misdiagnosis; the corresponding proportion among patients with other psychological disorders is only 10% [8]. 
Misdiagnosis of BoPD has serious clinical implications given distinct therapies are developed for each disorder [8], which can result in patients receiving inappropriate treatment. 

To advance the timely identification of misdiagnosed or under-diagnosed patients with BoPD, we have developed a machine learning algorithm utilizing electronic health record (EHR) data to automatically screen patients likely to have BoPD, but without a formal diagnosis. Our overarching goal is to facilitate and inform real-world clinical decision-making by enabling earlier identification of BoPD through automated screening of EHRs. 

Using de-identified structured electronic health records (EHRs) from the US-based Cerner Health Fact database and a clinical expert’s rating of the likelihood of having BoPD for 456 potential BoPD patient records from the database, we have developed the BoPD screening algorithm. The screening algorithm to identify individuals highly likely to have BoPD utilizes information such as patients’ diagnosis history, demographics, encounter types (emergency room, outpatient visit and hospitalization) and frequencies as inputs. The algorithm incorporates a clinical understanding of BoPD, including associations with bipolar disorder, and suicidal/intentional self-harm; a hallmark of the disease. Due to the large gap between vast unlabeled patient EHR data relative to a few hundred labelled patients (i.e. expert ratings), we implemented a knowledge-enriched, semi-supervised learning framework.

Results of the screening algorithm were encouraging. The out-of-sample test results show an area under the receiver operating characteristics (AUROC) of 0.84, and positive predictive value of 0.72 indicating that for every 10 patients identified by the algorithm, on average 7 of them are highly likely to be patients with BoPD. Accuracy is 0.82 and sensitivity and specificity are 0.54 and 0.92, respectively. 

Details of the data and algorithm development can be found in [Publication & Conference](#6-publication--conference).

In addition, we have integrated the algorithm into a screening tool in both portable version ([section 3.1](#31-Portable-version)) and headless version ([section 3.2](#32-Headless-version)) for implementation. The portable version is developed in WinPython with graphical interface, where WinPython is a portable distribution of Python programming language for Windows, and therefore it only requires Windows operating system without the need to install Python. The headless version includes executable Python source code and requires Python (version 3.8) environment but does not generate a graphical user interface. Prior to using either version of the tool, patient EHR data needs to be preprocessed in specific format as the input dataset for the screening tool (details see [section 2](#2-Preparing-input-dataset-for-the-screening-tool)). 


## 2. Preparation of input dataset for the screening tool
The key steps are demonstrated in the flowchart below.![Flow chart ](/images/flowchart.png)
More details are provided with sample SQL codes in the data preparation manual [Prepare_input_data.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/Prepare_input_data.md). The SQL codes are for demonstration purpose and please adjust them based on your database.



## 3. Execution of the screening tool
### 3.1 Portable version 

The screening tool aims at providing a centralized place to process built-in functions and deliver patient screening results. The tool is embeded in a TKinter GUI as an interactive interface for functionality. It is based on WinPython, a portable distribution of Python programming language for Windows and be implemented on a regular PC with Windows 8 or higher version, with >=4GB free space on C drive. Additional C drive space is needed for placing the dataset prepared from the previous section and it depends on the size of the dataset. The memory requirement depends on the volume of data and details can be found in the next section. The vesrion used for development is Winpython64-3.8.3.0 which could be downloaded from [Sourceforge](https://sourceforge.net/projects/winpython/files/WinPython_3.8/3.8.3.0/). 

The zip file [BoPDdiseasescreening.zip](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/BoPDScreeningTool.zip) contains supporting document needed for execution. Please download the file and refer to the tool maunal [tool_instruction.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/tool_instruction.md) for detailed execution instructions.

 ***Note**: Executing on WinPython is optional. It helps to make the tool as a protable distribtuion even for users without Python or with difficulty installing Python on the computer. If here is installed Python3.8 already, the screening tool script from the zip file could be executed directly through local Python.* 

### 3.2 Headless version

If graphical user interface is not needed, the headless version is provided to give the list of patient screening results through executing the script directly under python environment. The zip file [BoPDdiseasescreening_headless.zip](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/BoPDScreening_headless.zip) contains supporting document needed for execution. Please downlaod the file and unzip it under C:/ root and refer to read me.txt file for further instructions.


## 4. License 

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/LICENSE.md) file for details.

## 5. References

1.	Ellison, W.D., et al., Community and Clinical Epidemiology of Borderline Personality Disorder. Psychiatr Clin North Am, 2018. 41(4): p. 561-573.
2.	Lieb, K., et al., Borderline personality disorder. Lancet, 2004. 364(9432): p. 453-61.
3.	Gunderson, J.G., et al., Borderline personality disorder. Nat Rev Dis Primers, 2018. 4: p. 18029.
4.	Sansone, R.A. and L.A. Sansone, BORDERLINE PERSONALITY DISORDER IN THE MEDICAL SETTING: Suggestive Behaviors, Syndromes, and Diagnoses. Innov Clin Neurosci, 2015. 12(7-8): p. 39-44.
5.	Zanarini, M.C., et al., Axis II comorbidity of borderline personality disorder. Compr Psychiatry, 1998. 39(5): p. 296-302.
6.	Zanarini, M.C., et al., Axis I comorbidity of borderline personality disorder. Am J Psychiatry, 1998. 155(12): p. 1733-9.
7.	National Institute of Mental Health. Borderline Personality Disorder. 2017 October 2020]; Available from: https://www.nimh.nih.gov/health/topics/borderline-personality-disorder/index.shtml.
8.	Ruggero, C.J., et al., Borderline personality disorder and the misdiagnosis of bipolar disorder. J Psychiatr Res, 2010. 44(6): p. 405-8.

## 6. Publication & Conference

Characterization of borderline personality disorder using a large electronic health record database: potential for the development of a disease screening algorithm. (In preparation)

Identifying potentially undiagnosed borderline personality disorder from a large electronic health record database to aid the development of an automatic screening initiative. (In preparation)

Screening Borderline Personality Disorder Patients from Real-World Data under Limited Supervision. (In preparation)

Shao, N., Goodman, M., Zang, C., Zhu, Z., Tamas, Z., Ovens, R., Koczon-Jaremko, A., Sharma, V.M. (2021, August). Automatic disease screening of borderline personality disorder using electronic health records (EHR). Joint Statistical Meetings (JSM) 2021, Seattle, WA, United States.

Goodman, M., Zhu, Z., Tamas, Z., Sharma, V.M., Shao, N.(2021, April). Automatic screening of borderline personality disorder using electronic health record data. North American Society for the Study of Personality Disorders (NASSPD), virtual conference.

Goodman, M., Zhu, Z., Tamas, Z., Sharma, V.M., Shao, N.(2021, May). Identifying both diagnosed and potentially undiagnosed patients with borderline personality disorder from a large electronic health record database. American Psychiatric Association Annual Meeting (APA), virtual conference.

Shao, N., Goodman, M., Zang, C., Sharma, V.M. (2020, December). Disease screening for a personality disorder using Electronic Health Records (EHR) data. International Chinese Statistical Association Applied Statistics Symposium (ICSA) 2020, virtual conference.


## 7. Acknowledgement
Boehringer Ingelheim International GmbH supported this project. We collaborated with Dr. Marianne Goodman from James J Peters VA Medical Center / Icahn School of Medicine at Mount Sinai, who is the clinical expert in BoPD, and Dr. Chengxi Zang and Dr. Fei Wang from Weill Cornell Medicine, who are the machine learning / AI experts in health data science. 





