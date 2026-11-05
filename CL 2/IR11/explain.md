# Practical 11: Implementation of PageRank Algorithm Using Python

## 1. Aim
To implement the **PageRank algorithm using Python** to calculate the importance of web pages based on the links pointing to them.

## 2. Theory

**PageRank** is an algorithm originally developed by Google to rank web pages according to their importance.

The main idea is:
- A page linked to by many pages may be important.
- A link from an important page carries more value than a link from a less important page.
- PageRank repeatedly updates the scores until they become stable.

For example, suppose four web pages are connected as follows:

- **Page A** links to Pages B and C.
- **Page B** links to Page C.
- **Page C** links to Page A.
- **Page D** links to Page C.

We can represent these connections using a Python dictionary.

### PageRank formula

\[
PR(A)=\frac{1-d}{N}+d\sum_{T \rightarrow A}\frac{PR(T)}{L(T)}
\]

Where:
- **PR(A)** = PageRank score of page A.
- **d** = Damping factor, usually set to 0.85.
- **N** = Total number of pages.
- **PR(T)** = PageRank score of a page linking to A.
- **L(T)** = Number of outgoing links from page T.
- **\(T \rightarrow A\)** = A link from page T to page A.

The damping factor represents the probability that a user continues following links rather than jumping to a random page.

---

## 3. Python program

This program uses **basic Python only**—no Beautiful Soup or additional libraries are required. It also avoids list and dictionary comprehensions, as requested.

```python
# PageRank Algorithm

# Step 1: Define the web pages and their links
graph = {
    "A": ["B", "C"],
    "B": ["C"],
    "C": ["A"],
    "D": ["C"]
}

# Step 2: Get the list of pages
pages = list(graph.keys())

# Step 3: Find the total number of pages
N = len(pages)

# Step 4: Set the damping factor
damping = 0.85

# Step 5: Give every page an initial PageRank score
ranks = {}

for page in pages:
    ranks[page] = 1 / N

# Step 6: Calculate PageRank repeatedly
for iteration in range(100):

    # Initialize the new scores
    new_ranks = {}

    for page in pages:
        new_ranks[page] = (1 - damping) / N

    # Distribute each page's score through its outgoing links
    for source in pages:
        for target in graph[source]:
            new_ranks[target] = (
                new_ranks[target]
                + damping * ranks[source] / len(graph[source])
            )

    # Update the PageRank scores
    ranks = new_ranks

# Step 7: Display the final PageRank scores
print("PageRank Scores:")

for page in pages:
    print(page, ":", round(ranks[page], 4))

# Step 8: Display pages from highest to lowest rank
sorted_pages = sorted(
    ranks,
    key=ranks.get,
    reverse=True
)

print("\nPages ranked from highest to lowest:")

for page in sorted_pages:
    print(page, ":", round(ranks[page], 4))
```

---

## 4. Explanation of the code

### Step 1: Define the web pages and their links

```python
graph = {
    "A": ["B", "C"],
    "B": ["C"],
    "C": ["A"],
    "D": ["C"]
}
```

The dictionary represents the links between web pages.

- `"A": ["B", "C"]` means Page A links to Pages B and C.
- `"B": ["C"]` means Page B links to Page C.
- `"C": ["A"]` means Page C links to Page A.
- `"D": ["C"]` means Page D links to Page C.

The page names are dictionary keys, and their outgoing links are stored in lists.

### Step 2: Get the list of pages

```python
pages = list(graph.keys())
```

`graph.keys()` gives us the page names.

`list()` converts them into a list.

The result is:

```text
['A', 'B', 'C', 'D']
```

### Step 3: Count the pages

```python
N = len(pages)
```

`len()` counts the number of items in a list.

There are four pages, so:

```text
N = 4
```

### Step 4: Set the damping factor

```python
damping = 0.85
```

The damping factor is set to **0.85**, a commonly used value.

It represents an 85% probability of following links and a 15% probability of jumping to a randomly selected page in the simplified random-surfer model.

### Step 5: Initialize the PageRank scores

```python
ranks = {}

for page in pages:
    ranks[page] = 1 / N
```

First, `ranks = {}` creates an empty dictionary to store the scores.

The `for` loop visits each page and assigns it an initial score of:

\[
\frac{1}{N}=\frac{1}{4}=0.25
\]

Initially, all four pages have equal scores:

```text
A = 0.25
B = 0.25
C = 0.25
D = 0.25
```

