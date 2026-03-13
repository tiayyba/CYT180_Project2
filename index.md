# CYT180 — Project 2: Network Traffic Classification Using Machine Learning Algorithms (15%)
In this project, you will build a machine learning pipeline to detect malicious network traffic using data from a real cybersecurity dataset. The objective is to apply machine learning classification techniques to distinguish between benign network traffic and Distributed Denial of Service (DDoS) attacks.

You will work with a network flow dataset derived from the CICIDS2017 intrusion detection dataset. The dataset contains statistical features extracted from network traffic flows, such as packet counts, flow duration, and inter-arrival times. These features are commonly used in cybersecurity analytics to detect abnormal or malicious behavior.

Because the dataset contains real-world network traffic statistics, it includes data quality issues such as missing values and inconsistent attribute types. You will first need to clean and preprocess the data before applying machine learning models.

After preparing the dataset, you will train Logistic Regression and one additional machine learning model, then evaluate and compare their performance to determine which model better detects malicious traffic.

----

## Learning Objectives
This project helps you build practical skills for working with real cybersecurity data. Firewall logs are often messy, inconsistent, and difficult to analyze until they are cleaned. By completing this project, you practice the core steps analysts take before any threat detection or monitoring work can begin.
How This Project Supports CYT180 Learning Objectives: 
- Load and preprocess real-world cybersecurity datasets
- Perform data preprocessing and cleaning for machine learning
- Handle issues such as invalid values, missing values, and incorrect attribute types
- Build a machine learning pipeline in Altair AI Studio
- Train and evaluate multiple classification algorithms
- Compare model performance using accuracy, precision, recall, and F1-score
- Interpret results in the context of cybersecurity threat detection

## Dataset Description
The dataset used in this project is taken from the **CICIDS2017: https://www.unb.ca/cic/datasets/ids-2017.html** intrusion detection dataset, which is widely used in cybersecurity research for developing and evaluating intrusion detection systems.

The CICIDS2017 dataset contains realistic network traffic captured in a controlled environment that includes both normal user activity and multiple types of cyber attacks. The machine learning component of this dataset consists of multiple CSV files, each representing traffic collected during different days and attack scenarios.

For this project, you will work with one specific file from the dataset:

**Friday-WorkingHours-Afternoon-DDos.pcap_ISCX**

This file contains network flow records captured during a DDoS attack scenario. Each row represents a network flow and includes dozens of statistical attributes describing the behavior of that flow. You are provided with this file. If you like you can check the dataset using the link given above as well.

The dataset includes a Label column indicating whether the traffic is:

- **BENIGN** – normal network activity
- **DDoS** – malicious traffic associated with a Distributed Denial of Service attack

----

## Project Tasks

### Task 1 – Load the Dataset

Load the provided CSV dataset into Altair AI Studio.
Inspect the attribute types in the dataset.
Two attributes may appear as polynomial type attributes:

- `Flow_Bytes_s`
- `Flow_Packets_s`

If these attributes appear as polynomial, convert them to numeric attributes before continuing.
Failure to do this may cause Altair AI Studio to crash during model training.


### Task 2 – Sample the Dataset

Because the dataset is very large, if Altair AI is not able to handle all of the data due to your system's RAM capabilities, you should randomly sample 30-75% of the data to reduce processing time while building your machine learning pipeline. This step is completely optional. I was able to handle 100% of data on my machine with 32GB of RAM. 


### Task 3 – Data Cleaning

Perform the following preprocessing steps:

- Replace the values NaN and Infinity with missing values.
- Remove rows containing missing values.
- Ensure all feature attributes are numeric.

### Task 4 – Set the Label Attribute

Assign the **Label** column as the machine learning label (target variable).
This attribute indicates whether the network traffic is BENIGN or DDoS.


### Task 5 – Train/Test Split

Split the dataset into:

- 80% training data
- 20% testing data

### Task 6 – Train Machine Learning Models

You must train two models:

- Logistic Regression
- One additional model chosen from the following:
  - Decision Tree
  - Random Forest
  - Naïve Bayes

Train both models using:
- All feature columns as input attributes
- The Label column as the classification label


### Task 7 – Model Evaluation

Evaluate both models using the Performance (Classification) operator.
Report the following metrics for each model:

- Accuracy
- Precision
- Recall
- F1 Score

Also include screenshots of the confusion matrix for both models.

### Task 8 – Model Comparison

Compare the performance of the two models and briefly answer the following questions in your report:

- Which model performed better overall?
- Which model had the higher F1 score?
- Why might one model perform better than the other for this dataset?

----



## SUBMISSION DETAILS

Submit one report (PDF) through the course LMS.

Your report should clearly demonstrate your understanding of the machine learning workflow by including screenshots of each step and explanations of the parameter settings used in the operators.

1. **Dataset Overview**

   Include:
   - A brief description of the dataset
   - A screenshot showing the dataset loaded in Altair AI Studio
   - A short explanation of what the dataset represents 
3. **Data Preprocessing**

   Include screenshots and explanations showing:

   - Attribute type inspection
   - Conversion of `Flow_Bytes_s` and `Flow_Packets_s` to numeric
   - Handling of `NaN` and `Infinity` values
   - Setting the Label attribute
   
   For each step, include:
   - Screenshot of the operator
   - Screenshot of the operator parameter settings
   - A short explanation of why the step is necessary
5. **Machine Learning Pipeline**

   Include:
   - A screenshot of the complete Altair AI Studio workflow
   - A precise explanation of how the pipeline processes the data
7. **Model Training**

    Include screenshots showing:
   - Logistic Regression operator and its parameter settings
   - The second machine learning model and its parameter settings
Provide a brief explanation of how each model works.
9. **Model Performance**

   Include screenshots showing:
   - Performance metrics for both models
   - Confusion matrices
   - Explain what the metrics indicate about model performance.
11. **Model Comparison**

    Write a short comparison discussing:
    - Which model performed better
    - Which model achieved the higher F1 score
    - Possible reasons for the performance difference


----

## Rubric (15% Total)

### **Part A — DataCamp (5%)**
- Screenshot showing completion of **Chapter 1: Common Data Problems** (name + timestamp visible)

### **Part B — Firewall Log Cleaning (10%)**
1. Identifying Data Issues — **2.0 pts**
  - Correctly identifies **at least 8** issues in the dataset  
2. Cleaning & Standardization — **4.0 pts**
  - screenshots included in the report  
    - Parsed timestamps  
    - IP validation  
    - Port correction  
    - Protocol/action normalization  
    - Byte conversions  
    - Country/device cleanup  
    - Duplicate removal  
3. Validation Checks — **1.5 pts**
  - At least **3 meaningful checks**, such as:
    - Valid range for ports  
    - Valid protocol/action categories  
    - No negative or non‑numeric bytes  
    - No future timestamps  
    - Duplicates removed  

4. Visualization — **1.0 pt**
  - One simple, correct plot (e.g., top IPs, allow vs deny, common ports)  
  - Must be generated *after* cleaning  

5. Report Quality & Explanation — **1.5 pts**
  - Clear, concise summary (1–2 pages)  
  - Explains the problems found, cleaning steps, assumptions, and validations  
  - Demonstrates understanding of the cleaning logic  

----


### Academic Integrity
- The work you submit must be your own.
- You may discuss ideas with classmates, but you must write your own code, explanations, and analysis.
- You may use AI‑based tools for support, but:
   - Your submission must not be entirely AI‑generated.
   - You are responsible for understanding, verifying, and being able to explain all work you submit.
- All screenshots must show the current date/time and your unique username.
- Submissions may be checked for similarity, AI‑generated patterns, or unusual code structure.
