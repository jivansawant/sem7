# Practical 10: Agglomerative Hierarchical Clustering
### Information Retrieval (IR) / Machine Learning

We'll use the same learning format as the previous practicals: **aim → theory → dataset → Python code → line-by-line explanation → expected output → viva questions.**

We'll create a small dataset of documents and group similar documents using Agglomerative Hierarchical Clustering. This is easy to understand and connects directly to Information Retrieval.

## 1. Aim

To implement the Agglomerative Hierarchical Clustering algorithm using Python to group similar documents based on their word frequencies.

## 2. Basic theory

### What is clustering?

Clustering is an unsupervised machine learning technique that groups similar data points together.

For example, a search system might group documents about farming together and documents about sports into another group.

**Unsupervised learning** means the algorithm finds patterns in data without being given the correct group labels in advance.

### What is hierarchical clustering?

Hierarchical clustering organizes data into a hierarchy of clusters. This hierarchy can be represented as a tree called a **dendrogram**.

There are two main types:

1. **Agglomerative clustering:** Starts with each data point as its own cluster and repeatedly merges the closest clusters.
2. **Divisive clustering:** Starts with all data points in one cluster and repeatedly splits them into smaller clusters.

In this practical, we'll use **Agglomerative Hierarchical Clustering**.

### How does agglomerative clustering work?

Suppose we have four documents:

- Document 1: about farming
- Document 2: about agriculture
- Document 3: about football
- Document 4: about cricket

The algorithm begins with four separate clusters:

```text
D1    D2    D3    D4
```

It repeatedly merges the most similar clusters until the required number of clusters is reached.

Ideally, the result groups the farming documents together and the sports documents together.

### What is TF-IDF?

**TF-IDF** stands for Term Frequency–Inverse Document Frequency.

It converts text into numerical features based on how important words are within documents and across the document collection.

These numerical features allow a clustering algorithm to compare documents.

---

## 3. Dataset

We'll use six short documents directly in the Python program. You don't need to create a separate CSV file for this practical.

| Document | Text |
|---|---|
| D1 | farmers grow crops in fields |
| D2 | farmers grow wheat and rice |
| D3 | crops need water and soil |
| D4 | football players score goals |
| D5 | cricket players hit the ball |
| D6 | football and cricket are sports |

Our goal is to group documents with similar content.

We expect the algorithm to identify a farming/agriculture group and a sports group. The exact clusters depend on the text representation and distance calculations.

## 4. Python program

We'll use:
- `TfidfVectorizer` to convert documents into numerical features.
- `AgglomerativeClustering` to group similar documents.

If you are using Google Colab or Jupyter Notebook, install the required library if necessary:

```python
!pip install scikit-learn
```

### Code cell

```python
# Practical 10: Agglomerative Hierarchical Clustering

# Step 1: Import libraries
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.cluster import AgglomerativeClustering

# Step 2: Create sample documents
documents = [
    "farmers grow crops in fields",
    "farmers grow wheat and rice",
    "crops need water and soil",
    "football players score goals",
    "cricket players hit the ball",
    "football and cricket are sports"
]

# Step 3: Convert documents into numerical features
vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(documents).toarray()

# Step 4: Apply Agglomerative Hierarchical Clustering
model = AgglomerativeClustering(
    n_clusters=2,
    metric="cosine",
    linkage="average"
)

clusters = model.fit_predict(X)

# Step 5: Display the results
print("Document Clustering Results:\n")

for i, cluster in enumerate(clusters):
    print("Document", i + 1, ":", documents[i])
    print("Cluster:", cluster)
    print()

print("Agglomerative clustering completed.")
```

---

## 5. Explanation of the code

### Step 1: Import libraries

```python
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.cluster import AgglomerativeClustering
```

We import two tools from the `scikit-learn` library.

- `TfidfVectorizer` converts text into numerical features.
- `AgglomerativeClustering` performs hierarchical clustering by merging similar groups.

### Step 2: Create sample documents

```python
documents = [
    "farmers grow crops in fields",
    "farmers grow wheat and rice",
    "crops need water and soil",
    "football players score goals",
    "cricket players hit the ball",
    "football and cricket are sports"
]
```

We store six documents in a Python list.

The first three discuss farming or agriculture, while the last three discuss sports.

These documents are our input data.

### Step 3: Convert text into numerical features

```python
vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(documents).toarray()
```

Computers cannot directly calculate distances between sentences in their original text form. We therefore convert the documents into numbers.

**What does TF-IDF do?**

TF-IDF assigns numerical weights to words based on their occurrence in a document and across the document collection.

For example, the words `farmers` and `crops` help represent agricultural documents, while `football` and `cricket` help represent sports documents.

Let's understand the functions:

- `TfidfVectorizer()` creates the text-processing tool.
- `fit_transform(documents)` learns the vocabulary and converts each document into a numerical vector.
- `toarray()` converts the result into a regular numerical array.