### Step 6: Repeat the PageRank calculation

```python
for iteration in range(100):
```

This loop performs the calculation 100 times.

Repeated calculations help the scores approach stable values.

**Why do we need iterations?** The importance of one page depends on the importance of pages linking to it. Repeating the calculation allows this influence to spread through the network.

### Step 7: Initialize new scores

```python
new_ranks = {}

for page in pages:
    new_ranks[page] = (1 - damping) / N
```

We create a new dictionary to store the scores for the current iteration.

Each page starts with the base score:

\[
\frac{1-d}{N}
=
\frac{1-0.85}{4}
=
0.0375
\]

This represents the random-jump component of the PageRank formula.

### Step 8: Distribute scores through links

```python
for source in pages:
    for target in graph[source]:
        new_ranks[target] = (
            new_ranks[target]
            + damping * ranks[source] / len(graph[source])
        )
```

This is the **main part of the PageRank algorithm**.

The outer loop visits each source page. The inner loop visits the pages linked to by that source page.

Consider Page A:

```python
"A": ["B", "C"]
```

Page A has two outgoing links. Its current score is divided equally between B and C, and 85% of that score is distributed through those links.

The expression:

```python
damping * ranks[source] / len(graph[source])
```

means:

1. `ranks[source]` gets the source page's current score.
2. `len(graph[source])` counts its outgoing links.
3. Dividing distributes its score equally across those links.
4. Multiplying by `damping` applies the damping factor.

The calculated contribution is added to the target page's new score.

### Step 9: Update the scores

```python
ranks = new_ranks
```

Once all links have been processed, the newly calculated scores replace the old scores.

The next iteration uses these updated values.

### Step 10: Display the scores

```python
print("PageRank Scores:")

for page in pages:
    print(page, ":", round(ranks[page], 4))
```

This prints each page's score.

`round(ranks[page], 4)` rounds the score to four decimal places so it is easier to read.

### Step 11: Sort the pages

```python
sorted_pages = sorted(
    ranks,
    key=ranks.get,
    reverse=True
)
```

- `sorted()` sorts the page names.
- `key=ranks.get` tells Python to sort using each page's PageRank score.
- `reverse=True` sorts from highest to lowest.

The final loop prints the pages in descending order of importance.

**Note:** This code uses ordinary loops and function calls; it does not use list or dictionary comprehensions.

---

## 5. Expected output

```text
PageRank Scores:
A : 0.3725
B : 0.1958
C : 0.3941
D : 0.0375

Pages ranked from highest to lowest:
C : 0.3941
A : 0.3725
B : 0.1958
D : 0.0375
```

Your output should be approximately the same.

### How do we interpret the result?

| Page | PageRank score | Interpretation |
|---|---:|---|
| C | 0.3941 | Highest importance |
| A | 0.3725 | Second-highest importance |
| B | 0.1958 | Third-highest importance |
| D | 0.0375 | Lowest importance |

**Why does Page C have the highest score?**

Three pages—A, B, and D—link to Page C. Therefore, Page C receives contributions from several pages.

**Why does Page D have the lowest score?**

No other page links to Page D. It receives only the base score from the random-jump component.

---

## 6. Result

The PageRank algorithm was successfully implemented using Python. It calculates the importance of web pages based on their incoming links and the PageRank scores of the pages linking to them.

For the given network, **Page C has the highest PageRank score**, followed by Pages A, B, and D.

---

## 7. Viva questions and answers

### Q1. What is PageRank?
PageRank is an algorithm used to estimate the importance of web pages based on their link structure.

### Q2. Who developed PageRank?
PageRank was developed by **Larry Page and Sergey Brin** at Stanford University.

### Q3. What is the main purpose of PageRank?
Its purpose is to rank pages according to their importance within a network of links.

### Q4. What is a damping factor?
The damping factor represents the probability that a user follows a link instead of jumping to a random page. A commonly used value is **0.85**.

### Q5. What is an incoming link?
An incoming link is a link from another page to the current page.

### Q6. What is an outgoing link?
An outgoing link is a link from the current page to another page.

### Q7. Why are iterations required?
Iterations repeatedly update the PageRank scores until they approach stable values.

### Q8. Which data structure is used in this program?
A **Python dictionary** represents the web pages and their links. Lists store each page's outgoing links.

### Q9. Why is Page C ranked first in this example?
Because Pages A, B, and D all link to Page C, so it receives contributions from three pages.

