Let's simplify this practical in the same style as the previous ones, using basic Python without complicated libraries.

There is one important issue with your friend's code: it does not actually predict a 3D protein structure. It performs a simple sequence comparison and makes a rough secondary-structure guess.

For your assignment, we can create a beginner-friendly educational version that demonstrates:

1. Protein sequence validation.
2. Comparison with a template protein sequence.
3. Sequence identity calculation.
4. Simple secondary-structure estimation.
5. A basic modeling-confidence estimate.

Important: This is a simplified demonstration of the homology-modeling concept, not a real 3D homology model. Real structure prediction requires a suitable known template structure and structural modeling software.

## Practical: Protein Structure Prediction

### 1. Aim

To analyze a given protein sequence, compare it with a template sequence, calculate sequence identity, and estimate its secondary structure using Python.

### 2. Theory

Protein structure prediction is the process of estimating the three-dimensional structure of a protein from its amino acid sequence.

A protein consists of amino acids represented by single-letter codes.

Homology modeling predicts the structure of a target protein using the known structure of a similar protein called a template.

Important terms:

- Target sequence: The protein sequence whose structure we want to predict.
- Template sequence: A similar protein sequence with a known structure.
- Sequence identity: The percentage of matching amino acids between two aligned sequences.
- Secondary structure: Local structural arrangements of proteins.

Three common secondary structures are:

- H – Alpha helix: A spiral-shaped protein structure.
- E – Beta sheet: A sheet-like protein structure.
- C – Coil: A region without a regular helix or sheet structure.

### 3. Algorithm

1. Import the required library.
2. Accept the target protein sequence.
3. Validate the sequence using the 20 standard amino acids.
4. Define a template protein sequence.
5. Compare the target and template sequences.
6. Calculate sequence identity.
7. Estimate secondary structure using simple amino acid rules.
8. Estimate modeling confidence based on sequence identity.
9. Display the analysis and final result.

### 4. Complete Python program

This version uses only basic Python and NumPy. It does not require a complicated protein-modeling package.

```
# Practical: Protein Structure Predictionimport warningswarnings.filterwarnings("ignore")import numpy as np# Step 1: Validate protein sequencedef validate_sequence(sequence):    valid_amino_acids = "ACDEFGHIKLMNPQRSTVWY"    for amino_acid in sequence:        if amino_acid not in valid_amino_acids:            return False    return True# Step 2: Calculate sequence identitydef sequence_identity(target, template):    length = min(len(target), len(template))    if length == 0:        return 0    matches = 0    for i in range(length):        if target[i] == template[i]:            matches += 1    identity = (matches / length) * 100
```

### 5. Example input

Use this sample sequence when running the program:

```
MKTLLVAGAAAGTASAAPVLAADKPEMVRRLAEAGVEVVVEPGDNVVAMRDPQT
```

### 6. Explanation of the program

Step 1 – Validate protein sequence

```
valid_amino_acids = "ACDEFGHIKLMNPQRSTVWY"
```

These are the 20 standard amino acids.

The function checks every character in the sequence. If an invalid character is found, it returns `False`.

Step 2 – Calculate sequence identity

```
length = min(len(target), len(template))
```

Finds the length of the shorter sequence.

```
if target[i] == template[i]:    matches += 1
```

Compares amino acids at the same position. If they match, the match count increases.

```
identity = (matches / length) * 100
```

Calculates the percentage of matching amino acids.

Formula:

\\[ \text{Sequence Identity} = \frac{\text{Number of Matches}}{\text{Compared Positions}} \times 100 \\]

Step 3 – Predict secondary structure

```
helix = "ALMEKQRH"sheet = "VIFYWT"
```

These strings contain amino acids assigned to simplified helix-forming and sheet-forming groups.

- `H` represents alpha helix.
- `E` represents beta sheet.
- `C` represents coil.

The program checks each amino acid and assigns one of these symbols.

Step 4 – Estimate confidence

