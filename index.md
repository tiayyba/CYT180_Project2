<h1 align="center" style="text-align:center;">CYT180 — Project 2 — Fall2026</h1>
<h2 align="center" style="text-align:center;">Network Traffic Classification Using Machine Learning Algorithms (15%)</h2>

**Work type:** Group project (2 students, or solo) <br>
**Group registration:** Blackboard discussion post, before the end of Week 10 <br>
**Report due:** Dec 1 @ 11:59 PM, on Blackboard<br>
**Presentations:** Week 12, in person, to the instructor<br>

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

There are 19 group numbers, from **01** to **19**. Each group chooses one free group number and replies to the Project 2 Blackboard discussion thread with:

- Your chosen **group number** (01 to 19)
- The full names of both group members

Before you post, read the earlier replies and pick a number that no other group has taken. If two groups post the same number, the group that posted first keeps it, and the other group must pick a new free number. Your group number decides your dataset file, so you can download it as soon as you have posted.

The post must be submitted **before the end of Week 10**. All presentations take place in **Week 12**. Week 14 is a backup week, used only if a group cannot present in Week 12 for an approved reason. The instructor will post the presentation schedule after registration closes.

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

When Altair AI Studio splits the data into training and test sets (Task 5), it picks the rows at random. A **random seed** is a number that fixes this random choice, so you get the same split, and the same results, every time you run your process.

