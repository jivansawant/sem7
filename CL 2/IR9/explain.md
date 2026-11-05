Let's forget the complicated terminology for a moment. I'll teach you this practical as if you're seeing Bayesian Networks for the first time.

We'll understand what the program is doing, why each step is needed, and how the computer predicts heart disease.

# 1. First, what are we trying to achieve?

Imagine you have a dataset containing information about 1000 heart patients.

Each patient has information like:

| Chest pain (`cp`) | Exercise angina (`exang`) | Heart rate (`thalach`) | Diagnosis |
| ----------------- | ------------------------- | ---------------------- | --------- |
| 4                 | 1                         | 110                    | 1         |
| 2                 | 0                         | 170                    | 0         |
| 3                 | 1                         | 130                    | 1         |
| 1                 | 0                         | 180                    | 0         |

Our job is to teach the computer using this information.

After learning, we give it a new patient:

- Chest pain = 4
- Exercise angina = 1
- Heart rate = Low

The computer calculates whether the patient is likely to have heart disease.

This is called Machine Learning classification.

# 2. What is a Bayesian Network?

A Bayesian Network is a model that uses probability to represent relationships between different variables.

In our practical, the network looks like this:

## Heart Disease Bayesian Network

### Diagnosis

0 = No disease, 1 = Disease

cp

Chest pain

exang

Exercise angina

thalach

Heart rate group

The diagram shows the simplified network structure used in our code. Diagnosis is the parent node, and the three medical features are child nodes.

Understand these terms:

- Node: A variable, such as age or diagnosis.
- Edge: An arrow showing a probabilistic dependency.
- Parent: The variable from which an arrow starts.
- Child: The variable where an arrow ends.
- Probability: The chance of something happening.

For example, the arrow from Diagnosis to chest pain represents that the model learns how likely different chest pain categories are for each diagnosis category.

# 3. Now understand the code

## Step 1: Import libraries

```
import pandas as pdfrom pgmpy.models import DiscreteBayesianNetworkfrom pgmpy.estimators import BayesianEstimatorfrom pgmpy.inference import VariableElimination
```

What is importing?

We are bringing ready-made tools into Python.

| Command                   | What it does                               |
| ------------------------- | ------------------------------------------ |
| `pandas`                  | Handles the dataset                        |
| `DiscreteBayesianNetwork` | Creates our Bayesian Network               |
| `BayesianEstimator`       | Learns probability values                  |
| `VariableElimination`     | Calculates predictions using probabilities |

Remember these three pgmpy names:

1. Network
2. Estimator
3. Inference

They are the three main components of this practical.

## Step 2: Load the dataset

```
data = pd.read_csv("heart_disease.csv")data.columns = data.columns.str.strip()
```

First line reads the CSV file.

Second line removes unnecessary spaces from column names.

For example:

`" diagnosis "` becomes `"diagnosis"`.

Why do we do this?

Because Python treats `"diagnosis"` and `" diagnosis "` as different column names.

## Step 3: Select columns

```
data = data[["cp", "exang", "thalach", "diagnosis"]].copy()
```

Our original dataset might have 14 or more columns.

But we only need four columns for this simple network.

| Column      | Meaning                     |
| ----------- | --------------------------- |
| `cp`        | Chest pain type             |
| `exang`     | Exercise-induced angina     |
| `thalach`   | Maximum heart rate achieved |
| `diagnosis` | Heart disease result        |

Why `.copy()`?

It creates a separate copy of the selected data so that our changes do not unintentionally affect the original DataFrame.

## Step 4: Convert values into numbers

```
data = data.apply(pd.to_numeric, errors="coerce")data = data.dropna()
```

First line converts the selected columns into numbers.

Second line removes rows containing missing or invalid values.

Example:

| cp | exang | thalach | diagnosis |
| -- | ----- | ------- | --------- |
| 4  | 1     | 110     | 1         |
| 2  | 0     | 170     | 0         |
| 3  | 1     | NaN     | 1         |

The third row is removed because its heart rate is missing.

`NaN` means Not a Number. It is commonly used to represent missing data.

## Step 5: Convert diagnosis into two classes

```
data["diagnosis"] = (data["diagnosis"] > 0).astype(int)
```

The original diagnosis might contain values from 0 to 4.

We convert them into two classes:

| Original diagnosis | New diagnosis |
| ------------------ | ------------- |
| 0                  | 0             |
| 1                  | 1             |
| 2                  | 1             |
| 3                  | 1             |
| 4                  | 1             |

Why?

Because we want only two predictions:

- 0 = No heart disease
- 1 = Heart disease

This is called binary classification.

## Step 6: Convert heart rate into categories

This is an important step. Understand it carefully.

```
data["thalach_group"] = pd.cut(    data["thalach"],    bins=[0, 120, 160, float("inf")],    labels=["Low", "Medium", "High"])
```

The problem is that `thalach` contains numerical values.

For example:

- 110
- 130
- 170

We want to convert these numbers into categories.

Heart rate grouping used in this practical

Low

0–120

Medium

120–160

High

Above 160

Examples:

| Original value | New category |
| -------------- | ------------ |
| 110            | Low          |
| 145            | Medium       |
| 175            | High         |

What is `pd.cut()`?

It is a pandas function that divides numerical values into groups or intervals.

Why do we need it?

