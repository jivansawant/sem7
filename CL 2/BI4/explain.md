Let's do this in our usual practical format: Aim → Theory → Algorithm → Complete code → Line-by-line explanation → Output → Result → Viva questions.

First, one important correction about your friend's code: it does not actually perform molecular docking. It calculates molecular properties, filters possible candidates, and checks whether AutoDock Vina and prepared files exist. It only prints a docking command; it doesn't execute it.

We'll make the practical simple while keeping the two main parts of the assignment:

1. Virtual screening: Check and shortlist small molecules using their molecular properties.
2. Molecular docking: Use AutoDock Vina to calculate binding affinity, provided the required protein and ligand files are prepared.

## Practical 4: Molecular Docking and Virtual Screening

### 1. Aim

To perform virtual screening of small molecules based on their molecular properties and use molecular docking to predict their binding affinity with a protein target.

### 2. Theory

Molecular docking is a computational technique used to predict how a small molecule (ligand) binds to a protein (receptor).

- Protein/Receptor: The target molecule where the ligand may bind.
- Ligand: A small molecule that may bind to the protein.
- Binding affinity: An estimate of how strongly a ligand binds to a protein.
- Virtual screening: The process of checking many molecules computationally to identify promising candidates.

RDKit calculates molecular properties, while AutoDock Vina performs docking and estimates binding affinity.

### 3. Algorithm

1. Import the required libraries.
2. Create a list of small molecules using their SMILES representations.
3. Calculate molecular properties using RDKit.
4. Filter molecules using Lipinski's Rule of Five.
5. Display the shortlisted molecules.
6. Check whether AutoDock Vina and prepared protein/ligand files are available.
7. Perform docking using AutoDock Vina.
8. Display the docking results and binding affinity.

### 4. Complete Python program

We will use six sample molecules. The code has two sections: virtual screening and actual docking.

```
# Practical 4: Molecular Docking and Virtual Screening# Suppress warning messagesimport warningswarnings.filterwarnings("ignore")import osimport shutilimport subprocessimport pandas as pdfrom rdkit import Chemfrom rdkit.Chem import Descriptors, Lipinski# PART 1: VIRTUAL SCREENING# Step 1: Create a list of moleculesligands = {    "Molecule": [        "Ligand_01",        "Ligand_02",        "Ligand_03",        "Ligand_04",        "Ligand_05",        "Ligand_06"    ],    "SMILES": [        "CCO",        "CC(=O)Oc1ccccc1C(=O)O",        "CCN(CC)CC",        "c1ccccc1",        "CC(C)Cc1ccc(cc1)C(C)C(=O)O",        "CCOC(=O)c1ccccc1"    ]}ligands_df = pd.DataFrame(ligands)print("Molecule List:")print(ligands_df)# Step 2: Calculate molecular propertiesresults = []for index, row in ligands_df.iterrows():    molecule = Chem.MolFromSmiles(row["SMILES"])    if molecule is not None:        results.append({
```

Installation: Run this in your terminal or Jupyter Notebook if the libraries are not installed:

```
pip install pandas rdkit
```

AutoDock Vina is a separate application; installing the Python libraries alone does not install it.

### 5. Explanation of the code

Part 1: Virtual screening

Step 1 – Create molecules

```
ligands = {    "Molecule": ["Ligand_01", "Ligand_02"],    "SMILES": ["CCO", "CC(=O)Oc1ccccc1C(=O)O"]}
```

- A molecule is identified by its name.
- SMILES is a text representation of a chemical structure.
- For example, `CCO` represents ethanol.

Step 2 – Convert SMILES into molecules

```
molecule = Chem.MolFromSmiles(row["SMILES"])
```

This converts the SMILES string into a molecular structure that RDKit can understand.

Step 3 – Calculate molecular properties

```
Descriptors.MolWt(molecule)Descriptors.MolLogP(molecule)Lipinski.NumHDonors(molecule)Lipinski.NumHAcceptors(molecule)
```

| Property         | Meaning                                                          |
| ---------------- | ---------------------------------------------------------------- |
| Molecular Weight | Mass of the molecule                                             |
| LogP             | Estimates how well the molecule partitions into fat versus water |
| HBD              | Number of hydrogen bond donors                                   |
| HBA              | Number of hydrogen bond acceptors                                |

Step 4 – Apply Lipinski's Rule of Five

```
filtered = desc[    (desc["Molecular_Weight"] <= 500) &    (desc["LogP"] <= 5) &    (desc["HBD"] <= 5) &    (desc["HBA"] <= 10)]
```

This filters molecules using four commonly used drug-likeness guidelines:

- Molecular weight should be at most 500.
- LogP should be at most 5.
- Hydrogen bond donors should be at most 5.
- Hydrogen bond acceptors should be at most 10.