**Everyone uses the seed 1992** (Altair AI Studio's default). Wherever an operator has the option **use local random seed**, tick it and make sure the seed is 1992. Your screenshots must show this setting.

----

## Project Tasks

Each task tells you **what** your workflow must do and the **rules** it must follow. It is up to you to find the right Altair AI Studio operators, connect them, and choose their settings. Use the operator search box, the help text that Altair shows for each operator, and the Week 9 and Week 10 material. Where a step is known to be tricky, a **Hint** is given.

Follow the rules exactly. Every group's dataset file is different, and your row counts are checked against your own file.

**Seeing your results:** to see the data after any step, connect that operator's output to a **res** port (the small half-circles on the right edge of the process canvas) and click **Run**. The number of rows is shown at the bottom of the **Data** view, for example "ExampleSet (79,010 examples, 0 special attributes, 77 regular attributes)". The **Statistics** view shows each attribute's type, missing values, and the BENIGN/DDoS counts for `Label`. Keep a record of the number of rows after each task; you need them for Task 3.

### Task 1 – Load the Dataset

Load your group's file into Altair AI Studio and look at the type of each attribute.

Rules:
- `Label` must be **binominal** (exactly two values), which is what a yes/no classifier needs.
- Record the number of rows loaded and the number of BENIGN and DDoS rows.

**Hint (import types):** Altair guesses each attribute's type by looking only at the first rows of the file. For `Flow_Bytes_s` and `Flow_Packets_s` it guesses a number type, but a few rows further down contain the text `Infinity` or `NaN`, and the import fails with *"Unparseable number: Infinity"*. In the last step of the import wizard, set these two attributes to **polynominal** (text). You turn them back into numbers in Task 2.

### Task 2 – Data Cleaning

Your workflow must do the following, in this order:

1. **Remove the `Destination_Port` attribute.** In this dataset almost all DDoS flows go to port 80, so a model could "cheat" by learning the port number instead of the traffic behavior. A real attacker can use any port, so a model like that would not work in practice.
2. **Turn `Flow_Bytes_s` and `Flow_Packets_s` into numeric attributes.** Any value that is not a real number (`NaN` or `Infinity`) must become a **missing value**. Infinite values break the calculations inside the models, so they must not stay in the data.
3. **Remove every row that has a missing value.**
4. **Remove rows with zero or invalid values.** Keep only the rows where **all six** of these attributes are **greater than 0** (strictly greater, not "greater than or equal"):
   - `Flow_Duration`
   - `Init_Win_bytes_forward`
   - `Init_Win_bytes_backward`
   - `Flow_IAT_Min`
   - `Fwd_IAT_Min`
   - `Fwd_IAT_Max`

   In this dataset, a value of -1 or 0 in these columns means the value was not recorded, not a real measurement.

After these steps, every attribute except `Label` must be numeric (integer or real). Use the **Statistics** view to check your work after each step.

### Task 3 – Row Count Table

Fill in this table with the numbers Altair AI Studio shows after each step. Include it in your report.

| Step | Rows | BENIGN | DDoS |
|---|---|---|---|
| After loading (Task 1) | | | |
| After removing rows with missing values (Task 2, steps 2–3) | | | |
| After the "greater than 0" filter (Task 2, step 4) | | | |
| Training set (Task 5) | | | |
| Test set (Task 5) | | | |

### Task 4 – Set the Label and the Positive Class

Rules:
- `Label` must have the **label** role (the target the model learns to predict).
- **DDoS** must be the **positive class** and BENIGN the negative class, so that precision, recall, and F1 score are calculated for DDoS.

**Hint (positive class):** look for an operator that remaps the two values of a binominal attribute into a negative and a positive value. Set it up **before** you give `Label` the label role. You can check that it worked in the **Statistics** view: `Label` shows **Negative = BENIGN** and **Positive = DDoS**.

### Task 5 – Train/Test Split

Rules:
- 80% training data, 20% test data.
- The split must be **stratified**, so both parts keep the same share of BENIGN and DDoS rows as the cleaned data.
- Use a fixed random seed: tick **use local random seed** and set the seed to **1992**. With the same seed you get the same split, and the same results, every time you run the process.

### Task 6 – Normalize the Features

Logistic Regression is sensitive to the scale of the features (Week 9). For example, `Flow_Bytes_s` can be in the millions while `Fwd_IAT_Min` is often a small number. Without scaling, the large columns dominate.

Rules:
- Use **Z-transformation** (mean 0, standard deviation 1).
- The mean and standard deviation must be learned from the **training data only**, and then the **same values** must be used to scale the test data. Normalizing all rows before the split, or normalizing the test data on its own, leaks information from the test set (data leakage, Week 9).

**Hint (Normalize wiring):** connect the **training** output of the split to **Normalize**. Normalize has an output called **pre** (preprocessing model), which stores the training mean and standard deviation. Send **pre** and the **test** data into an **Apply Model** operator; its output is your normalized test data. Check the **Statistics** view: in the training data every average is 0; in the test data the averages are close to 0 but not exactly 0. Be ready to explain why.

### Task 7 – Train Two Machine Learning Models

Train two models on the normalized training data, and apply each one to the normalized test data:

- **Logistic Regression** (required)
- One additional model chosen from:
  - Decision Tree
  - Random Forest
  - Naïve Bayes

Rules:
- Both models use all remaining feature columns as inputs and `Label` as the label.
- Build your process **once** with Logistic Regression, evaluate it (Task 8), and export it as `groupNN_P2_LR.rmp`. Then replace Logistic Regression with your second model (nothing else changes, so both models are trained and tested on exactly the same rows), evaluate it, and export it as `groupNN_P2_model2.rmp`. Tip: right-clicking an operator gives you **Replace Operator**.

### Task 8 – Model Evaluation

Evaluate each model on the test data with a performance operator for **binominal** (two-class) classification. Your results must show:

- accuracy
- precision
- recall
- F1 score (called *f_measure* in Altair)
- the confusion matrix

When you click **precision** in the results, the heading must say **positive class: DDoS**. If it does not, check Task 4. The four numbers in each confusion matrix must add up to the size of your test set (Task 3).

### Task 9 – Model Comparison and Interpretation

Answer these questions in your report. Use your own numbers in every answer.

1. Which model performed better overall?
2. Which model had the higher F1 score?
3. Why might one model perform better than the other on this dataset?
4. **False negatives and false positives.** For each model, how many false negatives and how many false positives did it have on the test set? Explain what each one means for analysts in a Security Operations Centre (SOC). Which one is more harmful in a DDoS situation, and why?
5. **Choosing a metric.** Imagine your model will run in the SOC of an online store. The store's website must stay online during a holiday sale, and the SOC has only three analysts to check every alert. Which **one** metric (accuracy, precision, recall, or F1 score) would you use to choose between your two models, and why? Which of your two models would you pick based on that metric?
6. **Is this realistic?** Your models are probably more than 99% accurate. Give two reasons why a model trained on this dataset would likely perform worse on real network traffic in a SOC. (Think about how and where this data was collected.)

----

## Submission Details

Each group submits **two files** on Blackboard (one submission per group), and both members present in Week 12:

1. **Written report (PDF)**
2. **Your two exported Altair AI Studio processes (.rmp files)**, one with Logistic Regression and one with your second model (Task 7). In Altair AI Studio, use **File → Export Process...** and name them with your group number, for example `group07_P2_LR.rmp` and `group07_P2_model2.rmp`.

### 1. Report Requirements

Your report shows that you built and ran the workflow yourselves. It has a cover page, **9 required screenshots**, the row count table from Task 3, and your answers to the Task 9 questions.

**How to take a screenshot:** in Altair AI Studio, click an operator in your process. Its settings appear in the **Parameters** panel on the right. Take one screenshot that shows both the operator and its Parameters panel. Results screenshots must show the tab name and the "ExampleSet (... examples ...)" line at the bottom of the Data view.

**Cover page**

- Your group number
- Names and student IDs of both group members
- Your dataset file name

**Required screenshots**

Under each screenshot, write 2–3 sentences in your own words explaining what the step does and why it is needed.

| # | Screenshot | What it must show |
|---|---|---|
| 1 | Complete workflow | Your whole process, with every operator visible |
| 2 | Loaded data | The results view after loading: your file name, the number of rows, and `Flow_Bytes_s` and `Flow_Packets_s` as polynominal |
| 3 | Fixing the number columns | The operator(s) you used with their settings, and the Statistics view showing `Flow_Bytes_s` and `Flow_Packets_s` as numeric with their missing values |
| 4 | "Greater than 0" filter settings | The operator you used, with the six conditions visible |
| 5 | **Rows after the filter** | The results view after the "greater than 0" filter, showing the number of rows. This number is different for every group's file. |
| 6 | Positive class and label | The operators you used with their settings, plus the **Statistics** view where `Label` is the label and shows Negative = BENIGN and Positive = DDoS |
| 7 | Train/test split | 80/20, stratified, use local random seed = 1992 |
| 8 | Normalize | Z-transformation, and how the training mean and standard deviation are applied to the test data |
| 9 | Results for both models | For each model: accuracy, precision, recall, F1 score, and the confusion matrix |

Also include a screenshot of each model's operator with its parameter settings (Logistic Regression and your second model), with 2–3 sentences on how each model works.

**Row count table:** include your completed table from Task 3. The numbers must match your screenshots 2 and 5, and the four numbers in each confusion matrix must add up to your test set size.

**Task 9 answers:** answer all six questions using your own numbers.

### 2. Project Presentation and Questions (Week 12)

Each group meets the instructor in person during **Week 12**. Bring a laptop with Altair AI Studio, your two .rmp files, and your group's dataset file.

**Live demo.** You will:

1. Open one of your .rmp files (the instructor chooses which) and run it on your group's dataset file. The row count after the "greater than 0" filter and the model results must match your report.
2. Make one small change that the instructor asks for on the spot (for example, change the split to 70/30, or remove the Normalize operator), run it again, and explain what changed in the results and why.

**Individual questions.** Each student is asked **three questions** and receives their **own mark** for this part. Questions are about your own workflow and your own numbers, for example:

- Why did your number of rows drop so much at the "greater than 0" filter, and which class lost more rows?
- Point to the false negatives in your confusion matrix. What do they mean for a SOC analyst?
- Why is Normalize learned from the training data only?

If a student cannot explain a part of the report, marks for that part of the report may be reduced for that student.

----

## Grading Rubric (15 Marks)

| Component | What is marked | Marks |
|----------|-------------|------|
| **Report: screenshots and numbers** | All 9 required screenshots are included, and the numbers (row counts, confusion matrices) match your group's dataset file and each other | 3 |
| **Report: Task 9 answers** | Clear answers to the six Task 9 questions, using your own numbers (including the false negative / false positive discussion and the metric choice) | 2 |
| **Live demo** | The group runs its own .rmp files on its own file with matching results (2), and makes the requested change and explains its effect (2) | 4 |
| **Individual questions** | Each student answers three questions about their own workflow and results (2 marks per question, marked for each student separately) | 6 |
| **Total** |  | **15 Marks** |

A submission without the .rmp files loses 2 marks from the report. Results that match another group's dataset file, instead of your own, receive 0 for the project.
