# Practical 6: Agricultural Genomics and Crop Improvement
### Identifying Genetic Markers Associated with Desirable Crop Traits

We'll follow the same learning format as Practical 5: **simple theory → small dummy CSV dataset → Python code → line-by-line explanation → expected output → viva questions.**

We'll use a small dataset of crop samples to investigate whether particular genetic markers are associated with **disease resistance**.

## 1. Aim

To analyze crop genomic data and identify genetic markers associated with a desirable trait, such as disease resistance, using Python.

## 2. Basic theory

### What is agricultural genomics?

Agricultural genomics is the study of the genetic information of crops to understand traits such as:

- Disease resistance
- High crop yield
- Drought tolerance
- Improved nutritional quality

### What is crop improvement?

Crop improvement is the process of developing crop varieties with desirable characteristics, such as higher yield or better resistance to diseases.

### What is a genetic marker?

A genetic marker is a measurable DNA feature that can be used to study differences between plants.

For this practical, we'll use three imaginary genetic markers:

- `Marker1`
- `Marker2`
- `Marker3`

Each marker will have a value of `0` or `1` in our dummy dataset.

### What is disease resistance?

Disease resistance is a plant's ability to prevent or limit damage caused by a disease-causing organism.

Our dataset will label each crop sample as either:

- `Resistant` — the sample is classified as resistant.
- `Susceptible` — the sample is classified as susceptible.

**Our goal:** Compare the marker values between the two groups and identify which marker is most strongly associated with resistance in this example.

This is a simplified demonstration using artificial data, not real crop research.

## 3. Create the dummy dataset in a CSV file

Open Notepad or Excel and copy the following data.

Save the file as **`crop_genomics.csv`**.

```csv
Sample,Marker1,Marker2,Marker3,Trait
C1,1,1,1,Resistant
C2,1,1,0,Resistant
C3,1,1,1,Resistant
C4,1,1,0,Resistant
C5,1,0,1,Resistant
C6,1,0,0,Resistant
C7,0,1,1,Susceptible
C8,0,1,0,Susceptible
C9,0,1,1,Susceptible
C10,0,0,0,Susceptible
C11,0,0,1,Susceptible
C12,0,0,0,Susceptible
```

### Understanding the dataset

| Column | Meaning |
|---|---|
| `Sample` | Crop sample identification |
| `Marker1` | First genetic marker |
| `Marker2` | Second genetic marker |
| `Marker3` | Third genetic marker |
| `Trait` | Disease-resistance classification |

We have **12 crop samples**:
- 6 resistant samples.
- 6 susceptible samples.

The marker values are artificial and used only to demonstrate the analysis.

---

## 4. Python program

This program compares the frequency of each marker in resistant and susceptible crops. It then identifies the marker with the largest difference between the two groups.

### Code cell

```python
# Practical 6: Agricultural Genomics and Crop Improvement

# Step 1: Import pandas
import pandas as pd

# Step 2: Load the crop genomic dataset
data = pd.read_csv("crop_genomics.csv")

print("Crop Genomic Dataset:")
print(data)

# Step 3: Select genetic markers
markers = ["Marker1", "Marker2", "Marker3"]

# Step 4: Calculate marker frequencies for each trait
frequency = data.groupby("Trait")[markers].mean()

print("\nMarker Frequencies:")
print(frequency)

# Step 5: Calculate the difference between the two groups
difference = frequency.loc["Resistant"] - frequency.loc["Susceptible"]

print("\nFrequency Difference:")
print(difference)

# Step 6: Identify the most strongly associated marker
best_marker = difference.abs().idxmax()

print("\nMarker most strongly associated with resistance:", best_marker)

print("\nCrop genomics analysis completed.")
```

---

## 5. Explanation of the code

### Step 1: Import pandas

```python
import pandas as pd
```

Pandas is a Python library used to read and analyze tabular data.

We use it to load our CSV file and compare genetic markers.

### Step 2: Load the dataset

```python
data = pd.read_csv("crop_genomics.csv")

print("Crop Genomic Dataset:")
print(data)
```

- `pd.read_csv()` reads the CSV file.
- `data` stores the dataset as a table.
- `print()` displays the data.

Make sure the CSV file is in the same working folder as your Python program. In Google Colab, upload the file before running the code.

### Step 3: Select genetic markers

```python
markers = ["Marker1", "Marker2", "Marker3"]
```

This creates a list containing the three genetic markers we want to analyze.

We don't include `Sample` because it is just an identification number. We also exclude `Trait` from the marker list because it tells us which group each sample belongs to.

### Step 4: Calculate marker frequencies

```python
frequency = data.groupby("Trait")[markers].mean()

print("\nMarker Frequencies:")
print(frequency)
```

This is the most important step.

Let's understand the command:

- `groupby("Trait")` divides the samples into resistant and susceptible groups.
- `[markers]` selects the three genetic markers.
- `.mean()` calculates the average value of each marker within each group.

