# Borderline Personality Disorder (BoPD) automatic disease screening tool

## BoPD overview
Borderline personality disorder (BoPD) is a psychological disorder that begins in early adulthood and is characterized by ≥5 of the following symptoms: frantic efforts to avoid real/imagined abandonment, a pattern of unstable/intense personal relationships, identity disturbance, impulsivity that is potentially self-damaging, unstable mood, chronic feelings of emptiness, inappropriate/intense anger, paranoid ideation/severe dissociation and recurrent suicidal or self-harming tendencies(American Psychiatric Association, 2013; Limandri, 2018). BoPD affects approximately 1–2% of the general population, and it is estimated that approximately 10% of psychiatric outpatients and 20% of psychiatric inpatients are affected by the disorder (Ellison, Rosenstein, Morgan, & Zimmerman, 2018; Lieb, Zanarini, Schmahl, Linehan, & Bohus, 2004).

BoPD is a complex disorder which has been associated with many comorbidities such as depression, bipolar disorder and anxiety disorders (American Psychiatric Association, 2013; Zanarini et al., 1998a, 1998b). Due to this, patients with BoPD are often misdiagnosed and there is evidence that approximately 40% of patients have received a previous misdiagnosis of bipolar disorder compared with only 10% of patients without BoPD (Ruggero, Zimmerman, Chelminski, & Young, 2010). This highlights an unmet need for effective BoPD screening methods that enable an accurate and timely diagnosis.

## BoPD Automatic Screening Initiative

To address the unmet need for effective BoPD screening methods, an initiative has been formed to develop a machine learning algorithm that utilizes electronic health record (EHR) databases to automatically identify patients likely to have BoPD, but without formal diagnosis. Such an algorithm will facilitate efficient real-world clinical decision making for patients with BoPD.

The objective of the borderline personality disorder (BoPD) Automatic Screening Initiative is to advance timely identification of potential BoPD patients. This was made possible by the development of a machine learning algorithm. We built a knowledge-enriched semi-supervised learning framework to learn a classification model.  The algorithm screens electronic health record (EHR) data and automatically identifies patients with highly likely BoPD diagnosis for confirmation of clinical diagnosis.

In the pilot phase, the algorithm has been developed with two methods of implementation.  The first one is implementation through executable python source code.  The second one is implementation through WinPython, a portable distribution of Python programming language for Windows.

> **How much details should we talk about our two stage model? or just a high level overview?**
> 1. We proposed a knowledge-enriched semi-supervised learning framework to learn a classification model


## Develop environment
The portable WinPython can be implemented under windows operation system (Microsoft Windows 10 Enterprise).

## Data used to develop the algorithm
We use de-identified Electronic Health Record (EHR) from Cerner Health Fact data. Data in Health Facts is directly extracted from Electronic Medical Records (EMR) from hospitals who has data use agreement with Cerner. Encounters may include pharmacy, clinical and microbiology laboratory, admission, and billing information from affiliated patient care locations.  Date and time stamps are included for all admissions, medication orders and dispensing, laboratory orders and specimens, providing a temporal relationship between treatment patterns and clinical information. Cerner Corporation has established operating policies ensure that all data in the Health Facts database are fully de-identified in compliance with the Health Insurance Portability and Accountability Act. Currently Health Facts database contains information from approximately 69 million US patients, with data collected from 2000 to 2018 (most between 2009 and 2018).

> Same questions with algorithm, how much details should we give.
> 1. should we talk about 456 golden data, then silver label data, then model generation?

## Input data format (Lulu)

## Execution of BoPDscreen (Ziwei)
### 1. Portable version

The portable screening tool is based on WinPython, a portable distribution of Python programming language for Windows. The zip file attached contains full WinPython application and all document needed. Please see the tool maunal [tool_instruction.md](https://github.com/elyiorgos/sleeppy/blob/master/toolinstruction.md) for detailed execution instructions
$ Note: The downloading can take approximate 1 hour to 3 hours depending on the internet speed; to unzip the file, overnight unzipping is recommended. $ 

### 2. Headless version

After preparing the input dataset as instructed, set the location of input dataset as default directory path, download attached CCSR coding document (v2020-2) to the same location and run the code file for screening result. Here is a built-in check for input data standards. If data quality check does not pass, here will be a warning message indicating the questionable part and the program will be terminated. Please revisit, fix it and rerun. 

Main parts of the package: 

I.	Load in Dataset

II.	 Data quality check

III.	Data Preprocessing

IV.	Apply pre-defined inclusion/ exclusion criteria.

V.	Feature Engineering

VI.	Screening Model Implementation


## License (Ziwei)

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/BoPDdiseasescreening/Borderline-Personality-Disorder-BoPD-automatic-disease-screening-tool/blob/main/LICENSE.md) file for details.

## References

American Psychiatric Association. (2013). *Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition*. Arlington VA: American Psychiatric Association.

Ellison, W. D., Rosenstein, L. K., Morgan, T. A., & Zimmerman, M. (2018). Community and Clinical Epidemiology of Borderline Personality Disorder. *Psychiatr Clin North Am, 41*(4), 561-573. doi:10.1016/j.psc.2018.07.008

Limandri, B. J. (2018). Psychopharmacology for Borderline Personality Disorder. *J Psychosoc Nurs Ment Health Serv, 56*(4), 8-11. doi:10.3928/02793695-20180319-01

Lieb, K., Zanarini, M. C., Schmahl, C., Linehan, M. M., & Bohus, M. (2004). Borderline personality disorder. *Lancet, 364*(9432), 453-461. doi:10.1016/s0140-6736(04)16770-6

Ruggero, C. J., Zimmerman, M., Chelminski, I., & Young, D. (2010). Borderline personality disorder and the misdiagnosis of bipolar disorder. *J Psychiatr Res, 44*(6), 405-408. doi:10.1016/j.jpsychires.2009.09.011

Zanarini, M. C., Frankenburg, F. R., Dubo, E. D., Sickel, A. E., Trikha, A., Levin, A., & Reynolds, V. (1998a). Axis I comorbidity of borderline personality disorder. *Am J Psychiatry, 155*(12), 1733-1739. doi:10.1176/ajp.155.12.1733

Zanarini, M. C., Frankenburg, F. R., Dubo, E. D., Sickel, A. E., Trikha, A., Levin, A., & Reynolds, V. (1998b). Axis II comorbidity of borderline personality disorder. *Compr Psychiatry, 39*(5), 296-302. doi:10.1016/s0010-440x(98)90038-4

## Publication

Characterization of borderline personality disorder using a large electronic health record database: potential for the development of a disease screening algorithm. (In preparation)

Identifying potentially undiagnosed borderline personality disorder from a large electronic health record database to aid the development of an automatic screening initiative. (In preparation)

Screening Borderline Personality Disorder Patients from Real-World Data under Limited Supervision. (In preparation)