### Q10. Is the page with the most incoming links always the most important?
Not necessarily. The importance of the linking pages also matters.

### Q11. What is the difference between PageRank and counting links?
Counting links treats all incoming links equally. PageRank considers both the number of incoming links and the importance of the pages providing them.

### Q12. What does `round(score, 4)` do?
It rounds the score to four decimal places.

### Q13. Why did we use `range(100)`?
It performs 100 iterations of the PageRank calculation.

### Q14. What is a limitation of this program?
It assumes every page has at least one outgoing link. A page with no outgoing links, called a *dangling node*, needs additional handling in a more general implementation.

---

## 8. Quick revision before your practical exam

Remember these five points:

1. **PageRank** calculates the importance of pages based on their links.
2. **Dictionary:** stores pages and their outgoing links.
3. **Damping factor:** commonly `0.85`.
4. **Iterations:** repeatedly update the scores.
5. **Highest PageRank score:** indicates the highest-ranked page in the network.

**The most important line to remember is:**

```python
new_ranks[target] = (
    new_ranks[target]
    + damping * ranks[source] / len(graph[source])
)
```

This line distributes a source page's score across its outgoing links and adds each contribution to the destination page's score.
You're right. The formula in the theory and the calculation in the code should match. Also, I'll write formulas in separate **Markdown-style or code cells**, not as LaTeX equations.

The formula I gave in the theory is correct, but the code implements it in parts. Here's the corrected explanation.

## 1. PageRank formula — Markdown cell

**Formula:**

```text
PR(A) = (1 - d) / N + d * SUM(PR(T) / L(T))
```

Where:
- `PR(A)` = PageRank score of the page being calculated.
- `d` = Damping factor, usually 0.85.
- `N` = Total number of pages.
- `PR(T)` = PageRank score of a page linking to page A.
- `L(T)` = Number of outgoing links from page T.
- `SUM` = Add the contributions from all pages linking to A.

## 2. Python code — Code cell

The following code implements exactly the formula shown above. It uses no list or dictionary comprehensions.

```python
# Step 1: Define the web pages and their links
graph = {
    "A": ["B", "C"],
    "B": ["C"],
    "C": ["A"],
    "D": ["C"]
}

# Step 2: Count the total number of pages
pages = list(graph.keys())
N = len(pages)

# Step 3: Set the damping factor
d = 0.85

# Step 4: Initialize PageRank scores equally
ranks = {}

for page in pages:
    ranks[page] = 1 / N

# Step 5: Repeat the calculation 100 times
for iteration in range(100):

    # Create a dictionary for the new scores
    new_ranks = {}

    for page in pages:
        new_ranks[page] = (1 - d) / N

    # Apply the PageRank formula
    for target in pages:

        # Find all pages linking to the target page
        for source in pages:

            if target in graph[source]:

                new_ranks[target] = (
                    new_ranks[target]
                    + d * ranks[source] / len(graph[source])
                )

    # Update the scores
    ranks = new_ranks

# Step 6: Display the PageRank scores
print("PageRank Scores:")

for page in pages:
    print(page, ":", round(ranks[page], 4))

# Step 7: Display pages in descending order
sorted_pages = sorted(
    ranks,
    key=ranks.get,
    reverse=True
)

print("\nPages ranked from highest to lowest:")

for page in sorted_pages:
    print(page, ":", round(ranks[page], 4))
```

## 3. How the formula matches the code

The formula is:

```text
PR(A) = (1 - d) / N + d * SUM(PR(T) / L(T))
```

The first part of the formula is implemented here:

```python
new_ranks[page] = (1 - d) / N
```

This calculates the base score for each page.

The second part is implemented here:

```python
if target in graph[source]:

    new_ranks[target] = (
        new_ranks[target]
        + d * ranks[source] / len(graph[source])
    )
```

This checks whether a source page links to the target page. If it does, the source page contributes a portion of its PageRank score to the target.

The nested loops add contributions from **all pages linking to the target**, which is what `SUM` means in the formula.

## 4. Expected output

```text
PageRank Scores:
A : 0.3725
B : 0.1958
C : 0.3941
D : 0.0375

Pages ranked from highest to lowest:
C : 0.3941
A : 0.3725
B : 0.1958
D : 0.0375
```

**Important:** The theory formula and the code now use the same calculation. The code simply breaks the formula into smaller steps so it is easier to understand.