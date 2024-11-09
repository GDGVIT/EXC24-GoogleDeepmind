
26-10-2024 21:12

Status: under work

Tags: [[proteins]] [[AlphaFold]] 


# Proteins

### What is a protein
Proteins are large, complex molecules that play many critical roles in the body. They do most of the work in cells and are required for the structure, function, and regulation of the body’s tissues and organs.

Proteins are made up of hundreds or thousands of smaller units called amino acids, which are attached to one another in long chains. There are 20 different types of amino acids that can be combined to make a protein. The sequence of amino acids determines each protein’s unique 3-dimensional structure and its specific function. Amino acids are coded by combinations of three DNA building blocks (nucleotides), determined by the sequence of genes.

Examples: Hemoglobin, Anti-bodies, etc.


### What gives a protein it's function
The function of a protein is a product of it's 3d molecular structure, which is influenced by a large number of factors (electric and magnetic fields, temperature, pH, chemicals, space limitation and molecular crowding). 

All proteins are formed from 20 different amino acids connected in polypeptide chains. When these chains are assembled in a cell following a certain *recipe* encoded in the DNA of the cell, the proteins are not folded. 

### The Protein Folding Problem
In 1969, biologist Cyrus Leventhal saw that even for small simple proteins, there were an astronomical number of possible structures to be formed through folding. Testing out every possible configuration could take more time than the age of the universe, however since proteins fold reliably into their structures in less than a second, therefore there must be certain process responsible for this, and this became known as the protein folding problem which consists of the following questions

- How does a sequence of amino acids encode the final 3D structure of a protein?
- What are the series of stages along which it folds?
- How do we computationally predict the 3D structure?

These questions are core problems for structural biologists

### Why does the function of a protein matter

Proteins are the building blocks of life, and since protein structures give the protein it's function, understanding them tells us what specific proteins do and gives us ways to tackle issues related to misfolded proteins, like Alzheimer's and sickle cell anemia. Drugs can then be created to target that specific sickness creating faster, cheaper and better acting medication to sicknesses

### Approaches to Studying Protein Folding

Historically, scientists have used several methods to understand and predict protein folding:

1. **X-ray Crystallography**: This technique determines protein structures by analyzing the patterns of X-rays diffracted through crystallized proteins.
2. **Nuclear Magnetic Resonance (NMR) Spectroscopy**: NMR allows scientists to study protein structures in solution, which can more closely resemble physiological conditions.
3. **Cryo-Electron Microscopy (Cryo-EM)**: This is a high-resolution imaging method for studying protein structures without needing crystallization.

While accurate, these methods are time-consuming and expensive. They also require specialized equipment and can only be applied to certain proteins, which makes computational methods crucial for more efficient prediction.

It would cost hundreds of thousands of dollars and years of a PHD student's time to even just derive one structure.

### Protein Data Bank
In the 1970s, the PDB was started to catalog predetermined structures and stores the location of each atom as a set of coordinates in 3D space. The PDB now holds the structural data for over 200,000 proteins.

### Denaturation of Proteins 
In the 1970s, when proteins were denatured to unfold them, the reversal of the conditions required for denaturation caused the proteins to fold back to their native shapes, therefore **all the info required for the 3D structure of a protein are encoded in it's chemical sequence of amino acids** thus proving that you didn't need any biological machinery for the folding of a protein to take place, and thus it's structure could in theory be computed.

### CASP
CASP, the Critical Assessment of Structure Prediction, is a biennial competition that evaluates the performance of computational methods in protein structure prediction. Launched in 1994, CASP challenges researchers to predict the 3D structures of proteins based on amino acid sequences, providing unknown structures for testing. This competition has become a benchmark for advancements in the field, highlighting methods like AlphaFold, which achieved near-experimental accuracy in CASP14. CASP’s role is pivotal in guiding progress toward reliable, automated structure prediction methods that are essential in biological research.
### Physics Based Systems 
Initially there were no algorithms present that could even remotely come close to predicting protein structures, and this caused early researchers to try and derive the entire protein folding process step by step and hopefully recreate them using an algorithm, thus the earlier methods hoped on using physics simulations to figure out the protein structure from the amino acid sequence


# References
[How AI Solved Protein Folding and Won a Nobel Prize](https://youtu.be/cx7l9ZGFZkw?si=qrNDPnxBzkmIne2F)
