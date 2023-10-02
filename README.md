# An ML Project Proposal & Design Doc for Detecting Glaucoma

<img src="https://github.com/ehaynie63/ml-design-docs/blob/main/amanda-dalbjorn-UbJMy92p8wk-unsplash.jpg" object-fit="cover">

## Contents
* [Overview](#1-overview)
* [Motivation](#2-motivation)
* [Success Metrics](#3-success-metrics)
* [Requirements & Constraints](#4-requirements--constraints)
* [Functional Requirements](#functional-requirement)
* [Non-Functional Requirements](#non-functional-requirements)
* [Out of Scope](#41-out-of-scope)
* [Explainability Enhancements for V1.1](#explainability-enhancements-for-v11)
* [Methodology](#5-methodology)
* [Experimentation & Validation](#54-experimentation--validation)
* [Human in the Loop](#55-human-in-the-loop)
* [Implementation](#6-implementation)
* [Appendix](#7-appendix)
* [References](#73-references)

## 1. Overview
Glaucoma is the 2nd leading cause of blindness, affecting 57.5 million people worldwide. Routine eye examinations are an opportunity to identify Glaucoma in its early stages. However, many optometrists lack access to advanced screening technology. One study performed in the UK showed that the mean accuracy of a community optometrist viewing the fundus alone was 62%. Meanwhile, an AI screening tool had an accuracy of 93%. This project aims to provide healthcare application developers in optometry and telemedicine with access to highly accurate and cost-effective glaucoma screening technology. If successful, "Glaucoma Screening Service" will be made publicly available via paid API. (See [References](#73-references) for supporting evidence.)
## 2. Motivation
Hospitals and healthcare providers exploring AI screening models have done so in isolation, developing their own in-house models. Yet other healthcare companies do not have the resources to build their models and thus seek to decrease their time to market by using off-the-shelf models. 
The cost of allowing Glaucoma to go undiagnosed is high. Research shows that vision loss is associated with higher Medicare beneficiary costs for trauma, depression, subacute nursing facilities, and nursing homes. A person in the Medicare population with mild vision loss < 20/40 accrues an average of $5,302 in direct medical costs. In contrast, a person with severe vision loss accrues an average of $9,994 in direct medical costs. Therefore, patients, the government, and insurers are especially interested in technology that improves the accuracy and cost-effectiveness of Glaucoma screening. 
## 3. Success metrics
### Business Metrics
The proposed model will be sold to healthcare application developers in the fields of optometry and telemedicine. Telehealth providers could prove to be optimal adopters since their business models are agile and they are tech savvy. The success metrics below examine what it would look like to pursue US telemedicine companies as customers in Year 1.  
***Year 1 Sales Assumptions***
* There are 1,387 telemedicine providers in the US as of 2023
* 1% market share obtained in Year 1 = 13 customers
* Annual licensing fee of $10,000
* Gross Income = 13 * $10,000 = $130,000


***Year 1 Cost of Development & Maintenance Metrics***
*See [Cost of MVP](#cost-of-mvp) and [Estimated Monthly Cost](#estimated-monthly-costs) for detailed breakdowns.

* MVP cost of $14,617.46 
* Maintenance cost of $10,494 
* Total Year 1 Costs =  $14,617.46 + $10,494 = ~$25,111 

***Year 1 Project ROI***
* ROI = (Net Income – Cost of Development)/Cost of Development
* ROI = (130,000 –25,111)/25,111  = 435%

### Customer Satisfaction & Product Engagement Metrics
* Net promoter score of 85+ among physicians
* 90% of users run predictions daily
Agile 
* Team members have an average velocity of 8 story points per two-week sprint
Quality
The service API must be available at least 99.9% of the time
The service must receive updates at least quarterly to enhance the product performance, incorporate new data
## 4. Requirements & Constraints
### Functional Requirement: 
High-level user stories and acceptance criteria. 
A "user" in the stories below represents an external electronic health record system (EHR) that interfaces with this Rest API to generate glaucoma screening results, which will be reviewed by physicians.
***Table 1***
| # | User Story | Acceptance Criteria | Release| Estimate|
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 1 | A user would like to be able to submit their prediction via Rest API for ease of access| A request to `/glaucomaservice/v1/predict` containing fundus eye image and unique image  identifier receives a JSON response with prediction, status code and datetime stamp| V1| 2
| 2 | A user would like to authenticate via OAuth2 so that data access is secure | A request authenticated via OAuth2 is accepted while an unauthenticated request results in an error | V1 | 2
| 3 | A user would like to submit an eye fundus image and receive a "suspected glaucoma" result when the eye is glaucomatous-looking so that they can confirm or reevaluate their diagnosis | When an image of a known glaucomatous eye is submitted, a "glaucoma suspected" result is returned | V1 | 2
| 4 | A user would like to submit an eye fundus image and receive a "benign" result when no glaucomatous features are detected so that they can confirm or reevaluate their diagnosis | When an image of a known normal eye is submitted, a "benign" result is returned | V1 | 2
| 5 | A user would like to submit an eye fundus image and receive a "further testing required" result when the model confidence is 75% or less so that the user can refer the patient for further testing| When an image of an eye with irregular or alternate pathology is presented, a "further testing required" result is returned | V1 | 2
| 6 | The model must be able to accept fundus eye images in JPEG format | A request including a JPEG image is accepted, while a request with a PNG image results in an error code| V1 | 1
| 7 | The model must be able to handle square images and no other shape | A request including a square image is accepted, and a request with a rectangle image results in an error code| V1 | 1 
| 8 | The model should only be able to use full-color images and not grayscale | A full-color image is accepted by the model, while a grayscale image results in an error response | V1 | 1
| 9 | The model must be able to use images with sizes between 224x224 and 800x800 | A request with an image with dimensions 224x224 is accepted, while an image with dimensions 900x900 results in an error status code| V1 | 2
| 10 | The model must be able to differentiate between normal and glaucomatous eyes with an accuracy of at least 90% | A model that fails to have an accuracy of at least 90% triggers a manual review | V1| 3
| 11 | The model must be able to differentiate between normal and glaucomatous eyes with a specificity of at least 88% | A model that fails to have a specificity of at least 88% triggers a manual review | V1 | 3
| 12 | The model must be able to differentiate between normal and glaucomatous eyes with a sensitivity of at least 92% | A model that fails to have a sensitivity of at least 92% triggers a manual review | V1 | 3
| 13 | A business user would like to be able to request prior predictions so that they can be reviewed ad-hoc to comply with SLAs and audits | A request for predictions made between 11/01/2023 and 12/01/23 by Biosciences LLC is fulfilled and contains the datetime stamp of prediction, result and unique internal identifier maintained by Biosciences LLC | V1 | 3 
| 14 | The site operations team would like to be paged when the service goes down or when over 5% of HTPP responses have a 400 or 500 code within 15 minutes so that the service can be returned to its normal state| When 10% of response codes are 400 or 500, a SOPS engineer is paged | V1 | 1
| | | | ***Total*** | ***32***
### Non-Functional Requirements
The effort for these requirements is included in the estimates above. 
* A model prediction must be generated in less than 10 seconds
* Less than 2 bugs per 1000 lines of code is discovered upon release
### 4.1 Out of Scope:
***Table 2***
### Explainability Enhancements for V1.1
| User Story | Release | Estimate | Image
| ------------- | ------------- |------------- | ------------- |
| The model must be able to identify neuroretinal thinning which is associated with Glaucoma | V1.1 | TBD | <img src= "https://slideplayer.com/slide/13803871/85/images/21/Temporal+thinning+of+the+Neuro-retinal+rim.jpg" width=50% height=50%>
| The model must be able to identify peripapillary atrophy which is associated with Glaucoma | V1.1 | TBD | <img src= "https://entokey.com/wp-content/uploads/2016/10/A318070_1_En_4_Fig3_HTML.jpg" width=50% height=50%>
| The model must be able to identify disc hemorrhage which is associated with Glaucoma | V1.1 | TBD | <img src=https://webeye.ophth.uiowa.edu/dept/COMS/grading/thumbs/33-opticdischmrg-std-1.jpg width=50% height=50%>
| The model must be able to identify high cup-to-disc ratio which is associated with Glaucoma | V1.1 | TBD | <img src="https://glaucoma.org/wp-content/uploads/2021/11/optic-nerve-comparison-grf-1.jpg" width=50% height=50%>

## 5. Methodology
1. Architecture selection - This project will evaluate the accuracy of a VGG16 convolutional neural network in classifying eyes as "Glaucomatous" and "Non-Glaucomotous". VGG16 architecture has been chosen for its ability to classify small images with little data loss. 
2. Data selection – The recently published ACRIMA dataset is selected to for its consistency and quality. The images are clear, centered, and patients were screened by glaucoma experts.
3. Data Preprocessing – Images are resized to improve consistency. The data is augmented to be able to handle flipped, rotated, and off-center images.   
4. Training – Model hyperparameters will be adjusted to find the optimal performance.
5. Evaluation – The model will be evaluated using 10-fold cross-validation and performance metrics calculated.
### 5.1. Problem statement
Glaucoma detection is a supervised classification problem. 
### 5.2. Data
The ACRIMA dataset contains a total of 705 fundus images, 309 normal eyes, and 396 eyes with confirmed or suspected Glaucoma.
### 5.3. Techniques
Data Preprocessing 
* Original image coefficients are rescaled to be between 0 and 1 by multiplying by a factor of 1/255. 
* Images are standardized to be 224 x 224 pixels. 
* Training images will be flipped horizontally and vertically, shifted, and rotated randomly to account for variations within the dataset.
### 5.4. Experimentation & Validation
* Data will be split into training and validation sets using a split ratio of 4:1. 
* The accuracy, specificity, and sensitivity of the model will be measured and compared to similar models which had an accuracy, specificity, and sensitivity of 90%, 88%, and 92%.(6) 
* Two optimizers will be compared, Adam and SGD.
* Categorical cross-entropy will be used as the cost function since it can accept the output of the final SoftMax layer.
### 5.5. Human-in-the-loop
Predictions with a confidence level below 75% will contain a message that indicates the user should undergo additional screening. 
This model should not be used to assess Glaucoma in individuals with diseases that can mimic it, including ischemic and compressive optic neuropathies. (7)
## 6. Implementation

### 6.1. High-Level Design
<img src="https://github.com/ehaynie63/ml-design-docs/blob/main/Glaucoma_Design_DFD.jpg">

### 6.2. Infra Costs

### Estimated Monthly Costs

| Description | Unit | Qty. | Cost per Unit | Subtotal
| ------------- | ------------- |------------- | ------------- | ------------- |
| Engineering Maintenance Cost | 2 | Stories | $456 | $912 |
| Sagemaker Notebook Instance ml.t3.medium | 8 | Hours | $0.0464 | $0.37
| S3 Standard Storage First TB | 1 | Month | $.023 | $.023
| SageMaker Processing ml.t3.large| 4 | Hours | $0.10 | $$0.40 
| SageMaker Training ml.m5.large | 4 | Hours | $0.115 | $0.46
| SageMaker Real Time Inference ml.t2.medium | 730 | Hours | $0.056 | $40.88
| AWS Lambda  Free Tier| 300,000 | GB-seconds | Up to 400K Free | $0.000
| | | | ***Total*** | ***$954.00***
***Table 3***

### 6.3. Performance (Throughput, Latency)
* The glaucoma screening service will scale horizontally to be able to handle a maximum of 1,000 requests per minute. 
* The service must return a response for one prediction in less than 10 seconds. 
### 6.4. Security
Users of the service will authenticate via OAuth 2.0. 
### 6.5. Data Privacy
Patient fundus images will be stored encrypted with the external system's unique identifier. None of the information posted to the API will be shared with 3rd parties. 
### 6.6. Monitoring & Alarms
### Model Performance Monitoring
Each major release triggers a reevaluation of model performance metrics to ensure it performs well. 
Monitoring for Service API
HTTP responses with 400 and 500 codes will be logged in a Splunk dashboard. Log messages must not contain any patient data.
If over 5% of HTPP responses have a 400 or 500 code within 15 minutes, Splunk will trigger an alert to the site operations team. 
If the service goes down, an alert will be triggered. 
### 6.7. Cost 

These are the initial costs to design, train and launch a model. AWS services will be turned off when not used. 

### Cost of MVP

| Description | Unit | Qty. | Cost per Unit | Subtotal
| ------------- | ------------- | ------------- | ------------- | ------------- |
| Engineering Cost *See Table 1| 32 | Stories | $456 | $14,592 |
| Sagemaker Notebook Instance ml.t3.medium | 256 | Hours | $0.0464 | $11.88 
| S3 Standard Storage | 2 | Months | $.023 | $0.046
| SageMaker Processing ml.t3.large| 72 | Hours | $.10 | $7.20 
| SageMaker Training ml.m5.large | 18 | Hours | $0.115 | $2.07
| SageMaker Real Time Inference ml.t2.medium | 72 | Hours | $0.056 |$4.03
| AWS Lambda  Free Tier| 300,000 | GB-seconds | Includes up to 400K | $0.00
| | | | ***Total*** | ***$14,617.46***|
***Table 4***

## 6.8. Integration points
### Generate Prediction Request

 
`POST https://www.GlaucomaScreening.com/glaucomascreening/v1/predict`
| Request Field | Field Type | Field Description
| ------------- | ------------- | ------------- |
| unique_id | String | Unique identifier of this fundus eye image from the user’s system 
| image | String | Base64 encoded image 

***Response***

| Response Field | Field Type | Field Description
| ------------- | ------------- | ------------- |
| unique_id | String | Unique identifier of this fundus eye image from the user’s system
| status | String | Response status code 
| prediction | String | The result of the prediction can be “Suspected glaucoma”, “Benign” or “Further testing required” 

*Oauth2 details omitted 

### 6.9. Risks & Uncertainties
According to research, machine learning models developed to detect Glaucoma can perform far differently in real-world applications. This is mainly due to the use of differing fundus photography equipment. Improvement to the image preprocessing strategy may be necessary. 

## 7. Appendix

### Tables
* Table 1 – Functional Requirements
* Table 2 – Out Scope
* Table 3 – Estimated Monthly Costs
* Table 4 – Cost of MVP

### 7.1. Milestones & Timeline
	
* Jul 26 - Aug 2 – Data wrangling
* Aug 3 – Aug 9 – Model training and validation
* Aug 10 – Aug 16: REST API development 
* Aug 17 – Aug 23: Audit database built and alerts/monitoring set up
* Aug 24 – Aug 31 – Testing
* Sept 1: API Release

### 7.2. Glossary

Glaucoma - Glaucoma is a group of eye diseases that lead to damage of the optic nerve, which is important for transmitting visual information from the eye to the brain. This damage is often caused by increased pressure within the eye, known as intraocular pressure (IOP) and may cause vision loss if left untreated. The word glaucoma originated from the Greek word ΓλαύV̇ξ (glaukos), which means "to glow". Glaucoma has been called the "silent thief of sight" because the loss of vision usually occurs slowly over a long period of time. It is associated with old age, a family history of glaucoma, and certain medical conditions or medications.

EHR - An electronic health record (EHR) is the systematized collection of patient and population electronically-stored health information in a digital format. These records can be shared across different healthcare settings. Records are shared through network-connected, enterprise-wide information systems or other information networks and exchanges. EHRs may include a range of data, including demographics, medical history, medication and allergies, immunization status, laboratory test results, radiology images, vital signs, personal statistics like age and weight, and billing information. 

### 7.3. References
1. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7769798/
2. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8582616/
3. https://www.nature.com/articles/eye201077
4. https://analyticalsciencejournals.onlinelibrary.wiley.com/doi/full/10.1002/jemt.23094#:~:text=For%20measuring%20the%20optic%20nerve,and%20inter%2Deye%20asymmetry%20defect.
5. https://www.nature.com/articles/s41597-022-01388-1
6. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6425593/
7. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5223567/#:~:text=In%20eyes%20with%20IOP%20in,appearance%20resembled%20glaucomatous%20optic%20neuropathy.
8. https://www.nature.com/articles/s41746-023-00857-0
9. https://www.omic.com/how-to-survive-a-malpractice-suit/#:~:text=Approximately%2054%20percent%20will%20experience,at%20least%20one%20indemnity%20payment.
10. https://www.aao.org/education/preferred-practice-pattern/primary-open-angle-glaucoma-ppp
11. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5223567/
https://www.google.com/search?q=breast+cancer+false+positive+rate&rlz=1C1ONGR_enUS1065US1065&oq=breast+cancer+false+positive+rate&gs_lcrp=EgZjaHJvbWUyBggAEEUYOdIBCDgxMDhqMGo5qAIAsAIA&sourceid=chrome&ie=UTF-8
12. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3740958/
13. https://github.com/miag-ull/rim-one-dl
14. https://assets.bmctoday.net/glaucomatoday/pdfs/gt0514_landmark.pdf

