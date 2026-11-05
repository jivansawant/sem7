# Practical 7: Text Preprocessing — Stopword Removal and Stemming
### Information Retrieval (IR)

We'll start the Information Retrieval (IR) practicals now, using the same beginner-friendly format: **aim → theory → small example → simple Python code → line-by-line explanation → output → viva questions.**

## 1. Aim

To preprocess text by removing stopwords and applying stemming to improve text processing in an Information Retrieval system.

## 2. Basic theory

### What is Information Retrieval (IR)?

**Information Retrieval** is the process of finding relevant information from a collection of documents based on a user's query.

For example, when you search for *"best farming methods"* on Google, the search engine tries to find documents relevant to your query.

Before searching, text is often cleaned and processed. This is called **text preprocessing**.

### What is text preprocessing?

Text preprocessing is the process of cleaning and preparing text before analyzing or searching it.

Common steps include:
1. Converting text to lowercase.
2. Removing punctuation.
3. Removing stopwords.
4. Applying stemming.

### What are stopwords?

Stopwords are common words that are often removed because they contribute relatively little to a particular search task.

Examples include:
- `is`
- `the`
- `a`
- `an`
- `and`
- `in`

Example:

**Original sentence:**
> The farmer is working in the field.

**After stopword removal:**
> farmer working field

### What is stemming?

Stemming reduces words to a simpler root-like form by removing word endings.

Examples:

| Original word | Stem |
|---|---|
| playing | play |
| played | play |
| farming | farm |
| studies | studi |

Notice that `studi` is not a complete English word. Stemming uses simple rules and does not always produce a dictionary word.

---

## 3. Python program

We'll use Python's **NLTK library**, which provides tools for text processing.

### Step 1: Install the library

If you're using Google Colab or Jupyter Notebook, run this in a separate code cell:

```python
!pip install nltk
```

You only need to install it once in your environment.

### Step 2: Main program

```python
# Practical 7: Text Preprocessing
# Stopword Removal and Stemming

import nltk
import string
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer

# Download stopwords
nltk.download("stopwords")

# Sample text
text = "The farmers are playing and working in the fields."

print("Original Text:", text)

# Convert text to lowercase
text = text.lower()

# Remove punctuation
text = text.translate(str.maketrans("", "", string.punctuation))

# Split text into words
words = text.split()

# Remove stopwords
stop_words = set(stopwords.words("english"))
filtered_words = [word for word in words if word not in stop_words]

print("\nAfter Stopword Removal:", filtered_words)

# Apply stemming
stemmer = PorterStemmer()
stemmed_words = [stemmer.stem(word) for word in filtered_words]

print("\nAfter Stemming:", stemmed_words)

print("\nText preprocessing completed.")
```

---

## 4. Explanation of the code

### Step 1: Import libraries

```python
import nltk
import string
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer
```

- `nltk`: A Python library for natural language processing.
- `string`: Provides common string-related tools, including punctuation characters.
- `stopwords`: Provides lists of common words that can be removed.
- `PorterStemmer`: Applies stemming rules to words.

### Step 2: Download stopwords

```python
nltk.download("stopwords")
```

This downloads the English stopword list required by the program.

### Step 3: Define the text

```python
text = "The farmers are playing and working in the fields."
print("Original Text:", text)
```

We create a sample sentence and display it.

### Step 4: Convert to lowercase

```python
text = text.lower()
```

This converts uppercase letters to lowercase.

For example:

`The Farmer` → `the farmer`

This helps treat uppercase and lowercase versions of a word consistently.

### Step 5: Remove punctuation

```python
text = text.translate(str.maketrans("", "", string.punctuation))
```

This removes punctuation marks such as commas, full stops, and question marks.

For example:

`hello, world!` → `hello world`

### Step 6: Split the text into words

```python
words = text.split()
```

The `split()` function separates the sentence into individual words.

Example:

```text
"the farmer works"
```

becomes:

```python
["the", "farmer", "works"]
```

This list is called a list of tokens, and the process is known as **tokenization**.

### Step 7: Remove stopwords

```python
stop_words = set(stopwords.words("english"))
filtered_words = [word for word in words if word not in stop_words]
```

- `stopwords.words("english")` gets a list of common English stopwords.
- `set()` stores them in a set, making membership checks efficient.
- The list comprehension keeps only words that are **not** stopwords.

For our sentence, words such as `the`, `are`, `and`, and `in` are removed.

### Step 8: Apply stemming

```python
stemmer = PorterStemmer()
stemmed_words = [stemmer.stem(word) for word in filtered_words]
```

- `PorterStemmer()` creates a stemming tool.
- `stemmer.stem(word)` reduces each word to its stem.

For example:
- `farmers` → `farmer`
- `playing` → `play`
- `working` → `work`
- `fields` → `field`

### Step 9: Display the result

```python
print("\nAfter Stemming:", stemmed_words)
print("\nText preprocessing completed.")
```

These statements display the processed words and confirm that the program has finished.

---

## 5. Expected output

Your output should look like this:

```text
Original Text: The farmers are playing and working in the fields.

After Stopword Removal: ['farmers', 'playing', 'working', 'fields']

After Stemming: ['farmer', 'play', 'work', 'field']

Text preprocessing completed.
```

---

## 6. Result

Text preprocessing was performed using Python and NLTK. The input text was converted to lowercase, punctuation was removed, stopwords were eliminated, and stemming was applied to the remaining words.

---

## 7. Viva questions and answers

**Q1. What is Information Retrieval?**  
Information Retrieval is the process of finding relevant information from a collection of documents.

**Q2. What is text preprocessing?**  
It is the process of cleaning and preparing text before analysis or retrieval.

**Q3. What are stopwords?**  
Stopwords are common words, such as *the*, *is*, and *and*, that are often removed during text preprocessing.

**Q4. What is stemming?**  
Stemming reduces words to root-like forms by removing word endings according to simple rules.

**Q5. What is tokenization?**  
Tokenization is the process of splitting text into smaller units, such as words.

**Q6. Why do we convert text to lowercase?**  
To treat uppercase and lowercase versions of the same word consistently.

**Q7. Which library is used in this practical?**  
NLTK, which stands for Natural Language Toolkit.

**Q8. What does `split()` do?**  
It splits a string into a list of words, using whitespace by default.

**Q9. What is PorterStemmer?**  
It is an NLTK tool that applies the Porter stemming algorithm to words.

**Q10. Does stemming always produce a meaningful English word?**  
No. Stemming can produce root-like forms that are not complete dictionary words.

**Q11. What is the difference between stemming and stopword removal?**  
Stopword removal deletes selected common words, while stemming reduces words to their stem forms.

**Q12. Why is text preprocessing useful in IR?**  
It can reduce unnecessary variation in text and help a retrieval system match relevant documents more effectively.

---

## 8. Quick revision

Remember these five points:

1. **Aim:** Clean text for information retrieval.
2. **Library:** NLTK.
3. **Stopword removal:** Removes selected common words.
4. **Stemming:** Reduces words to root-like forms.
5. **Tokenization:** Splits text into individual words.

**One-line viva answer:** Text preprocessing improves text consistency by performing operations such as lowercasing, punctuation removal, stopword removal, and stemming.

**Practical 7 is complete.** Next is **Practical 8: Inverted Index and Document Retrieval**.