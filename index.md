# CYT180 — Project 2: Network Traffic Classification Using Machine Learning Algorithms (15%)

**Work type:** Group project (2 students, or solo)
**Group registration:** Blackboard discussion post, before the end of Week 11
**Report due:** ____________________ (11:59 PM, on Blackboard)
**Presentations:** Week 12, in person, to the instructor (Week 14 is a backup week only)

In this project, you will build a machine learning pipeline to detect malicious network traffic using data from a real cybersecurity dataset. The objective is to apply machine learning classification techniques to tell the difference between benign (normal) network traffic and Distributed Denial of Service (DDoS) attacks.

You will work with a network flow dataset taken from the CICIDS2017 intrusion detection dataset. The dataset contains statistical features extracted from network traffic flows, such as packet counts, flow duration, and inter-arrival times. These features are commonly used in cybersecurity analytics to detect abnormal or malicious behavior.

Because the dataset contains real-world network traffic statistics, it has data quality problems such as missing values, invalid values, and attributes with the wrong type. You will first clean and prepare the data before applying machine learning models.

After preparing the dataset, you will train Logistic Regression and one additional machine learning model, then evaluate and compare their performance to decide which model detects malicious traffic better.

----

## Learning Objectives

This project helps you build practical skills for working with real cybersecurity data. Network traffic data is often messy, inconsistent, and difficult to analyze until it is cleaned. By completing this project, you practice the core steps analysts take before any threat detection or monitoring work can begin.

In this project you will learn to:

- Load and preprocess a real-world cybersecurity dataset
- Handle problems such as invalid values, missing values, and incorrect attribute types
- Build a machine learning pipeline in Altair AI Studio
- Train and evaluate two classification algorithms
- Compare model performance using accuracy, precision, recall, and F1 score
- Interpret results in the context of a Security Operations Centre (SOC)

----

## Tool: Altair AI Studio

This project is completed in **Altair AI Studio**, a visual data analytics and machine learning platform. Instead of writing code, you build a machine learning workflow by connecting operators in a graphical pipeline. The platform supports the common tasks of the machine learning lifecycle: loading data, cleaning data, preparing features, training models, and evaluating performance.

Altair AI Studio is a good fit for this project because it lets you focus on understanding the machine learning workflow instead of spending time writing code. By connecting operators in a pipeline, you can clearly see how data moves through each stage of the process, from preprocessing to model training and evaluation.

Using a visual workflow also makes it easier to:

* Understand the order of the steps in a machine learning pipeline
* Inspect intermediate results at each stage of the workflow
* Experiment with different models and parameter settings
* Compare two models within the same pipeline

In many production environments, machine learning models are built with code, using tools such as **scikit-learn** or **Spark MLlib**. However, visual tools like Altair AI Studio are widely used in industry for rapid prototyping, exploratory data analysis, and teaching machine learning concepts.

You can access **Altair AI Studio through Seneca MyApps**. Log in to your Seneca account and launch the application from the **MyApps portal**.

----

## Project Type

This is a **group project**.

- Each group should have 2 students. Solo submissions are allowed, but groups may not have more than 2 students.
- Both students must take part in building the workflow, writing the report, and presenting the project.
- During the presentation, both group members must be able to explain the project and answer questions.
- The presentation is **not a formal class presentation**. Each group demonstrates and explains their project directly to the instructor.

----

## Blackboard Discussion Post (Project 2 Group Registration)

Each group must reply to the Project 2 Blackboard discussion thread with:

- The full names and student IDs of both group members
- Which member is **Member 1**. Member 1's student ID decides your random seed (see below).

After registration, the instructor assigns each group a **group number** (01, 02, ...) and posts it on Blackboard. Your group number decides your dataset file.

The post must be submitted **before the end of Week 11**. All presentations take place in **Week 12**. Week 14 is a backup week, used only if a group cannot present in Week 12 for an approved reason. The instructor will post the presentation schedule after registration closes.

----

## Dataset Description