Because we are using a Discrete Bayesian Network, which works with discrete categories.

Note: These are manually chosen teaching categories, not medical reference ranges.

## Step 7: Convert categories into strings

```
data["cp"] = data["cp"].astype(int).astype(str)data["exang"] = data["exang"].astype(int).astype(str)data["thalach_group"] = data["thalach_group"].astype(str)
```

This converts the feature values into strings.

For example:

- Number `4` becomes `"4"`.
- Number `1` becomes `"1"`.
- Category `Low` remains `"Low"`.

Why?

We want the network to treat these as discrete states or categories rather than continuous numerical measurements.

Then:

```
data = data[["diagnosis", "cp", "exang", "thalach_group"]]
```

Keeps only the four columns we need, in the order used for our network data.

## Step 8: Create the Bayesian Network

This is the most important step.

```
model = DiscreteBayesianNetwork([    ("diagnosis", "cp"),    ("diagnosis", "exang"),    ("diagnosis", "thalach_group")])
```

What is happening?

We are creating the network structure.

Each pair represents an arrow.

For example:

```
("diagnosis", "cp")
```

Means:

Diagnosis → Chest pain.

Similarly:

```
("diagnosis", "exang")
```

Means:

Diagnosis → Exercise angina.

And:

```
("diagnosis", "thalach_group")
```

Means:

Diagnosis → Heart rate group.

Remember: These arrows define the model's assumed probabilistic relationships. They do not prove that diagnosis causes these medical features.

## Step 9: Learn probability values

```
model.fit(data, estimator=BayesianEstimator)
```

This is where the model learns.

It examines the dataset and calculates probabilities.

For example, it learns things like:

- Probability of chest pain type 4 when diagnosis is 1.
- Probability of exercise angina when diagnosis is 1.
- Probability of low heart rate when diagnosis is 0.

These probabilities are stored in something called a Conditional Probability Distribution (CPD).

What does CPD mean?

It is simply a table of probabilities.

For example, an illustrative table:

| Diagnosis      | P(cp = 4) |
| -------------- | --------- |
| No disease (0) | 10%       |
| Disease (1)    | 60%       |

These are just example numbers to understand the concept, not your dataset's actual results.

`BayesianEstimator` estimates the probabilities using the data and a prior to avoid zero-probability problems.

Next:

```
print("Network valid:", model.check_model())
```

Checks whether the network is correctly constructed and its probability distributions are valid.

If valid, it returns `True`.

## Step 10: Create the inference engine

```
inference = VariableElimination(model)
```

Now our network has learned the probabilities.

But how do we use it to predict a new patient?

We need an inference engine.

`VariableElimination` calculates the probability of a variable based on the evidence we provide.

Think of it like asking a question to the trained network.

## Step 11: Give new patient information

```
evidence = {    "cp": "4",    "exang": "1",    "thalach_group": "Low"}
```

This is the new patient's information.

| Feature          | Value |
| ---------------- | ----- |
| Chest pain       | 4     |
| Exercise angina  | 1     |
| Heart rate group | Low   |

We are asking:

"Given these three characteristics, what is the probability that this patient has heart disease?"

## Step 12: Calculate diagnosis probabilities

```
result = inference.query(    variables=["diagnosis"],    evidence=evidence)
```

Understand these two parameters:

- `variables` = What do we want to predict?
- `evidence` = What information do we already know?

Here:

`variables=["diagnosis"]` means we want the diagnosis.

`evidence=evidence` means we already know the patient's medical details.

The result may look conceptually like this:

| Diagnosis      | Probability |
| -------------- | ----------- |
| 0 – No disease | 0.25        |
| 1 – Disease    | 0.75        |

Again, these are illustrative probabilities.

## Step 13: Select the highest probability

```
states = result.state_names["diagnosis"]prediction = states[result.values.argmax()]
```

Let's understand this.

`result.state_names["diagnosis"]` gets the possible diagnosis categories.

They are:

```
[0, 1]
```

`result.values` contains the probabilities.

For example:

```
[0.25, 0.75]
```

What does `.argmax()` do?

It finds the position of the largest value.

Here:

- 0.25 is at position 0.
- 0.75 is at position 1.

Therefore:

```
result.values.argmax()
```

Returns `1`.

Then we use that position to get the corresponding diagnosis state.

Finally:

```
if prediction == 1:    print("Heart disease indicated")else:    print("No heart disease indicated")
```

Prints the result.

# 4. Test your understanding

Let's see if you can answer these five viva-style questions.

Mini viva practice

0/5 answered

1\. Which library is used to create the Bayesian Network?

pandas

pgmpy

matplotlib

2\. What does a node represent in a Bayesian Network?

A variable

A Python loop

A dataset file

3\. What does model.fit() do?

Deletes missing data

Learns probability distributions

Prints the prediction

4\. What is evidence in this practical?

The patient's known medical information

The final prediction

The accuracy score

5\. What does argmax() do?

Finds the smallest value

Finds the largest value's position

Calculates the average

Check answers

My biggest advice: Don't memorise all the code as 13 independent steps. Remember what each of the five main operations does:

1. Prepare patient data.
2. Create the network.
3. Learn probabilities.
4. Give patient evidence and perform inference.
5. Select the most probable diagnosis.

If you understand those five operations, you can reconstruct most of the program even if you forget a line.