# Borderline Personality Disorder (BoPD) disease screening algorithm

# Table of Contents
- [1. Overview](#1-overview)
- [2. Preparation of input dataset for the screening algorithm](#2-preparation-of-input-dataset-for-the-screening-algorithm)
- [3. Execution of the screening algorithm](#3-Execution-of-the-screening-algorithm)
	- [3.1 Portable version](#31-Portable-version)
	- [3.2 Headless version](#32-Headless-version)
- [4. License](#4-License)
- [5. References](#5-References)
- [6. Publication & Conference](#6-publication--conference)
- [7. Acknowledgement](#7-acknowledgement)




## 1. Overview

Borderline personality disorder (BoPD) is a serious psychological condition that affects 1–2% of the general population, and approximately 10% and 20% of psychiatric outpatients and inpatients, respectively [1, 2]. BoPD is characterized by chronic disinhibition, extreme sensitivity, volatile emotions, self-harm, impulsive behaviors and high mortality rate due to suicide (up to 10%) [3, 4]. The diagnosis of BoPD is often overlooked with almost 40% reporting a previous misdiagnosis compared to only 10% of patients with other psychological disorders [5], and even when identified, many patients do not receive appropriate treatments. 

We believe that using ML to perform automated screening of EHRs which contain routinely collected longitudinal patient health information is a potentially helpful strategy to address the unmet need for the identification of possible BoPD patients who are mis- or not diagnosed. Our aim is, to provide an additional resource to facilitate clinical decision making and promote the development of digital medicine. This approach is not meant to replace the clinical gold-standard screening (i.e. semi-structured interview with patients) nor to make a diagnosticis decision, and HCPs should rely on their own judgement to make clinical decisions for an individual patient. In addition, the algorithm is developed using US coding system (ICD-10-CM), and therefore it is applicable in US at the moment.

The proposed method follows a two-step approach by first selecting potential patients based on rules on the presence of comorbidities and characteristics commonly associated with BoPD when BoPD has not been formally diagnosed, and then narrowing down the recommendation to health care professionals (HCPs) by predicting whether the patients are most likely to have BoPD using ML. Our clinical expert rated the records of 456 patients randomly selected from a large-scale US-based de-identified EHR database, which was adopted as the initial training and test data sets for ML model development. We then built a 90 times larger training dataset to improve the generalizability of the ML algorithm. The performance of the ML algorithm shows a high consistency with our clinical expert's ratings, with AUROC 0.837 (0.778-0.892), positive predictive value 0.717 (0.583-0.836), accuracy 0.820 (0.768-0.873), sensitivity 0.541 (0.417-0.667) and specificity 0.922 (0.880-0.960). 

When developing the screening algorithm, model interpretability was important because providing clarity and transparency into how the algorithm makes decision is critical to us. In fact, we have explored several machine learning models including “black-box” models, and the interpretable model stands out which is shared here. Details of the data and algorithm development can be found in [Publication & Conference](#6-publication--conference).

In addition, we have integrated the algorithm into a screening tool in both a portable version ([section 3.1](#31-Portable-version)) and a headless version ([section 3.2](#32-Headless-version)) for implementation. The portable version has a graphical interface, and it can be executed in WinPython (a portable distribution of Python programming language for Windows without the need to install Python) or Python 3.8. The headless version includes executable Python source code without a graphical user interface and it requires Python 3.8. Prior to using either version of the tool, patient EHR data needs to be processed in a specific format as the input dataset for the screening tool. The instruction and sample SQL code for such EHR data processing are included in a user manual [section 2](#2-Preparing-input-dataset-for-the-screening-algorithm)). 


## 2. Preparation of input dataset for the screening algorithm
The key steps are demonstrated in the flowchart below.![Flow chart ](/images/flowchart.png)
More details are provided with sample SQL codes in the data preparation manual [Prepare_input_data.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/Prepare_input_data.md). The SQL codes are for demonstration purpose and please adjust them based on your database.



## 3. Execution of the screening algorithm
### 3.1 Portable version 

The tool in portable version is embeded in a TKinter GUI as an interactive interface for functionality. If installation of python 3.8 is challenging, we suggest to consider WinPython for implementation. 

To execute in WinPython, please follow these steps. 
	1) Download Winpython64-3.8.3.0 from [Sourceforge](https://sourceforge.net/projects/winpython/files/WinPython_3.8/3.8.3.0/) and save it under C:/ root. WinPython requires a regular PC with Windows 8 or higher version, with >=4GB free space on C drive. Additional C drive space is needed for placing the dataset prepared from the previous section and it depends on the size of the dataset. The memory requirement depends on the volume of data and details can be found in tool maunal [tool_instruction.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/tool_instruction.md). 
	2) Download the zip file [BoPDdiseasescreening.zip](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/BoPDScreeningTool.zip) which contains codes/supporting documents needed for executing the screening tool and save it under C:/ root.  
	3) Refer to the tool maunal [tool_instruction.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/tool_instruction.md) for detailed execution instructions.
	

To execute in Python 3.8, please download the zip file [BoPDdiseasescreening.zip](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/BoPDScreeningTool.zip) and follow instructions in the tool maunal [tool_instruction.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/tool_instruction.md).


### 3.2 Headless version

If graphical user interface is not needed, the headless version is provided to give the list of patient screening results through executing the script directly under python environment. The zip file [BoPDdiseasescreening_headless.zip](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/BoPDScreening_headless.zip) contains codes/supporting document needed for execution. Please downlaod the file and unzip it under C:/ root and refer to *read me.txt* file for further instructions.


## 4. License 

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/LICENSE.md) file for details.

## 5. References

1.	Ellison, W.D., et al., Community and Clinical Epidemiology of Borderline Personality Disorder. Psychiatr Clin North Am, 2018. 41(4): p. 561-573.
2.	Lieb, K., et al., Borderline personality disorder. Lancet, 2004. 364(9432): p. 453-61.
3.	Gunderson, J.G., et al., Borderline personality disorder. Nat Rev Dis Primers, 2018. 4: p. 18029.
4.	Sansone, R.A. and L.A. Sansone, BORDERLINE PERSONALITY DISORDER IN THE MEDICAL SETTING: Suggestive Behaviors, Syndromes, and Diagnoses. Innov Clin Neurosci, 2015. 12(7-8): p. 39-44.
5.	Ruggero, C.J., et al., Borderline personality disorder and the misdiagnosis of bipolar disorder. J Psychiatr Res, 2010. 44(6): p. 405-8.


## 6. Publication & Conference

Development of a screening algorithm for borderline personality disorder using electronic health records. (In preparation) 

Dubovsky, S., Ghosh, B., Marshall, D., Sharma, V., Shao, N., Ovens, R., Wan, O., Rast, R., ORourke, J., Goodman, M. (2021, October) Initial pilot results of borderline personality disorder screening algorithm using electronic health records. European Congress of Neuropsychopharmacology (ECNP), Lisbon, Portugal.

Goodman, M., Yang, L., Sharma, V.M., Shao, N. (2021 October) Automatic Screening of Borderline Personality Disorder Using Electronic Health Record Data. Psych Congress, San Antonio, TX, United States.

Shao, N., Goodman, M., Zang, C., Zhu, Z., Tamas, Z., Ovens, R., Koczon-Jaremko, A., Sharma, V.M. (2021, August). Automatic disease screening of borderline personality disorder using electronic health records (EHR). Joint Statistical Meetings (JSM) 2021, Seattle, WA, United States.

Goodman, M., Zhu, Z., Tamas, Z., Sharma, V.M., Shao, N.(2021, April). Identifying both diagnosed and potentially undiagnosed patients with borderline personality disorder from a large electronic health record database. North American Society for the Study of Personality Disorders (NASSPD), virtual conference.

Goodman, M., Zhu, Z., Tamas, Z., Sharma, V.M., Shao, N.(2021, May). Identifying both diagnosed and potentially undiagnosed patients with borderline personality disorder from a large electronic health record database. American Psychiatric Association Annual Meeting (APA), virtual conference.

Shao, N., Goodman, M., Zang, C., Sharma, V.M. (2020, December). Disease screening for a personality disorder using Electronic Health Records (EHR) data. International Chinese Statistical Association Applied Statistics Symposium (ICSA) 2020, virtual conference.


## 7. Acknowledgement
Boehringer Ingelheim International GmbH supported this project. We collaborated with Dr. Marianne Goodman from James J Peters VA Medical Center / Icahn School of Medicine at Mount Sinai, who is the clinical expert in BoPD, and Dr. Chengxi Zang and Dr. Fei Wang from Weill Cornell Medicine, who are the machine learning / AI experts in health data science. 





