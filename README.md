# An ML Project Proposal & Design Doc for Detecting Glaucoma

---
## 1. Overview
A summary of the doc's purpose, problem, solution, and desired outcome, usually in 3-5 sentences.
Glaucoma is the 2nd leading cause of blindness affecting 57.5 million people worldwide. Routine eye examinations are an opportunity to identify Glaucoma in its early stages however many optometrists lack access to advanced screening technology. (3) One study performed in the UK showed that mean accuracy of a community optometrist viewing the fundus alone was 62%. Meanwhile, an AI screening tool had an accuracy of 93%. (3) The purpose of this project is to provide healthcare application developers in the field of optometry and telemedicine with access to highly accurate and cost-effective glaucoma screening technology. If successful, this model will be made publicly available via paid API. 
## 2. Motivation
Hospitals and healthcare providers exploring the use of AI screening models have done so in isolation, developing their own in-house models. There are yet other healthcare companies who do not have the resources to develop their own models and thus seek to decrease their time to market by using off-the-shelf models. 
The cost of allowing Glaucoma to go undiagnosed is high. Research shows that vision loss is associated with higher Medicare beneficiary costs for trauma, depression, subacute nursing facilities and nursing homes.  A person in the Medicare population with mild vision loss < 20/40 accrues an average of $5,302 in direct medical costs while a person with severe vision loss accrues an average of $9,994 in direct medical costs. Therefore, patients, government and insurers are especially interested in technology which improves the accuracy and cost-effectiveness of Glaucoma screening. 
## 3. Success metrics
Business Metrics
The proposed model will be sold to healthcare application developers in the field of optometry and telemedicine. Telehealth providers could prove to be optimal adopters since their business models are agile, and they are tech savvy. The success metrics below examine what it would look like to pursue US telemedicine companies as customers in Year 1.  
Business Performance Year 1
Assuming a goal of 1% market share in year 1, annual licensing fee of $10,000 and cost of development of $100,000, here are the target business performance metrics. There are 1,387 telemedicine providers in the US as of 2023. 
ROI = (Net Income – Cost of Development)/Cost of Development
ROI = (130,000 – 100,000)/ 100,000 = 30%
Customer Satisfaction & Product Engagement
* Net promoter score of 80+ among physicians
* 10% of licensed users run predictions weekly
* 90% of users run predictions daily
Agile 
* Team members have an average velocity of 8 story points per two-week sprint
Quality
The service API should be available at least 99.9% of the time
The service should receive updates at least quarterly to enhance the products performance, incorporate new data
## 4. Requirements & Constraints
### Functional Requirement: 
| User Story | Release| Estimate|
| ------------- | ------------- | ------------- |
| A user would like to be able to submit their prediction request to the endpoint GET /glaucomaservice/v1/predict | V1| 2
| The model must be able to accept fundus eye images in JPG format | V1 | 2
| The model must be able to handle square images and no other shape  | V1 | 1 
| The model must be able to use images with sizes between 224x224 and 800x800  | V1 | 2
| The model must be able to differentiate between normal and glacomotous eyes with an accuracy, specificity and sensitivity of at least 90%, 88% and 92% respectively| V1.1 | 3
| The model must be able to identify  high optic cup-to-disc ratio which is associated with Glaucoma | V1.1| 3
| The model should only be able to use full-color images and not grayscale | V1 | 3
| | ***Total*** | ***16***
### Non-Functional Requirements
The effort for these requirements is included in the estimates above. 
* A model prediction should be generated in less than 10 seconds
* Less than 2 bugs per 1000 lines of code is discovered upon release
### 4.1 Out of Scope:
### Explainability Enhancements
| User Story | Release | Estimate | Image
| ------------- | ------------- |------------- | ------------- |
| A physician wants to be able to see an annotation of glaucomatous features| V1.1 | TBD
| The model must be able to identify neuroretinal thinning which is associated with Glaucoma | V1.1 | TBD | <img src="https://slideplayer.com/slide/13803871/85/images/21/Temporal+thinning+of+the+Neuro-retinal+rim.jpg" width=50% height=50%>
| The model must be able to identify peripapillary atrophy which is associated with Glaucoma | V1.1 | TBD | <img src="https://entokey.com/wp-content/uploads/2016/10/A318070_1_En_4_Fig3_HTML.jpg" width=50% height=50%>
| The model must be able to identify disc hemorrhage which is associated with Glaucoma | V1.1 | TBD | <img src=https://webeye.ophth.uiowa.edu/dept/COMS/grading/thumbs/33-opticdischmrg-std-1.jpg width=50% height=50%>
| The model must be able to identify high cup-to-disc ratio which is associated with Glaucoma | V1.1 | TBD | <img src="https://glaucoma.org/wp-content/uploads/2021/11/optic-nerve-comparison-grf-1.jpg" width=50% height=50%>

