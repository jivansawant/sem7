# Practical 5: Machine Learning for Genomic Data
### Using the Random Forest Algorithm

## 1. Aim

To apply the Random Forest machine learning algorithm to classify genomic samples based on selected genetic markers.

## 2. Basic theory

**What is genomic data?**  
Genomic data contains information about an organism's DNA, genes, or genetic markers.

**What is a genetic marker?**  
A genetic marker is a measurable feature of DNA that can help distinguish between samples. For this demonstration, we'll represent markers using simple numeric values, `0` and `1`.

**What is machine learning?**  
Machine learning allows a computer to learn patterns from example data and use those patterns to make predictions about new data.

**What is Random Forest?**  
Random Forest is a machine learning algorithm that combines multiple decision trees to make a prediction. For classification, the trees vote for a class, and the most-voted class becomes the final prediction.

**What will our practical do?**
1. Create a small dummy genomic dataset.
2. Save it as a CSV file.
3. Load the data into Python.
4. Separate genetic markers from the class labels.
5. Train a Random Forest model.
6. Predict the class of a new sample.
7. Measure the model's accuracy.

**Note:** This is an educational dummy dataset, not real biological data. The classes are illustrative labels, not actual disease diagnoses or biological conclusions.

## 3. Create the dummy dataset in a CSV file

Open **Notepad or Excel** and copy the following data. Save the file as **`genomic_data.csv`**.

Make sure you save it as a `.csv` file, not a `.txt` file.

### Dataset: `genomic_data.csv`

```csv
Marker1,Marker2,Marker3,Marker4,Class
0,0,1,0,Type_A
0,0,0,1,Type_A
0,0,1,1,Type_A
0,0,0,0,Type_A
0,0,1,0,Type_A
0,0,0,1,Type_A
0,0,1,1,Type_A
0,0,0,0,Type_A
0,0,1,0,Type_A
0,0,0,1,Type_A
1,1,0,1,Type_B
1,1,1,0,Type_B
1,1,0,0,Type_B
1,1,1,1,Type_B
1,1,0,1,Type_B
1,1,1,0,Type_B
1,1,0,0,Type_B
1,1,1,1,Type_B
1,1,0,1,Type_B
1,1,1,0,Type_B
```

### Understanding the dataset

| Column | Meaning |
|---|---|
| `Marker1` | First genetic marker |
| `Marker2` | Second genetic marker |
| `Marker3` | Third genetic marker |
| `Marker4` | Fourth genetic marker |
| `Class` | Category the model must predict |

- `0` and `1` represent two possible marker values in our dummy dataset.
- `Type_A` and `Type_B` are the classes we want the model to learn.
- The dataset contains **20 samples**: 10 in each class.

Notice that `Marker1` and `Marker2` help distinguish the two classes in this example. The Random Forest model will try to learn this pattern.

---

## 4. Python program

You can run this program in **Google Colab, Jupyter Notebook, or another Python environment**.

If you use Google Colab, upload `genomic_data.csv` to your session before running the code.

### Code cell

```python
# Practical 5: Machine Learning for Genomic Data
# Algorithm: Random Forest

# Step 1: Import libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Step 2: Load the dataset
data = pd.read_csv("genomic_data.csv")

print("Genomic Dataset:")
print(data)

# Step 3: Separate features and target
X = data[["Marker1", "Marker2", "Marker3", "Marker4"]]
y = data["Class"]

# Step 4: Split data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)

# Step 5: Create the Random Forest model
model = RandomForestClassifier(n_estimators=100, random_state=42)

# Step 6: Train the model
model.fit(X_train, y_train)

# Step 7: Predict classes for test data
y_pred = model.predict(X_test)

# Step 8: Calculate accuracy
accuracy = accuracy_score(y_test, y_pred)
print("\nModel Accuracy:", round(accuracy * 100, 2), "%")

# Step 9: Predict the class of a new sample
new_sample = pd.DataFrame({
    "Marker1": [1],
    "Marker2": [1],
    "Marker3": [0],
    "Marker4": [1]
})

prediction = model.predict(new_sample)
print("Predicted Class:", prediction[0])
```

---

## 5. Explanation of the code

Let's understand each step from the beginning.

### Step 1: Import libraries

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
```

**What is a library?**  
A library is a collection of ready-made functions and tools that we can use in our program.

- `pandas`: Used to read and work with tabular data, such as CSV files.
- `train_test_split`: Divides the dataset into training and testing sets.
- `RandomForestClassifier`: Implements the Random Forest classification algorithm.
- `accuracy_score`: Calculates the proportion of correct predictions.

The `as pd` part gives pandas a short name, `pd`, so we can write `pd.read_csv()`.

### Step 2: Load the dataset

```python
data = pd.read_csv("genomic_data.csv")
print("Genomic Dataset:")
print(data)
```

- `pd.read_csv()` reads the CSV file.
- `data` stores the dataset in a pandas DataFrame, which is like a table.
- `print()` displays the dataset.

**Important:** The CSV file must be accessible from the folder or working directory in which you run the program.

### Step 3: Separate features and target

```python
X = data[["Marker1", "Marker2", "Marker3", "Marker4"]]
y = data["Class"]
```

This is an important machine learning concept.

- **Features (`X`):** The input information used by the model. Here, these are the four genetic markers.
- **Target (`y`):** The answer the model must learn to predict. Here, it is `Type_A` or `Type_B`.

Think of it this way: we give the model the marker values and ask it to predict the class.

### Step 4: Split the dataset

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)
```

We divide our data into two parts:

