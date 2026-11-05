# Practical 8: Inverted Index and Document Retrieval
### Information Retrieval (IR)

We'll use the same learning format as Practical 7: **aim → theory → simple Python code → line-by-line explanation → expected output → viva questions.**

## 1. Aim

To create an inverted index for a collection of documents and retrieve documents containing a user-specified search term.

## 2. Basic theory

### What is document retrieval?

Document retrieval is the process of finding documents relevant to a user's search query.

For example, suppose we have three documents:

- Document 1: `apple banana mango`
- Document 2: `banana orange`
- Document 3: `apple orange mango`

If a user searches for `apple`, the system should return Document 1 and Document 3.

### What is an inverted index?

An **inverted index** is a data structure that maps each word to the documents in which it appears.

Instead of checking every document separately for every search, we can use the index to find matching documents quickly.

Example:

| Word | Document IDs |
|---|---|
| apple | Doc1, Doc3 |
| banana | Doc1, Doc2 |
| mango | Doc1, Doc3 |
| orange | Doc2, Doc3 |

This table is an inverted index.

### Why is it useful?

Inverted indexes are used by search engines and document retrieval systems because they help find documents containing specific terms efficiently.

---

## 3. Python program

We'll create three small documents, build an inverted index, and search for a word entered by the user.

### Code

```python
# Practical 8: Inverted Index and Document Retrieval

# Step 1: Create sample documents
documents = {
    1: "apple banana mango",
    2: "banana orange",
    3: "apple orange mango"
}

# Step 2: Create an empty inverted index
inverted_index = {}

# Step 3: Build the inverted index
for doc_id, text in documents.items():
    words = text.lower().split()

    for word in words:
        if word not in inverted_index:
            inverted_index[word] = []

        if doc_id not in inverted_index[word]:
            inverted_index[word].append(doc_id)

# Step 4: Display the inverted index
print("Inverted Index:")
for word, doc_ids in inverted_index.items():
    print(word, ":", doc_ids)

# Step 5: Search for a word
query = input("\nEnter a search word: ").lower()

# Step 6: Retrieve matching documents
if query in inverted_index:
    print("Matching Documents:", inverted_index[query])
else:
    print("No matching documents found.")
```

---

## 4. Explanation of the code

### Step 1: Create sample documents

```python
documents = {
    1: "apple banana mango",
    2: "banana orange",
    3: "apple orange mango"
}
```

We create a dictionary containing three documents.

- `1`, `2`, and `3` are document IDs.
- The text after each ID is the document content.

A **dictionary** stores information as key-value pairs.

For example, `1` is the key and `"apple banana mango"` is its value.

### Step 2: Create an empty inverted index

```python
inverted_index = {}
```

We create an empty dictionary that will store each word and its list of document IDs.

Initially, the index contains nothing.

### Step 3: Build the inverted index

```python
for doc_id, text in documents.items():
    words = text.lower().split()
```

Let's break this down:

- `documents.items()` provides each document ID and its text.
- `for` processes each document one at a time.
- `text.lower()` converts the text to lowercase.
- `split()` divides the text into individual words.

For Document 1:

```python
["apple", "banana", "mango"]
```

The program processes these words individually.

Next:

```python
for word in words:
    if word not in inverted_index:
        inverted_index[word] = []

    if doc_id not in inverted_index[word]:
        inverted_index[word].append(doc_id)
```

Here's what happens:

1. The program checks whether the word already exists in the index.
2. If not, it creates an empty list for that word.
3. It checks whether the current document ID is already in that word's list.
4. If not, it adds the document ID.

For example, when processing Document 1, the index begins to look like this:

```python
{
    "apple": [1],
    "banana": [1],
    "mango": [1]
}
```

When Document 2 is processed, `banana` already exists, so document ID `2` is added to its list.

After all documents have been processed, the index is complete.

### Step 4: Display the index

```python
print("Inverted Index:")
for word, doc_ids in inverted_index.items():
    print(word, ":", doc_ids)
```

- `.items()` provides each word and its associated document IDs.
- `print()` displays them.

This lets us see which documents contain each word.

### Step 5: Accept a search query

```python
query = input("\nEnter a search word: ").lower()
```

- `input()` asks the user to enter a word.
- `.lower()` converts it to lowercase.

For example, entering `APPLE` produces the query `apple`.

### Step 6: Retrieve matching documents

```python
if query in inverted_index:
    print("Matching Documents:", inverted_index[query])
else:
    print("No matching documents found.")
```

The program checks whether the search word exists in the inverted index.

- If the word exists, it prints the associated document IDs.
- If the word does not exist, it displays a message saying no documents were found.

**This is the retrieval step:** the system uses the inverted index to find the matching documents.

---

## 5. Expected output

When you run the program, the inverted index will look like this:

```text
Inverted Index:
apple : [1, 3]
banana : [1, 2]
mango : [1, 3]
orange : [2, 3]
```

The order of the words may differ, but the index contents should be the same.

### Example 1: Search for `apple`

```text
Enter a search word: apple
Matching Documents: [1, 3]
```

This means `apple` appears in Documents 1 and 3.

### Example 2: Search for `banana`

```text
Enter a search word: banana
Matching Documents: [1, 2]
```

This means `banana` appears in Documents 1 and 2.

### Example 3: Search for `grapes`

```text
Enter a search word: grapes
No matching documents found.
```

This means `grapes` does not appear in any of the documents.

---

## 6. Result

An inverted index was created for a collection of three documents using Python. The index maps each word to the document IDs in which it appears. A search query was then used to retrieve matching documents efficiently.

---

## 7. Viva questions and answers

**Q1. What is Information Retrieval?**  
Information Retrieval is the process of finding relevant information from a collection of documents.

**Q2. What is an inverted index?**  
An inverted index maps each word to the documents containing that word.

**Q3. Why is an inverted index used?**  
It helps retrieve documents containing a search term without scanning every document for each query.

**Q4. What is a document ID?**  
A document ID is a unique identifier assigned to a document.

**Q5. What is a dictionary in Python?**  
A dictionary stores data as key-value pairs.

**Q6. What is a list in Python?**  
A list is an ordered collection of items.

**Q7. What does `split()` do?**  
It divides a string into a list of words using whitespace by default.

**Q8. What does `lower()` do?**  
It converts uppercase letters to lowercase.

**Q9. What does `append()` do?**  
It adds an item to the end of a list.

**Q10. What is a search query?**  
A search query is the word or phrase a user enters to find information.

**Q11. What happens when a query is not in the inverted index?**  
The program displays a message indicating that no matching documents were found.

**Q12. Where are inverted indexes used?**  
They are commonly used in search engines, digital libraries, and document retrieval systems.

**Q13. What is the difference between a normal index and an inverted index?**  
A normal index may map documents to their contents or terms, while an inverted index maps terms to the documents containing them.

**Q14. Does this program retrieve documents based on meaning?**  
No. It retrieves documents based on exact word matching, ignoring letter case.

---

## 8. Quick revision

Remember these five points:

1. **Aim:** Create an inverted index and retrieve documents.
2. **Input:** A collection of text documents.
3. **Data structure:** Python dictionary.
4. **Inverted index:** Maps words to document IDs.
5. **Retrieval:** Finds document IDs associated with the search word.

**One-line viva answer:** An inverted index is a data structure that maps words to the documents containing them, making keyword-based document retrieval faster.

✅ **Practical 8 is complete.**

Next is **Practical 9: Bayesian Network for Medical Data / Heart Disease Prediction**.