## 5. Methodology
1. Architecture selection - This project will evaluate the accuracy of a VGG16 convolutional neural network in classifying eyes as “Glaucomatous” and “Non-Glaucomotous”. VGG16 architecture has been chosen for its ability to classify small images with little data loss. 
2. Data selection – The recently published ACRIMA dataset is selected to for its consistency and quality. The images are clear, centered, and patients were screened by glaucoma experts.
3. Data Pre-Processing – Images are resized to improve consistency. The data is augmented to be able to handle flipped, rotated, and off-center images.   
4. Training – Model hyper parameters will be adjusted to find the optimal performance.
5. Evaluation – The model will be evaluated using 10-fold cross validation and performance metrics calculated.
### 5.1. Problem statement
Glaucoma detection is a supervised classification problem. 
### 5.2. Data
The ACRIMA dataset this model contains a total of 705 fundus images, 309 normal eyes and 396 eyes with confirmed or suspected glaucoma.
### 5.3. Techniques
Data Preprocessing 
* Original image coefficients are rescaled to be between 0 and 1 by multiplying by a factor of 1/255. 
* Images are standardized to be 224 x 224 pixels. 
* Training images will be flipped horizontally and vertically, shifted, and rotated randomly to account for variations within the dataset.
What machine learning techniques will you use? How will you clean and prepare the data (e.g., excluding outliers) and create features?
### 5.4. Experimentation & Validation
* Data will be split into training and validation sets using a split ratio of 4:1. 
* The accuracy, specificity and sensitivity of the model will be measured and compared to similar models which had an accuracy, specificity and sensitivity of 90%, 88% and 92%.(6) 
* Two optimizers will be compared Adam and SGD and the one that performs best on average over 20 tests will be used.
* Categorical cross-entropy will be used as the cost function since it can accept the output of the final SoftMax layer.
### 5.5. Human-in-the-loop
 Predictions with a confidence level below 75% will contain a message which indicates the user should undergo additional screening. 
This model should not be used to assess glaucoma in individuals with diseases that can mimic it including isquemic and compressive optic neuropathies. (7)
## 6. Implementation

### 6.1. High-Level Design
<img src="https://github.com/ehaynie63/ml-design-docs/blob/main/Glaucoma_Design_DFD.jpg">
### 6.2. Infra
This service will be hosted in an AWS EC2 instance and images will be stored in S3. 

### 6.3. Performance (Throughput, Latency)
* The glaucoma screening service will scale horizontally to be able to handle a maximum of 1000 request per minute. 
* A response for 1 prediction should be returned by the service in less than 10 seconds. 
### 6.4. Security
Users of the service will authenticate via OAuth 2.0. 
### 6.5. Data privacy
PII will not be received at any time but patient fundus images will be stored encrypted. None of the information posted to the API will be shared with 3rd parties. 
### 6.6. Monitoring & Alarms
Model Performance Monitoring
Each major release triggers a reevaluation of model performance metrics to ensure it is still performing well. 
Monitoring for Service API
HTTP responses with 400 and 500 codes should be logged in a Splunk dashboard. Logs messages should not contain any patient data.
If greater than 5% of HTPP responses have a 400 or 500 code within 15 minutes an alert should be triggered. 
If the service goes down, an alert will be triggered. 
### 6.7. Cost
ML Notebook Instance – Price per hour for ml.t2.medium is $0.0464
•	Estimated Cost for MVP: $3.71
•	ML Storage $0.14 per GB-month
•	Estimated cost: $0.56 per month
•	Data Processing Cost - $0.16 per GB
•	Est MVP Cost: $6.40
•	Training Instance Hourly Cost (Charged for duration of training job) – 
•	Storage - $0.14 per month per GB
•	Hosting Real-Time Instance: Instance Cost TBD
•	Data In/Out Cost - $0.016 per GB
•	Image Audit Storage: TBD
•	Endpoint Charge is hourly

| No. of Units | Cost per Unit | Subtotal
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
How much will it cost to build and operate your system? Share estimated monthly costs (e.g., EC2 instances, Lambda, etc.)
### 6.8. Integration points
Users of the proposed model will query a REST API posting 1 or more fundus images for analysis and receive predictions back. 
### 6.9. Risks & Uncertainties
According to research, machine learning models developed to detect Glaucoma can perform far differently in real-world applications. This mainly due to the use of differing fundus photography equipment. Improvement to the image pre-processing strategy may be necessary. 
## 7. Appendix

### 7.1. Milestones & Timeline

What are the key milestones for this system and the estimated timeline?
* July 26 – Finalize product requirements document
* July 31 – Data pre-processing complete
* Aug 2 – Model Training & Performance Evaluation Complete
* Aug 16 – Model deployed to an EC2 instance 
* Aug 23 – API released to the public

### 7.2. Glossary

Define and link to business or technical terms.

### 7.3. References
(1)	https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7769798/
(2)	https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8582616/
(3)	https://www.nature.com/articles/eye201077
(4)	https://analyticalsciencejournals.onlinelibrary.wiley.com/doi/full/10.1002/jemt.23094#:~:text=For%20measuring%20the%20optic%20nerve,and%20inter%2Deye%20asymmetry%20defect.
(5)	PAPILA: Dataset with fundus images and clinical data of both eyes of the same patient for glaucoma assessment | Scientific Data (nature.com)
(6)	https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6425593/
(7)	https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5223567/#:~:text=In%20eyes%20with%20IOP%20in,appearance%20resembled%20glaucomatous%20optic%20neuropathy.
(8)	https://www.nature.com/articles/s41746-023-00857-0
(9)	
https://www.omic.com/how-to-survive-a-malpractice-suit/#:~:text=Approximately%2054%20percent%20will%20experience,at%20least%20one%20indemnity%20payment.
https://www.aao.org/education/preferred-practice-pattern/primary-open-angle-glaucoma-ppp

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5223567/
https://www.google.com/search?q=breast+cancer+false+positive+rate&rlz=1C1ONGR_enUS1065US1065&oq=breast+cancer+false+positive+rate&gs_lcrp=EgZjaHJvbWUyBggAEEUYOdIBCDgxMDhqMGo5qAIAsAIA&sourceid=chrome&ie=UTF-8
Cost-effectiveness of treating Glaucoma in developed nations - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3740958/

https://github.com/miag-ull/rim-one-dl
https://assets.bmctoday.net/glaucomatoday/pdfs/gt0514_landmark.pdf

---