- **Training data:** Used to teach the model.
- **Testing data:** Used to check how well the trained model predicts samples it did not train on.

Here, `test_size=0.25` means 25% of the data is reserved for testing.

For 20 samples:
- Training set: 15 samples.
- Testing set: 5 samples.

`random_state=42` makes the random split reproducible, while `stratify=y` helps preserve the class proportions in both sets.

### Step 5: Create the Random Forest model

```python
model = RandomForestClassifier(n_estimators=100, random_state=42)
```

This creates our classifier.

- `RandomForestClassifier`: The Random Forest algorithm for predicting categories.
- `n_estimators=100`: The forest contains 100 decision trees.
- `random_state=42`: Helps make the model's random choices reproducible.

**What is a decision tree?**

A decision tree makes predictions by asking a sequence of questions about the input features.

For example:
- Is `Marker1` equal to 0?
- Is `Marker2` equal to 0?
- Which class best matches these values?

A Random Forest combines predictions from many decision trees rather than relying on just one tree.

### Step 6: Train the model

```python
model.fit(X_train, y_train)
```

The `fit()` method trains the model using the training data.

It learns patterns connecting the marker values (`X_train`) with the known classes (`y_train`).

### Step 7: Predict the test classes

```python
y_pred = model.predict(X_test)
```

The `predict()` method uses the trained model to predict the classes of the test samples.

- `X_test` contains the marker values.
- `y_pred` contains the predicted classes.

### Step 8: Calculate accuracy

```python
accuracy = accuracy_score(y_test, y_pred)
print("\nModel Accuracy:", round(accuracy * 100, 2), "%")
```

Accuracy measures the proportion of test samples classified correctly.

\[
\text{Accuracy} =
\frac{\text{Number of correct predictions}}
{\text{Total number of predictions}}
\times 100
\]

For example, if the model correctly predicts all 5 test samples:

\[
\text{Accuracy} = \frac{5}{5}\times100 = 100\%
\]

The exact accuracy depends on the trained model's predictions. With this deliberately simple dataset, the model should achieve 100% accuracy because the two classes are clearly separated by the marker values.

**Remember:** High accuracy on a tiny, artificial dataset does not mean the model would perform equally well on real genomic data.

### Step 9: Predict a new sample

```python
new_sample = pd.DataFrame({
    "Marker1": [1],
    "Marker2": [1],
    "Marker3": [0],
    "Marker4": [1]
})

prediction = model.predict(new_sample)
print("Predicted Class:", prediction[0])
```

Here, we provide a new sample with four marker values.

The model uses the patterns it learned during training to predict its class.

The square brackets around the values, such as `[1]`, indicate that each feature is being supplied as a one-row column in a DataFrame.

The predicted class should be:

```text
Predicted Class: Type_B
```

---

## 6. Expected output

The program first prints all 20 rows of the dataset. The final lines should look like this:

```text
Model Accuracy: 100.0 %
Predicted Class: Type_B
```

The accuracy shown here is the expected result for this simple, clearly separated dataset.

---

## 7. Result

The Random Forest classification algorithm was applied to a dummy genomic dataset containing four genetic markers. The dataset was divided into training and testing sets, the model was trained, and its classification accuracy was evaluated. Finally, the trained model was used to predict the class of a new sample.

---

## 8. Viva questions and answers

**Q1. What is the aim of this practical?**  
To classify genomic samples using the Random Forest machine learning algorithm.

**Q2. What is machine learning?**  
Machine learning enables computers to learn patterns from data and use them to make predictions.

**Q3. What is genomic data?**  
Genomic data contains information about an organism's DNA, genes, or genetic markers.

**Q4. What is a genetic marker?**  
A genetic marker is a measurable genetic feature that can help distinguish between samples.

**Q5. What is Random Forest?**  
Random Forest is a machine learning algorithm that combines multiple decision trees to make predictions.

**Q6. Why is it called Random Forest?**  
Because it combines a collection, or forest, of decision trees built using randomized sampling and feature selection.

**Q7. What is classification?**  
Classification is a machine learning task that assigns data to predefined categories.

**Q8. What are features and targets?**  
Features are the input variables used to make predictions. The target is the class or value being predicted.

**Q9. Why do we split data into training and testing sets?**  
Training data teaches the model, while testing data evaluates its performance on samples not used for training.

**Q10. What does `n_estimators=100` mean?**  
It specifies that the Random Forest contains 100 decision trees.

**Q11. What is accuracy?**  
Accuracy is the percentage of predictions that match the correct labels.

**Q12. What is the purpose of pandas?**  
Pandas is a Python library used to load, organize, and analyze tabular data.

**Q13. What is the purpose of `model.fit()`?**  
It trains the model using the training features and their known classes.

**Q14. What is the purpose of `model.predict()`?**  
It uses the trained model to predict the classes of new samples.

**Q15. Can this model diagnose a genetic disease?**  
No. This is a demonstration using dummy data. Real-world diagnosis would require suitable biological data, rigorous validation, and appropriate clinical evaluation.

---

## 9. Quick revision before the viva

Remember these five points:

1. **Algorithm:** Random Forest.
2. **Input:** Four genetic markers.
3. **Output:** `Type_A` or `Type_B`.
4. **Training:** `model.fit()`.
5. **Prediction:** `model.predict()`.

**The entire practical in one sentence:** We use genetic markers as input features, train a Random Forest classifier on labeled samples, evaluate its accuracy, and predict the class of a new sample.

When you're comfortable with this practical, we can move to Practical 6: Agricultural Genomics and Crop Improvement.