Molecules passing these filters are shortlisted for further investigation. Passing the rules does not prove that a molecule is an effective drug.

Part 2: Molecular docking

Step 5 – Check AutoDock Vina

```
vina = shutil.which("vina")
```

Checks whether AutoDock Vina is installed and available in the system PATH.

Step 6 – Check protein and ligand files

```
receptor = "receptor.pdbqt"ligand = "ligand_01.pdbqt"
```

- `receptor.pdbqt` is the prepared protein structure.
- `ligand_01.pdbqt` is the prepared small-molecule structure.

Both files are required for docking.

Step 7 – Set docking box

```
center_x = 0center_y = 0center_z = 0
```

These values specify the centre of the docking search region.

```
size_x = 20size_y = 20size_z = 20
```

These values specify the dimensions of the docking search region.

The example coordinates are placeholders. For real docking, use the actual binding-site coordinates of your chosen protein. Arbitrary coordinates may produce meaningless results.

Step 8 – Execute docking

```
result = subprocess.run(    command,    capture_output=True,    text=True)
```

This executes the AutoDock Vina command from Python.

- `subprocess.run()` executes an external program.
- `capture_output=True` captures its output.
- `text=True` returns the output as readable text.
- `returncode == 0` indicates successful execution.

The docking program produces a docked ligand structure and reports estimated binding affinity.

### 6. Expected output

The virtual screening section should display something similar to:

```
Molecule List:
   Molecule                         SMILES
0  Ligand_01                       CCO
1  Ligand_02                       CC(=O)Oc1ccccc1C(=O)O
...

Molecular Properties:
   Molecule  Molecular_Weight  LogP  HBD  HBA
   ...

Virtual Screening Results:
   Molecule  Molecular_Weight  LogP  HBD  HBA
   ...

Docking cannot be performed.
Check AutoDock Vina and the prepared PDBQT files.
```

The docking message above is expected if you haven't prepared the protein and ligand files or installed Vina.

If docking runs successfully, Vina displays estimated binding affinities in kcal/mol. A more negative binding affinity generally indicates stronger predicted binding, but it does not prove that a molecule is an effective drug.

### 7. Result

Virtual screening was performed using molecular properties and Lipinski's Rule of Five to shortlist potential candidates. Molecular docking can then be performed using AutoDock Vina to estimate the binding affinity between a prepared protein receptor and ligand.

### 8. Viva questions and answers

Q1. What is molecular docking?

Molecular docking is a computational method used to predict how a small molecule binds to a protein.

Q2. What is a ligand?

A ligand is a small molecule that can bind to a protein.

Q3. What is a receptor?

A receptor is the target protein with which the ligand interacts.

Q4. What is virtual screening?

Virtual screening is the computational process of checking many molecules to identify promising drug candidates.

Q5. What is RDKit?

RDKit is a Python library used for chemical informatics and molecular property calculations.

Q6. What is SMILES?

SMILES is a text-based representation of a chemical structure.

Q7. What is Lipinski's Rule of Five?

It is a set of guidelines used to evaluate drug-likeness based on molecular weight, LogP, hydrogen bond donors, and acceptors.

Q8. What is AutoDock Vina?

AutoDock Vina is a software tool used to perform molecular docking.

Q9. What is PDBQT?

PDBQT is a molecular structure file format used by AutoDock Vina. It includes information such as atom types and partial charges.

Q10. What is binding affinity?

Binding affinity indicates how strongly a ligand is predicted to bind to a protein.

Q11. Which binding affinity is generally better?

A more negative predicted binding affinity is generally considered better.

Q12. Does virtual screening confirm that a drug works?

No. It only identifies possible candidates for further experimental testing.

## 9. What you should remember for the practical

Only remember these important functions:

| Function                   | Purpose                     |
| -------------------------- | --------------------------- |
| `Chem.MolFromSmiles()`     | Creates molecule            |
| `Descriptors.MolWt()`      | Calculates molecular weight |
| `Descriptors.MolLogP()`    | Calculates LogP             |
| `Lipinski.NumHDonors()`    | Counts H-bond donors        |
| `Lipinski.NumHAcceptors()` | Counts H-bond acceptors     |
| `shutil.which()`           | Checks Vina installation    |
| `subprocess.run()`         | Executes docking            |

One important limitation of this practical: Our virtual screening checks only six example molecules, and the docking code runs only one prepared ligand. It does not automatically convert all shortlisted molecules into PDBQT files or dock all of them. That requires additional protein/ligand preparation.

For your exam, understand this distinction: RDKit performs molecular property screening; AutoDock Vina performs molecular docking. They are two different steps.