The resulting `X` contains the numerical representation of all six documents.

### Step 4: Apply Agglomerative Clustering

```python
model = AgglomerativeClustering(
    n_clusters=2,
    metric="cosine",
    linkage="average"
)
```

This creates our clustering model.

**`n_clusters=2`**

We ask the algorithm to divide the documents into two clusters.

**`metric="cosine"`**

Cosine distance compares the direction of two numerical vectors. It is useful for comparing text documents represented by TF-IDF vectors.

**`linkage="average"`**

Average linkage calculates the average distance between the members of two clusters when deciding which clusters to merge.

Now, we run the algorithm:

```python
clusters = model.fit_predict(X)
```

- `fit_predict()` analyzes the numerical data.
- It groups similar documents together.
- It returns a cluster label for every document.

The labels `0` and `1` are just group identifiers. Cluster 0 is not inherently better or more important than Cluster 1.

### Step 5: Display the results

```python
for i, cluster in enumerate(clusters):
    print("Document", i + 1, ":", documents[i])
    print("Cluster:", cluster)
    print()
```

The `for` loop processes each document.

- `enumerate(clusters)` provides the position and cluster label.
- `i + 1` displays document numbering starting from 1.
- `documents[i]` displays the original text.
- `cluster` displays the assigned cluster label.

Finally:

```python
print("Agglomerative clustering completed.")
```

This prints a message indicating that the program has finished.

---

## 6. Expected output

The output will look similar to this:

```text
Document Clustering Results:

Document 1 : farmers grow crops in fields
Cluster: 0

Document 2 : farmers grow wheat and rice
Cluster: 0

Document 3 : crops need water and soil
Cluster: 0

Document 4 : football players score goals
Cluster: 1

Document 5 : cricket players hit the ball
Cluster: 1

Document 6 : football and cricket are sports
Cluster: 1

Agglomerative clustering completed.
```

**Note:** Cluster numbers can be reversed, and the exact grouping depends on the calculated distances. The output above illustrates the intended grouping.

### How do we interpret the result?

- **Agricultural cluster:** Documents about farmers, crops, wheat, rice, water, and soil.
- **Sports cluster:** Documents about football, cricket, players, and sports.

The purpose is to group documents with similar word patterns without manually assigning categories to them beforehand.

---

## 7. Result

Agglomerative Hierarchical Clustering was implemented using Python and scikit-learn. The text documents were converted into numerical TF-IDF vectors, and the clustering algorithm grouped the documents based on cosine distance.

---

## 8. Viva questions and answers

**Q1. What is clustering?**  
Clustering is an unsupervised learning technique that groups similar data points together.

**Q2. What is hierarchical clustering?**  
It is a clustering technique that organizes data into a hierarchy of clusters.

**Q3. What is Agglomerative Hierarchical Clustering?**  
It is a bottom-up clustering algorithm that starts with individual data points and repeatedly merges the closest clusters.

**Q4. What are the two types of hierarchical clustering?**  
Agglomerative clustering and divisive clustering.

**Q5. What is the difference between agglomerative and divisive clustering?**  
Agglomerative clustering merges smaller clusters, while divisive clustering splits larger clusters.

**Q6. What is TF-IDF?**  
TF-IDF is a technique that converts text into numerical features based on word importance.

**Q7. Why do we use TF-IDF in this practical?**  
It converts documents into numerical vectors so their similarity can be calculated.

**Q8. What is cosine distance?**  
Cosine distance measures how different the directions of two vectors are.

**Q9. What is linkage?**  
Linkage defines how the distance between two clusters is calculated.

**Q10. What is average linkage?**  
It calculates the average distance between pairs of data points from two clusters.

**Q11. What does `n_clusters=2` mean?**  
It instructs the algorithm to produce two clusters.

**Q12. Is clustering supervised or unsupervised learning?**  
Clustering is an unsupervised learning technique because it does not require predefined class labels.

**Q13. What is a dendrogram?**  
A dendrogram is a tree-like diagram that shows how clusters are merged or divided in hierarchical clustering.

**Q14. Where is clustering used in Information Retrieval?**  
It can group similar documents, organize search results, and help discover topics within a document collection.

**Q15. What is the purpose of `fit_predict()`?**  
It fits the clustering model to the input data and returns the cluster label for each data point.

---

## 9. Quick revision

Remember these five points:

1. **Aim:** Group similar documents.
2. **Algorithm:** Agglomerative Hierarchical Clustering.
3. **Text representation:** TF-IDF.
4. **Distance metric:** Cosine distance.
5. **Output:** Two clusters of documents.

**One-line viva answer:** Agglomerative Hierarchical Clustering is an unsupervised algorithm that starts with individual data points and repeatedly merges the closest clusters until the desired number of clusters is reached.

✅ **Practical 10 is complete.**

Next is **Practical 11: PageRank Algorithm**.