The dataset used in this project comes from the **CICIDS2017** intrusion detection dataset (https://www.unb.ca/cic/datasets/ids-2017.html), which is widely used in cybersecurity research for building and testing intrusion detection systems.

CICIDS2017 contains realistic network traffic captured in a controlled environment. It includes normal user activity and several types of cyber attacks. The machine learning part of the dataset is a set of CSV files, one for each day and attack scenario. This project uses the file from Friday afternoon, when a DDoS attack was run:

**Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv**

Each row is one **network flow** (one conversation between two computers) and has about 78 statistical attributes that describe the behavior of that flow.

### Your group's dataset file

You do **not** work with the full file. Each group gets a random sample of it (about 79,000 of the 225,745 rows, roughly 35%), so the file is small enough for Altair AI Studio on any computer.

- Download the file for **your group number** from Blackboard. For example, group 07 uses `CYT180_P2_group07.zip`, which contains `CYT180_P2_group07.csv`. Every group has a different file.
- The sample still has all the problems of the original file (text values such as `NaN` and `Infinity`, zero and negative values), so all the cleaning steps below still apply.
- Groups that used a different file than their own group's file, or whose numbers match another group's numbers, receive 0 for the project.

### Column names

In the original CICIDS2017 file, most column names start with a space (for example `" Label"` and `" Flow Duration"`) and some contain a `/` (for example `Flow Bytes/s`). In your sample file the names have been cleaned: the leading spaces are removed, spaces are replaced with `_`, and `/s` is written as `_s`. This handout uses the names exactly as they appear in your file:

| Original name | Name in your file |
|---|---|
| `" Flow Duration"` | `Flow_Duration` |
| `"Flow Bytes/s"` | `Flow_Bytes_s` |
| `" Flow Packets/s"` | `Flow_Packets_s` |
| `"Init_Win_bytes_forward"` | `Init_Win_bytes_forward` |
| `" Init_Win_bytes_backward"` | `Init_Win_bytes_backward` |
| `" Flow IAT Min"` | `Flow_IAT_Min` |
| `" Fwd IAT Min"` | `Fwd_IAT_Min` |
| `" Fwd IAT Max"` | `Fwd_IAT_Max` |
| `" Destination Port"` | `Destination_Port` |
| `" Label"` | `Label` |

### The Label column

The `Label` column says whether each flow is:

- **BENIGN**: normal network activity
- **DDoS**: malicious traffic from a Distributed Denial of Service attack

In this project **DDoS is the positive class**. This means that precision, recall, and F1 score are always calculated for the DDoS class: recall is "what share of the real attacks did the model catch", and precision is "when the model raised a DDoS alert, how often was it right". Task 4 shows how to set this in Altair AI Studio.

----

## Your Random Seed

Several operators in this project use random numbers (the train/test split). To make your results repeatable, and different from other groups, you must use your own seed:

**Your seed = the last 4 digits of Member 1's student ID.** For example, if Member 1's ID is 123456789, your seed is 6789. If the last 4 digits start with 0 (for example 0412), use 412.

Wherever an operator has the option **use local random seed**, tick it and type your seed. Your screenshots must show this setting.

----

## Project Tasks

Keep a record of the number of rows (examples) after each step. Altair AI Studio shows this at the top of the results view, for example "ExampleSet (79,012 examples, 1 special attribute, 77 regular attributes)". You will need these numbers for the table in Task 3.

### Task 1 – Load the Dataset

1. Use **Import Data** (or the **Read CSV** operator) to load your group's sample file.
2. In the last step of the import wizard, look at the type of each attribute. Two attributes appear as **polynominal** (text) instead of numeric:
   - `Flow_Bytes_s`
   - `Flow_Packets_s`

   This happens because some of their values are the text `NaN` or `Infinity` instead of numbers. Leave them as they are for now; you fix them in Task 2.
3. Change the type of `Label` from **polynominal** to **binominal**. Binominal means "exactly two values", which is what a yes/no classifier needs. If you missed this in the wizard, add a **Nominal to Binominal** operator for the `Label` attribute right after loading.
4. Write down the number of rows loaded, and the number of BENIGN and DDoS rows (use the **Statistics** tab of the results view).

### Task 2 – Data Cleaning

Perform the following steps, in this order:

1. **Remove `Destination_Port`.** Use **Select Attributes** with *invert selection* ticked to remove it. In this dataset almost all DDoS flows go to port 80, so a model could "cheat" by learning the port number instead of the traffic behavior. A real attacker can use any port, so a model like that would not work in practice.
2. **Convert `Flow_Bytes_s` and `Flow_Packets_s` to numeric.** Use **Parse Numbers** on these two attributes, with *unparsable value handling* set to **replace with missing**. Any value that cannot be read as a number (such as `NaN`) becomes a missing value.
3. **Replace infinite values.** Use **Replace Infinite Values** and set them to **missing**. Infinite values break the calculations inside the models.
4. **Remove rows with missing values.** Use **Filter Examples** with the condition **no_missing_attributes**.
5. **Remove rows with zero or invalid values.** Use **Filter Examples** with *custom filters*, set to match **all** conditions, and keep only rows where each of these columns is **greater than 0**:
   - `Flow_Duration`
   - `Init_Win_bytes_forward`
   - `Init_Win_bytes_backward`
   - `Flow_IAT_Min`
   - `Fwd_IAT_Min`
   - `Fwd_IAT_Max`

   (In this dataset, a value of -1 or 0 in these columns means the value was not recorded, not a real measurement.)
6. Check that every attribute except `Label` is now numeric (integer or real).

### Task 3 – Row Count Table

Fill in this table with the numbers Altair AI Studio shows after each step. Include it in your report.

| Step | Rows | BENIGN | DDoS |
|---|---|---|---|
| After loading (Task 1) | | | |
| After removing rows with missing or infinite values (Task 2, steps 2–4) | | | |
| After the "greater than 0" filter (Task 2, step 5) | | | |
| Training set (Task 5) | | | |
| Test set (Task 5) | | | |

### Task 4 – Set the Label and the Positive Class

1. Use **Remap Binominals** on `Label`: set *negative value* to **BENIGN** and *positive value* to **DDoS**. This tells Altair AI Studio that DDoS is the positive class, so precision, recall, and F1 score are calculated for DDoS.
2. Use **Set Role** to make `Label` the **label** (the target the model learns to predict).

Do Remap Binominals **before** Set Role. (If you do it after, tick *include special attributes* in Remap Binominals.)

### Task 5 – Stratified Train/Test Split

Use **Split Data** to split the cleaned data into:

- 80% training data
- 20% testing data

Set *sampling type* to **stratified sampling**, so both parts keep the same share of BENIGN and DDoS rows as the full data. Tick **use local random seed** and type your seed.

### Task 6 – Normalize the Features

Logistic Regression is sensitive to the scale of the features (Week 9). For example, `Flow_Bytes_s` can be in the millions while `Fwd_IAT_Min` is often a small number. Without scaling, the large columns dominate.

1. Add a **Normalize** operator on the **training** output of Split Data, with method **Z-transformation**.
2. Connect the **pre** (preprocessing model) output of Normalize to an **Apply Model** operator, and use that Apply Model to normalize the **test** data.

This way the mean and standard deviation are learned from the training rows only and then reused on the test rows. Normalizing all rows before the split would leak information from the test set into training (data leakage, Week 9).

Use a **Multiply** operator if you need to send the normalized training data (and test data) to both models.

### Task 7 – Train Two Machine Learning Models

Train two models on the normalized training data:

- **Logistic Regression** (required)
- One additional model chosen from:
  - Decision Tree
  - Random Forest
  - Naïve Bayes

Both models use all remaining feature columns as inputs and `Label` as the label. Apply each trained model to the normalized test data with **Apply Model**.

### Task 8 – Model Evaluation

Evaluate both models on the test data using the **Performance (Binominal Classification)** operator. Tick these criteria:

- accuracy
- precision
- recall
- f_measure (F1 score)

Report these four numbers for each model, and include screenshots of the **confusion matrix** for both models. Check that the confusion matrix adds up to the size of your test set (Task 3).

### Task 9 – Model Comparison and Interpretation

Answer these questions in your report. Use your own numbers in every answer.

1. Which model performed better overall?
2. Which model had the higher F1 score?
3. Why might one model perform better than the other on this dataset?
4. **False negatives and false positives.** For each model, how many false negatives and how many false positives did it have on the test set? Explain what each one means for analysts in a Security Operations Centre (SOC). Which one is more harmful in a DDoS situation, and why?
5. **Choosing a metric.** Imagine your model will run in the SOC of an online store. The store's website must stay online during a holiday sale, and the SOC has only three analysts to check every alert. Which **one** metric (accuracy, precision, recall, or F1 score) would you use to choose between your two models, and why? Which of your two models would you pick based on that metric?

----

## Submission Details

Each group submits **two files** on Blackboard (one submission per group), and both members present in Week 12:

1. **Written report (PDF)**
2. **Your exported Altair AI Studio process (.rmp file).** In Altair AI Studio, use **File → Export Process...** and name it with your group number, for example `group07_P2.rmp`.

A submission without the .rmp file loses 2 marks from the report.

### 1. Report Requirements

Your report must show your understanding of the machine learning workflow, with screenshots of each step and explanations of the parameter settings you used.

- **Cover page**

   Include:
   - Your group number
   - Names and student IDs of both group members, and which one is Member 1
   - Your dataset file name and your seed

- **Dataset Overview**

   Include:
   - A brief description of the dataset
   - A screenshot showing the dataset loaded in Altair AI Studio
   - A short explanation of what the dataset represents

- **Data Preprocessing**

   Include screenshots and explanations showing:

   - Attribute type inspection (including `Label` as binominal)
   - Removal of `Destination_Port`
   - Conversion of `Flow_Bytes_s` and `Flow_Packets_s` to numeric
   - Handling of `NaN` and `Infinity` values
   - The "greater than 0" filter
   - Setting the positive class (Remap Binominals) and the label (Set Role)
   - The completed row count table from Task 3

   For each step, include:
   - A screenshot of the operator
   - A screenshot of the operator's parameter settings
   - A short explanation of why the step is necessary

- **Machine Learning Pipeline**

   Include:
   - A screenshot of the complete Altair AI Studio workflow
   - A clear explanation of how the pipeline processes the data, from loading to evaluation
   - Screenshots of the Split Data settings (stratified sampling and your seed) and the Normalize settings

- **Model Training**

   Include screenshots showing:
   - The Logistic Regression operator and its parameter settings
   - The second model and its parameter settings
   - A brief explanation of how each model works

- **Model Performance**

   Include screenshots showing:
   - Accuracy, precision, recall, and F1 score for both models
   - The confusion matrices for both models
   - An explanation of what the metrics tell you about each model

- **Model Comparison and Interpretation**

   Answer all five questions from Task 9.

### 2. Project Presentation

Each group presents its project to the instructor in person during **Week 12**.

During the presentation, students should:

- Open and show their Altair AI Studio workflow
- Explain the data preprocessing steps
- Explain how the two machine learning models work
- Show and interpret the model performance results
- Compare the two models and explain their metric choice from Task 9

Both members must be ready to answer questions about their workflow and results. Each member will be asked their own questions.

----

## Grading Rubric (15 Marks)

| Component | Description | Marks |
|----------|-------------|------|
| **Written Report** | Dataset description, preprocessing steps with screenshots of operators and parameter settings, completed row count table, model training, evaluation metrics, confusion matrices, and answers to the Task 9 questions (including the metric choice and false negative / false positive discussion). Exported .rmp file included. | 5 |
| **Project Presentation** | Group presents the Altair AI Studio workflow, explains the preprocessing steps, demonstrates both machine learning models, and discusses the performance results | 5 |
| **Answering Questions** | Each student shows understanding of the project by correctly answering questions about the dataset, preprocessing steps, models used, and evaluation metrics | 5 |
| **Total** |  | **15 Marks** |
