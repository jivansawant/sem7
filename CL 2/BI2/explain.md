# Practical: RNA-Seq Data Analysis
# Practical: RNA-Seq Data Analysis

Let's understand this practical from the beginning, including the theory, concepts, Python functions, methods, parameters and every important line of code.

The goal is not just to memorize the code. You should understand what each part does so that you can write it yourself during your practical exam.

## 1. Theory: What is RNA-Seq?

RNA-Seq stands for RNA Sequencing.

It is a biological technique used to measure the activity of genes in a cell, tissue or organism.

Our body contains thousands of genes. Not all genes are active at the same time. Some genes are more active, while others are less active.

RNA-Seq helps us measure this gene activity.

Example:

Suppose we want to study the effect of a medicine on gene activity.

We take two groups:

1. Control group: Cells without medicine.
2. Treatment group: Cells given medicine.

We measure the expression of genes in both groups and compare their values.

[Transcriptomic response of overexpression ZNF32 in breast cancer cells | Scientific Reports](https://images.openai.com/static-rsc-4/5ee-RvAsYmMJbYWWJ3BNfZCADuXuUItJK1ymcPtPSe6jEbpCMaalt46kogv28HhBhB-G7joVnuPGHc6xNclTNTftlutqiJNyt8VMiFVxP3v3iNnPmMeO7z3U6yb320xB9is5rx-16VWWHkZis7APqUvKBfAiZ0lBSEFiHiGig2I?purpose=inline)

[nature.com](https://www.nature.com/articles/s41598-024-80125-7?utm_source=chatgpt.com)

If a gene has higher expression in the treatment group, it may be called upregulated. If it has lower expression, it may be called downregulated, provided the statistical criteria are met.

This comparison is called Differential Gene Expression Analysis (DGE).

## 2. Important theoretical concepts

Before understanding the code, learn these concepts.

### A. Gene expression

Gene expression means the process through which information in a gene is used to produce a functional product, such as RNA or protein.

In this practical, we use numerical values to represent gene expression levels.

For example:

| Gene   | Control | Treatment |
| ------ | ------- | --------- |
| Gene_A | 100     | 400       |
| Gene_B | 200     | 180       |

Gene_A has higher expression in the treatment group.

### B. Control and treatment samples

- Control: The normal or untreated group used for comparison.
- Treatment: The group exposed to a drug, chemical or other experimental condition.

We have three samples in each group:

- Control1, Control2, Control3
- Treatment1, Treatment2, Treatment3

Having multiple samples helps us understand variation between samples.

### C. Differential Gene Expression

It means identifying genes whose expression levels differ between control and treatment groups.

We classify genes into three categories:

| Category        | Meaning                                              |
| --------------- | ---------------------------------------------------- |
| Upregulated     | Gene expression increased                            |
| Downregulated   | Gene expression decreased                            |
| Not Significant | No statistically significant difference was detected |

### D. Fold Change

Fold Change measures how much gene expression changed in the treatment group compared with the control group.

Formula:

Fold Change = Average Treatment Expression / Average Control Expression

Example:

- Control average = 100
- Treatment average = 400

Fold Change = 400 / 100 = 4

This means gene expression increased four times.

Remember:

| Fold Change | Meaning               |
| ----------- | --------------------- |
| 1           | No change             |
| 2           | Doubled               |
| 4           | Increased four times  |
| 0.5         | Reduced to half       |
| 0.25        | Reduced to one-fourth |

### E. P-value

A P-value is used in statistical testing to assess whether the observed difference between two groups could reasonably be due to random variation.

In our practical, we use:

- P-value below 0.05: Statistically significant.
- P-value of 0.05 or above: Not statistically significant.

The value 0.05 is called the significance threshold.

Important: A low p-value does not tell us how large the change is. That is why we also calculate Fold Change.

## 3. Python libraries used

Our code uses two libraries.

| Library | Purpose                            |
| ------- | ---------------------------------- |
| Pandas  | Creating and handling tabular data |
| SciPy   | Performing statistical tests       |

### Import statements

```
import pandas as pdfrom scipy.stats import ttest_ind
```

1\. `import pandas as pd`

- Imports the Pandas library.
- Pandas is used to work with data in rows and columns.
- `pd` is an alias, which is a shorter name for Pandas.

2\. `from scipy.stats import ttest_ind`

- SciPy is a Python library used for scientific and mathematical operations.
- `stats` contains statistical functions.
- `ttest_ind` performs an independent two-sample t-test.

We use this function to compare gene expression between control and treatment samples.

## 4. Complete code with detailed explanation

### Step 1: Create the RNA-Seq dataset

```
df = pd.DataFrame({    "Gene": ["Gene_A", "Gene_B", "Gene_C", "Gene_D", "Gene_E"],    "Control1": [100, 200, 150, 80, 300],    "Control2": [110, 190, 160, 85, 290],    "Control3": [105, 210, 155, 75, 310],    "Treatment1": [400, 180, 145, 300, 305],    "Treatment2": [420, 195, 150, 310, 295],    "Treatment3": [390, 205, 158, 290, 315]})
```

Function: `pd.DataFrame()`

It creates a DataFrame.

A DataFrame is a two-dimensional data structure containing rows and columns, similar to an Excel spreadsheet.

Here:

- Each row represents a gene.
- Each column represents a sample or gene name.
- The numbers represent expression counts.

For example, Gene_A has:

- Control values: 100, 110, 105
- Treatment values: 400, 420, 390

Why do we use it?

To create the RNA-Seq dataset that we will analyze.

### Step 2: Display the dataset

```
print("RNA-Seq Dataset:")print(df)
```

Functions:

- `print()` displays information on the screen.
- `df` is the DataFrame containing our dataset.

This allows us to check the data before performing analysis.

### Step 3: Create an empty results list

```
results = []
```

This creates an empty Python list.

We use it to store the calculated results for every gene.

For each gene, we will store:

1. Gene name.
2. Fold Change.
3. P-value.
4. Expression status.

### Step 4: Process each gene using a loop

```
for i in range(len(df)):
```

Let's understand the individual parts.

- `for`: Starts a loop.
- `i`: A variable representing the current row index.
- `len(df)`: Returns the number of rows in the DataFrame.
- `range()`: Generates a sequence of numbers.

Since our dataset contains five genes, the loop runs five times.

The values of `i` will be:

`0, 1, 2, 3, 4`

Why do we use a loop?

Instead of writing separate code for every gene, we write the comparison once and repeat it for all five genes.

### Step 5: Separate control and treatment values

```
control = df.iloc[i, 1:4].astype(float)treatment = df.iloc[i, 4:7].astype(float)
```

This is one of the most important parts of the code.

What is `iloc[]`?

`iloc[]` is a Pandas method used to select rows and columns using their numerical positions.

Its basic structure is:

```
df.iloc[row_position, column_position]
```

For example:

```
df.iloc[0, 1:4]
```

This selects the first row and columns 1, 2 and 3.

In our dataset:

| Column position | Column name |
| --------------- | ----------- |
| 0               | Gene        |
| 1               | Control1    |
| 2               | Control2    |
| 3               | Control3    |
| 4               | Treatment1  |
| 5               | Treatment2  |
| 6               | Treatment3  |

Therefore:

```
control = df.iloc[i, 1:4]
```

Selects the three control values for the current gene.

```
treatment = df.iloc[i, 4:7]
```

Selects the three treatment values.

What is `astype(float)`?

It converts the selected values into floating-point numbers (decimal-number format).

Example:

- Integer: `100`
- Float: `100.0`

We use this because the statistical function expects numerical input. It also prevents the data-type error you encountered earlier.

### Step 6: Perform the t-test

```
p_value = ttest_ind(treatment, control).pvalue
```

This line performs a statistical comparison between treatment and control.

Function: `ttest_ind()`

It performs an independent two-sample t-test.

Its basic syntax is:

```
ttest_ind(sample1, sample2)
```

Parameters:

| Parameter   | Meaning                           |
| ----------- | --------------------------------- |
| `treatment` | First group of expression values  |
| `control`   | Second group of expression values |

There are other optional parameters:

- `equal_var=True`: Assumes both groups have equal variance. This is the default.
- `equal_var=False`: Does not assume equal variance; performs Welch's t-test.
- `alternative`: Specifies the alternative hypothesis, such as two-sided or one-sided.

Our simplified code uses the default `equal_var=True`.

What is `.pvalue`?

The t-test returns a result containing a test statistic and a p-value.

`.pvalue` extracts only the p-value from that result.

We store it in the variable `p_value`.

Why do we use it?

To determine whether the difference between the control and treatment groups is statistically significant.

### Step 7: Calculate Fold Change

```
fold_change = treatment.mean() / control.mean()
```

Let's divide this into two parts.

1\. `treatment.mean()`

Calculates the average expression of the treatment samples.

2\. `control.mean()`

Calculates the average expression of the control samples.

3\. `/`

Divides treatment average by control average.

Example for Gene_A:

Treatment values = 400, 420, 390

Control values = 100, 110, 105

Treatment average = 403.33

Control average = 105

Fold Change = 403.33 / 105 = approximately 3.84.

This means Gene_A expression is approximately 3.84 times higher in the treatment group.

### Step 8: Classify the genes

```
if p_value < 0.05 and fold_change >= 2:    status = "Upregulated"elif p_value < 0.05 and fold_change <= 0.5:    status = "Downregulated"else:    status = "Not Significant"
```

This section uses conditional statements.

What is `if`?

Checks whether a condition is true.

What is `elif`?

Checks another condition if the previous `if` condition was false.

What is `else`?

Executes when none of the previous conditions are true.

Let's understand the conditions.

Condition 1:

```
if p_value < 0.05 and fold_change >= 2:
```

If:

- P-value is below 0.05.
- Fold Change is 2 or greater.

The gene is classified as Upregulated.

Condition 2:

```
elif p_value < 0.05 and fold_change <= 0.5:
```

If:

- P-value is below 0.05.
- Fold Change is 0.5 or lower.

The gene is classified as Downregulated.

Condition 3:

```
else:
```

If neither condition is satisfied, the gene is classified as Not Significant.

What is `and`?

It is a logical operator. Both conditions must be true for the complete condition to be true.

### Step 9: Store the results

```
results.append([df["Gene"][i], fold_change, p_value, status])
```

Function: `append()`

Adds a new item to the end of a list.

Here, we add four values for every gene:

- `df["Gene"][i]`: Current gene name.
- `fold_change`: Calculated fold change.
- `p_value`: Calculated p-value.
- `status`: Classification of the gene.

After the loop finishes, the list contains results for all five genes.

### Step 10: Create the final results table

```
result_df = pd.DataFrame(    results,    columns=["Gene", "Fold Change", "P Value", "Status"])
```

We use `pd.DataFrame()` again to convert the results list into a table.

`columns` specifies the names of the table columns.

### Step 11: Display the final results

```
print("\nDifferential Gene Expression Results:")print(result_df.round(4).to_string(index=False))
```

Let's understand the methods:

| Method        | Purpose                                           |
| ------------- | ------------------------------------------------- |
| `round(4)`    | Rounds numerical values to four decimal places    |
| `to_string()` | Converts the DataFrame into a readable text table |
| `index=False` | Hides the default row numbers                     |
| `print()`     | Displays the output                               |

`\n` creates a new line before printing the result heading.

## 5. Expected output

The result will look approximately like this:

| Gene   | Fold Change | Status          |
| ------ | ----------- | --------------- |
| Gene_A | 3.84        | Upregulated     |
| Gene_B | 0.97        | Not Significant |
| Gene_C | 0.97        | Not Significant |
| Gene_D | 3.87        | Upregulated     |
| Gene_E | 1.00        | Not Significant |

The p-values will also appear in the actual output.

## 6. Important Python functions and methods to remember

This is your quick revision table for the practical.

| Function / Method | Purpose                              |
| ----------------- | ------------------------------------ |
| `pd.DataFrame()`  | Creates a table                      |
| `print()`         | Displays output                      |
| `len()`           | Returns number of rows/items         |
| `range()`         | Generates a sequence of numbers      |
| `df.iloc[]`       | Selects data by position             |
| `.astype(float)`  | Converts values to decimal numbers   |
| `ttest_ind()`     | Compares two independent groups      |
| `.pvalue`         | Extracts p-value                     |
| `.mean()`         | Calculates average                   |
| `append()`        | Adds an item to a list               |
| `.round(4)`       | Rounds values to four decimal places |
| `.to_string()`    | Displays DataFrame as text           |

## 7. Viva questions and answers

Q1. What is RNA-Seq?

RNA-Seq is a sequencing technique used to measure gene expression levels.

Q2. What is the main aim of this practical?

To compare gene expression between control and treatment samples and identify differentially expressed genes.

Q3. What is a gene?

A gene is a segment of DNA that contains information to produce a functional product.

Q4. What is gene expression?

Gene expression is the process by which information from a gene is used to produce RNA or protein.

Q5. What is differential gene expression?

It is the comparison of gene expression levels between two experimental conditions.

Q6. What is Fold Change?

It is the ratio of average treatment expression to average control expression.

Q7. What is a P-value?

It is a statistical measure used to assess whether an observed difference is statistically significant.

Q8. What is the significance threshold used in this practical?

0.05.

Q9. What is upregulation?

An increase in gene expression in the treatment group compared with the control group.

Q10. What is downregulation?

A decrease in gene expression in the treatment group compared with the control group.

Q11. Why do we use `ttest_ind()`?

To compare the expression values of two independent groups.

Q12. Why do we use Pandas?

To create, organize and manipulate tabular data.

Q13. Why do we use a for loop?

To perform the same analysis on every gene without writing repeated code.

Q14. What does `iloc[]` do?

It selects rows and columns using their numerical positions.

Q15. What is the difference between control and treatment?

Control represents normal or untreated conditions, whereas treatment represents conditions after an experimental intervention.

## 8. Important scientific limitation to understand

I want to point out something so you don't accidentally give an incorrect answer in your viva.

Our code is a simplified educational demonstration, not a complete professional RNA-Seq pipeline.

Real RNA-Seq analysis generally requires:

1. Quality checking of sequencing reads.
2. Alignment or quantification of reads.
3. Normalization of gene counts.
4. Differential expression analysis.
5. Correction for testing thousands of genes.

Professional tools such as DESeq2 and edgeR are commonly used for the statistical analysis.

Our code skips those advanced steps to keep the practical manageable. Also, a real RNA-Seq analysis should use count-aware statistical methods rather than relying on a simple t-test on raw counts.

## 9. What you should actually memorize for the exam

Don't try to memorize every explanation above. Focus on these five things:

1. Dataset: Contains gene names and control/treatment expression values.
2. Loop: Processes each gene one by one.
3. T-test: Compares the two groups statistically.
4. Fold Change: Measures how much expression increased or decreased.
5. Conditions: Classify genes as upregulated, downregulated or not significant.

If you understand these five things, you understand the central logic of this practical.
Aim: To analyze an RNA-Seq dataset and perform differential gene expression analysis to identify genes whose expression levels change between control and treatment samples.

## 1. What is RNA-Seq?

RNA-Seq stands for RNA Sequencing. It is a technique used to measure how active different genes are in a cell or tissue.

For example:

- A control sample represents normal conditions.
- A treatment sample represents conditions after giving a drug or applying some treatment.
- By comparing both samples, we can identify which genes have increased or decreased their activity.

This comparison is called Differential Gene Expression Analysis.

## 2. Code

```
import pandas as pdfrom scipy.stats import ttest_ind# 1. Create RNA-Seq datasetdf = pd.DataFrame({    "Gene": ["Gene_A", "Gene_B", "Gene_C", "Gene_D", "Gene_E"],    "Control1": [100, 200, 150, 80, 300],    "Control2": [110, 190, 160, 85, 290],    "Control3": [105, 210, 155, 75, 310],    "Treatment1": [400, 180, 145, 300, 305],    "Treatment2": [420, 195, 150, 310, 295],    "Treatment3": [390, 205, 158, 290, 315]})print("RNA-Seq Dataset:")print(df)# 2. Compare gene expressionresults = []for i in range(len(df)):    control = df.iloc[i, 1:4]    treatment = df.iloc[i, 4:7]    p_value = ttest_ind(treatment, control).pvalue    fold_change = treatment.mean() / control.mean()    if p_value < 0.05 and fold_change >= 2:        status = "Upregulated"    elif p_value < 0.05 and fold_change <= 0.5:        status = "Downregulated"    else:        status = "Not Significant"
```

## 3. Explanation of the code

### Step 1: Import libraries

```
import pandas as pdfrom scipy.stats import ttest_ind
```

- `pandas`: Used to create and manage tabular data.
- `ttest_ind`: Used to compare two independent groups and calculate a p-value.

### Step 2: Create the dataset

```
df = pd.DataFrame({    "Gene": ["Gene_A", "Gene_B", "Gene_C", "Gene_D", "Gene_E"],    "Control1": [100, 200, 150, 80, 300],    "Control2": [110, 190, 160, 85, 290],    "Control3": [105, 210, 155, 75, 310],    "Treatment1": [400, 180, 145, 300, 305],    "Treatment2": [420, 195, 150, 310, 295],    "Treatment3": [390, 205, 158, 290, 315]})
```

- Creates a dataset containing five genes.
- Each gene has three control and three treatment measurements.
- The numbers represent gene expression counts.

For example, Gene_A has control values of 100, 110, 105 and treatment values of 400, 420, 390.

### Step 3: Prepare to store results

```
results = []
```

Creates an empty list to store the analysis results for each gene.

### Step 4: Compare gene expression

```
for i in range(len(df)):    control = df.iloc[i, 1:4]    treatment = df.iloc[i, 4:7]
```

- `for` loop processes each gene one by one.
- `len(df)` gives the total number of genes (5).
- `df.iloc[i, 1:4]` selects the three control values.
- `df.iloc[i, 4:7]` selects the three treatment values.

In simple words: We take one gene at a time and separate its control and treatment measurements.

### Step 5: Calculate P Value

```
p_value = ttest_ind(treatment, control).pvalue
```

- Performs a statistical t-test between the two groups.
- Calculates the p-value to help determine whether the difference is statistically significant.
- We use 0.05 as the significance cutoff.

### Step 6: Calculate Fold Change

```
fold_change = treatment.mean() / control.mean()
```

- `mean()` calculates the average expression.
- Divides treatment average by control average.
- This tells us how much gene expression has changed.

Example:

If control average = 100 and treatment average = 400:

Fold Change = 400 / 100 = 4

This means gene expression increased four times.

### Step 7: Identify gene expression status

```
if p_value < 0.05 and fold_change >= 2:    status = "Upregulated"elif p_value < 0.05 and fold_change <= 0.5:    status = "Downregulated"else:    status = "Not Significant"
```

This condition classifies the genes:

| Condition                                      | Meaning         |
| ---------------------------------------------- | --------------- |
| P Value below 0.05 and Fold Change at least 2  | Upregulated     |
| P Value below 0.05 and Fold Change at most 0.5 | Downregulated   |
| Any other result                               | Not Significant |

### Step 8: Store the results

```
results.append([df["Gene"][i], fold_change, p_value, status])
```

Stores the gene name, fold change, p-value and expression status in the results list.

### Step 9: Display final results

```
result_df = pd.DataFrame(    results,    columns=["Gene", "Fold Change", "P Value", "Status"])print("\nDifferential Gene Expression Results:")print(result_df.round(4))
```

- Creates a new table containing the analysis results.
- `round(4)` displays numerical values up to four decimal places.
- Prints the final differential gene expression results.

## 4. Expected output

The final results will be approximately:

| Gene   | Fold Change | Status          |
| ------ | ----------- | --------------- |
| Gene_A | 3.90        | Upregulated     |
| Gene_B | 0.97        | Not Significant |
| Gene_C | 0.97        | Not Significant |
| Gene_D | 3.87        | Upregulated     |
| Gene_E | 1.00        | Not Significant |

The exact p-values are calculated when the code runs.

## 5. Important terms for viva

| Term                    | Meaning                                              |
| ----------------------- | ---------------------------------------------------- |
| RNA-Seq                 | Technique used to measure gene expression.           |
| Gene expression         | Activity of a gene.                                  |
| Control sample          | Normal or untreated sample.                          |
| Treatment sample        | Sample exposed to a treatment.                       |
| Differential expression | Change in gene expression between two groups.        |
| Fold Change             | Ratio of treatment expression to control expression. |
| P Value                 | Measure used to assess statistical significance.     |
| Upregulated             | Gene expression has increased.                       |
| Downregulated           | Gene expression has decreased.                       |

## 6. Viva questions and answers

Q1. What is the main purpose of RNA-Seq?

To measure gene expression and understand which genes are active in a sample.

Q2. What is differential gene expression analysis?

It is the process of comparing gene expression between two groups to identify genes that have increased or decreased activity.

Q3. Why do we use control and treatment samples?

To compare normal gene expression with gene expression after treatment.

Q4. What is Fold Change?

It measures how many times gene expression has increased or decreased compared with the control group.

Q5. What is a p-value?

It helps determine whether the observed difference between groups is statistically significant.

Q6. What does upregulated mean?

The gene has increased expression in the treatment group.

Q7. What does downregulated mean?

The gene has decreased expression in the treatment group.

Q8. Which Python libraries are used?

Pandas is used for handling data, and SciPy is used for the statistical t-test.

## 7. Result

RNA-Seq data analysis was performed successfully. Gene expression levels in control and treatment samples were compared using fold change and a statistical t-test. Genes were classified as upregulated, downregulated, or not significant.

One important note for your practical: This is a simplified educational demonstration using manually created expression counts. It does not include RNA-Seq normalization or multiple-testing correction, which are important in real RNA-Seq studies. If asked, explain that this code demonstrates the basic concept of differential gene expression rather than a complete professional RNA-Seq pipeline.