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
•	Net promoter score of 80+ among physicians
•	10% of licensed users run predictions weekly
•	90% of users run predictions daily
Agile 
•	Team members have an average velocity of 8 story points per sprint
Quality
The service API should be available at least 99.9% of the time
The service should receive updates at least quarterly to enhance the products performance, incorporate new data
## 4. Requirements & Constraints
Functional Requirement: 
The model must be able to accept fundus eye images in JPG format
The model must be able to accept square images
The model must work well with small to medium sized images sized between 224 x 224 and 800 x 800 pixels
The model must be able to accept full color images 
Images and predictions must be retained for quality control and audit purposes for 10 years
After 10 years images and predictions must be purged
The model can identify the following defects which are associated with Glaucoma. (4)
Neuroretinal rim thinning
![Area between outline of cup and optic disk]( https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.semanticscholar.org%2Fpaper%2FDetection-of-glaucoma-using-Neuroretinal-Rim-Das-Nirmala%2F73a07c0e1691665a5b376475c4a8355eba8f05f8&psig=AOvVaw0KJzMHsxvWwlflazBGgxmH&ust=1690411512607000&source=images&cd=vfe&opi=89978449&ved=0CBAQjRxqFwoTCJiBmeD3qoADFQAAAAAdAAAAABAEhttps://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.semanticscholar.org%2Fpaper%2FDetection-of-glaucoma-using-Neuroretinal-Rim-Das-Nirmala%2F73a07c0e1691665a5b376475c4a8355eba8f05f8&psig=AOvVaw0KJzMHsxvWwlflazBGgxmH&ust=1690411512607000&source=images&cd=vfe&opi=89978449&ved=0CBAQjRxqFwoTCJiBmeD3qoADFQAAAAAdAAAAABAE)
Peripapillary Atrophy
![Rim is pale in normal eyes, rim is darker in glaucomatous eyes]( https://www.google.com/url?sa=i&url=https%3A%2F%2Fpressbooks.pub%2Fcasebasedneuroophthalmology%2Fchapter%2Fnonglaucomatous-cupping%2F&psig=AOvVaw3BbeCVI6hh6c4DTq5zCIMB&ust=1690410878886000&source=images&cd=vfe&opi=89978449&ved=0CBAQjRxqFwoTCOi-_7H1qoADFQAAAAAdAAAAABAt)
High optic cup-to-disc ratio (CDR)
![Cup occupies a large amount of space within the optic disc]( https://glaucoma.org/wp-content/uploads/2021/11/optic-nerve-comparison-grf-1.jpg)
Disc hemorrhage 
![Flame or splinter shaped red area]( https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.sciencedirect.com%2Fscience%2Farticle%2Fpii%2FS0008418216311759&psig=AOvVaw0e811UK7E4_h0PCbNUKUAA&ust=1690411963172000&source=images&cd=vfe&opi=89978449&ved=0CBAQjRxqFwoTCMjuhbf5qoADFQAAAAAdAAAAABAs)
Non-Functional Requirements
A model prediction should be generated in less than 10 seconds
Less than 2 bugs per 1000 lines of code is discovered upon release
### 4.1 Out of Scope:
Normalization of colors, brightness, contrast within images. 
## 5. Methodology
This project will evaluate the accuracy, specificity, and sensitivity of VGG16 convolutional neural network in classifying eyes as “Glaucomatous” and “Non-Glaucomotous”. VGG16 architecture has been chosen for its ability to classify small images and its success in other health diagnostic use cases. 
488 images from 333 healthy eyes and 155 diseased eyes will be resized to 224x224 dimensions and will be input to 16 convolution layers grouped into 5 blocks. 
### 5.1. Problem statement
Glaucoma detection is a supervised classification problem. 
### 5.2. Data
The Papilla dataset this model will use contains a total of 488 fundus images, 333 healthy eyes and 155 eyes with confirmed or suspected glaucoma. The examinations were carried out by the Department of Ophthalmology of the Hospital General Universitario Reina Sofía, HGURS, (Murcia, Spain). (5)
### 5.3. Techniques
Data Preprocessing 
Original image coefficients are rescaled to be between 0 and 1 by multiplying by a factor of 1/255. 
Images are resized to be 224 x 224 pixels. 
Training images will be flipped horizontally and vertically, shifted, and rotated randomly to account for variations within the dataset.
What machine learning techniques will you use? How will you clean and prepare the data (e.g., excluding outliers) and create features?
### 5.4. Experimentation & Validation
Data will be split into training and validation sets using a split ratio of 4:1. 
The accuracy, specificity and sensitivity of the model will be measured and compared to similar models which had an accuracy, specificity and sensitivity of 90%, 88% and 92%.(6) 
Two optimizers will be compared Adam and SGD and the one that performs best on average over 20 tests will be used.
Categorical cross-entropy will be used as the cost function since it can accept the output of the final SoftMax layer.
If you're A/B testing, how will you assign treatment and control (e.g., customer vs. session-based) and what metrics will you measure? What are the success and [guardrail](https://medium.com/airbnb-engineering/designing-experimentation-guardrails-ed6a976ec669) metrics?

### 5.5. Human-in-the-loop
 Predictions with a confidence level below 75% will contain a message which indicates the user should pursue additional testing.  
This model should not be used to assess glaucoma in individuals with diseases that can mimic it including isquemic and compressive optic neuropathies.(7)
## 6. Implementation

### 6.1. High-level design

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2e/Data-flow-diagram-example.svg/1280px-Data-flow-diagram-example.svg.png)

Start by providing a big-picture view. [System-context diagrams](https://en.wikipedia.org/wiki/System_context_diagram) and [data-flow diagrams](https://en.wikipedia.org/wiki/Data-flow_diagram) work well.

### 6.2. Infra
This service will be hosted in an AWS EC2 instance and images will be stored in S3. 

### 6.3. Performance (Throughput, Latency)

How will your system meet the throughput and latency requirements? Will it scale vertically or horizontally?

### 6.4. Security

How will your system/application authenticate users and incoming requests? If it's publicly accessible, will it be behind a firewall?

### 6.5. Data privacy

How will you ensure the privacy of customer data? Will your system be compliant with data retention and deletion policies (e.g., [GDPR](https://gdpr.eu/what-is-gdpr/))?

### 6.6. Monitoring & Alarms

How will you log events in your system? What metrics will you monitor and how? Will you have alarms if a metric breaches a threshold or something else goes wrong?

### 6.7. Cost
How much will it cost to build and operate your system? Share estimated monthly costs (e.g., EC2 instances, Lambda, etc.)

### 6.8. Integration points
Users of the proposed model will query a REST API posting 1 or more fundus images for analysis and receive predictions back. 
### 6.9. Risks & Uncertainties

Risks are the known unknowns; uncertainties are the unknown unknows. What worries you and you would like others to review?

## 7. Appendix

### 7.1. Alternatives

What alternatives did you consider and exclude? List pros and cons of each alternative and the rationale for your decision.

### 7.2. Experiment Results

Share any results of offline experiments that you conducted.

### 7.3. Performance benchmarks

Share any performance benchmarks you ran (e.g., throughput vs. latency vs. instance size/count).

### 7.4. Milestones & Timeline

What are the key milestones for this system and the estimated timeline?

### 7.5. Glossary

Define and link to business or technical terms.

### 7.6. References
(1)	https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7769798/
(2)	https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8582616/
(3)	https://www.nature.com/articles/eye201077
(4)	https://analyticalsciencejournals.onlinelibrary.wiley.com/doi/full/10.1002/jemt.23094#:~:text=For%20measuring%20the%20optic%20nerve,and%20inter%2Deye%20asymmetry%20defect.
(5)	PAPILA: Dataset with fundus images and clinical data of both eyes of the same patient for glaucoma assessment | Scientific Data (nature.com)
(6)	https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6425593/
(7)	https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5223567/#:~:text=In%20eyes%20with%20IOP%20in,appearance%20resembled%20glaucomatous%20optic%20neuropathy.
(8)	
https://www.omic.com/how-to-survive-a-malpractice-suit/#:~:text=Approximately%2054%20percent%20will%20experience,at%20least%20one%20indemnity%20payment.
https://www.aao.org/education/preferred-practice-pattern/primary-open-angle-glaucoma-ppp

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5223567/
https://www.google.com/search?q=breast+cancer+false+positive+rate&rlz=1C1ONGR_enUS1065US1065&oq=breast+cancer+false+positive+rate&gs_lcrp=EgZjaHJvbWUyBggAEEUYOdIBCDgxMDhqMGo5qAIAsAIA&sourceid=chrome&ie=UTF-8
Cost-effectiveness of treating Glaucoma in developed nations - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3740958/

https://github.com/miag-ull/rim-one-dl
https://assets.bmctoday.net/glaucomatoday/pdfs/gt0514_landmark.pdf

---