```
if identity >= 50:    confidence = "High"elif identity >= 30:    confidence = "Moderate"else:    confidence = "Low"
```

The program assigns a rough confidence category based on sequence identity.

These thresholds are only for this educational demonstration. They are not a validated measure of actual protein-model accuracy.

Step 5 – Display structural representation

The loop displays each amino acid's position and its estimated secondary-structure category.

For example:

```
Position  Amino Acid  Structure
1         M           Alpha Helix
2         K           Alpha Helix
3         T           Coil
```

### 7. Expected output

The exact output depends on the sequence entered.

```
Enter protein sequence:
MKTLLVAGAAAGTASAAPVLAADKPEMVRRLAEAGVEVVVEPGDNVVAMRDPQT

Protein sequence is valid.
Protein length: 54

Template sequence:
MKTIIALSYIFCLVFADYKDDDDKGQRLIFAGKQLEDGRTLSDYVAVIDALNDSD

Sequence Identity: ...

Secondary Structure:
...

Modeling Confidence: ...

Position  Amino Acid  Structure
1         M           Alpha Helix
2         K           Alpha Helix
...

Protein Structure Analysis Completed.
```

### 8. Result

The given protein sequence was validated and compared with a template sequence. Sequence identity was calculated, and a simplified secondary-structure representation was generated using Python.

### 9. Viva questions and answers

Q1. What is protein structure prediction?

It is the process of predicting the structure of a protein from its amino acid sequence.

Q2. What is homology modeling?

Homology modeling predicts a target protein's structure using a similar protein with a known structure.

Q3. What is a template protein?

A template is a protein with a known structure used as a reference for modeling another protein.

Q4. What is sequence identity?

It is the percentage of matching amino acids between two aligned protein sequences.

Q5. What are the 20 standard amino acids?

They are represented by the codes:

`ACDEFGHIKLMNPQRSTVWY`

Q6. What is secondary structure?

It refers to local arrangements of amino acids in a protein, such as alpha helices and beta sheets.

Q7. What does H represent?

Alpha helix.

Q8. What does E represent?

Beta sheet.

Q9. What does C represent?

Coil.

Q10. What is NumPy?

NumPy is a Python library used for numerical operations and array processing. It is not needed in our simplified code.

Q11. What is the difference between primary and secondary structure?

- Primary structure: Sequence of amino acids.
- Secondary structure: Local arrangements such as helices and sheets.

Q12. Why is sequence identity important?

It helps estimate how similar the target and template sequences are.

Q13. What is the PDB?

PDB stands for Protein Data Bank. It stores experimentally determined three-dimensional structures of proteins and other biological molecules.

Q14. Does this program generate a 3D protein structure?

No. It performs simplified sequence analysis and secondary-structure estimation. Actual 3D homology modeling requires a structural template and specialized modeling software.

### 10. Quick revision

| Function / Concept              | Purpose                                |
| ------------------------------- | -------------------------------------- |
| `validate_sequence()`           | Checks amino acids                     |
| `sequence_identity()`           | Calculates sequence similarity         |
| `predict_secondary_structure()` | Assigns H, E, C                        |
| `len()`                         | Finds sequence length                  |
| `for` loop                      | Checks amino acids one by one          |
| `if-else`                       | Makes decisions                        |
| Homology modeling               | Uses a similar known protein structure |

### One correction you should know for your practical exam

Your friend's code—and our simplified version—has a limitation: it compares amino acids at the same positions without performing proper sequence alignment. Insertions and deletions can make that comparison misleading. Also, assigning H/E/C using fixed amino-acid groups is only a toy demonstration, not a scientifically reliable secondary-structure prediction.

So if the examiner asks whether this is actual 3D protein structure prediction, answer honestly:

"This is a simplified demonstration of sequence-based protein analysis and the homology-modeling concept. Actual 3D structure prediction requires sequence alignment, a suitable structural template, and a modeling tool."

That distinction is important because your assignment title specifically asks for 3D structure prediction.