Because the markers contain only `0` and `1`, their average tells us the **proportion of samples in which the marker value is 1**.

For example, if 4 out of 6 resistant samples have `Marker2 = 1`:

\[
\text{Marker frequency}=\frac{4}{6}=0.667
\]

This means approximately 66.7% of resistant samples have that marker value.

### Step 5: Calculate the difference

```python
difference = frequency.loc["Resistant"] - frequency.loc["Susceptible"]

print("\nFrequency Difference:")
print(difference)
```

This calculates:

**Difference = Frequency in resistant samples − Frequency in susceptible samples**

If a marker is much more common in resistant samples, the difference will be positive and close to `1`.

If it is much more common in susceptible samples, the difference will be negative and close to `-1`.

If its frequency is the same in both groups, the difference will be `0`.

### Step 6: Identify the strongest association

```python
best_marker = difference.abs().idxmax()

print("\nMarker most strongly associated with resistance:", best_marker)
```

Let's understand the two functions:

- `.abs()` calculates the absolute value of each difference, ignoring whether the difference is positive or negative.
- `.idxmax()` identifies the marker with the largest absolute difference.

The program then prints the marker with the strongest observed difference between the two groups.

**Important:** This identifies the strongest association in our dummy dataset. It does not prove that the marker causes disease resistance. Real research would require larger datasets, statistical testing, and validation in independent crop samples.

---

## 6. Expected output

The marker frequency table should look like this:

```text
Marker Frequencies:
              Marker1   Marker2   Marker3
Trait
Resistant         1.0  0.666667       0.5
Susceptible       0.0  0.500000       0.5
```

The frequency differences should be:

```text
Frequency Difference:
Marker1    1.000000
Marker2    0.166667
Marker3    0.000000
```

Finally, the program prints:

```text
Marker most strongly associated with resistance: Marker1

Crop genomics analysis completed.
```

### How do we interpret the result?

| Marker | Resistant frequency | Susceptible frequency | Difference |
|---|---:|---:|---:|
| Marker1 | 1.00 | 0.00 | 1.00 |
| Marker2 | 0.67 | 0.50 | 0.17 |
| Marker3 | 0.50 | 0.50 | 0.00 |

**Marker1 shows the strongest association with resistance in this dataset.**

All resistant samples have `Marker1 = 1`, while all susceptible samples have `Marker1 = 0`.

Marker2 shows only a small difference between the groups, and Marker3 has the same frequency in both groups.

---

## 7. Result

The crop genomic dataset was analyzed using Python and pandas. The frequencies of three genetic markers were compared between resistant and susceptible crop samples. Marker1 showed the largest difference in frequency and was identified as the marker most strongly associated with disease resistance in the dummy dataset.

---

## 8. Viva questions and answers

**Q1. What is the aim of this practical?**  
To identify genetic markers associated with disease resistance in crops.

**Q2. What is agricultural genomics?**  
It is the study of crop genomes to understand and improve desirable agricultural traits.

**Q3. What is crop improvement?**  
Crop improvement is the development of crop varieties with desirable traits, such as disease resistance and high yield.

**Q4. What is a genetic marker?**  
A genetic marker is a measurable DNA feature used to study genetic differences between organisms.

**Q5. What is disease resistance?**  
It is the ability of a plant to prevent or limit the effects of a disease.

**Q6. What is a desirable trait?**  
A characteristic that is beneficial for agriculture, such as high yield, disease resistance, or drought tolerance.

**Q7. Why do we use pandas?**  
Pandas helps us load, organize, and analyze tabular data.

**Q8. What does `groupby()` do?**  
It groups data according to a particular column, such as the disease-resistance trait.

**Q9. Why do we calculate the mean of genetic markers?**  
When markers are coded as 0 and 1, the mean gives the proportion of samples with a marker value of 1.

**Q10. Which marker was most strongly associated with resistance in this practical?**  
Marker1.

**Q11. Does association prove that a marker causes resistance?**  
No. Association alone does not prove causation. Further experiments and validation are needed.

**Q12. What is marker-assisted selection?**  
It is a crop-breeding approach that uses genetic markers associated with desirable traits to help select plants for breeding.

**Q13. How can genomics help improve crop yield?**  
It can help researchers identify genetic variants associated with yield and use that information in crop-breeding programs.

**Q14. Can this practical be used directly on real crop data?**  
The basic method can be adapted, but real data needs careful quality checks, appropriate statistical analysis, and validation.

---

## 9. Quick revision

Remember these five points:

1. **Aim:** Identify genetic markers associated with disease resistance.
2. **Input:** A CSV file containing crop samples, markers, and traits.
3. **Method:** Compare marker frequencies between resistant and susceptible groups.
4. **Key function:** `groupby().mean()`.
5. **Result:** Marker1 has the strongest association with resistance in our dummy dataset.

**One-line viva answer:** *Agricultural genomics uses genetic information to identify markers associated with desirable crop traits, helping researchers improve crops through informed breeding.*

**Practical 6 is complete.**