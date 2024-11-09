
27-10-2024 23:00

Status: done

Tags: [[AlphaFold]] [[AI]] [[google]] 


# AlphaFold 2
AlphaFold 2’s architecture represents a leap in protein structure prediction through a unique combination of deep learning and computational biology techniques. Below are key components of its architecture, each contributing to its success:

### 1. **Input Processing and MSA (Multiple Sequence Alignment) Embeddings**

AlphaFold 2 relies heavily on **evolutionary information** extracted from MSAs and **pairwise residue data**:

- **MSA Embeddings**: By gathering sequences from evolutionarily related proteins, AlphaFold 2 creates an alignment that reveals conserved patterns, helping the model understand how amino acid sequences evolve over time. This alignment is critical for encoding context, as proteins with similar sequences often fold in similar ways.
- **Pairwise Features**: The model constructs a matrix of pairwise features between residues, which includes geometric relationships and distances, providing a foundation for understanding spatial interactions within the protein.

### 2. **Evoformer Module**

The **Evoformer** is a specialized neural network module designed to process and enhance MSA and pairwise information:

- **Axial Attention Mechanism**: This mechanism applies attention across rows and columns independently, which allows AlphaFold 2 to handle MSAs and pair representations efficiently without overwhelming computational resources. It captures dependencies both within sequences and across related proteins in the MSA.
- **Outer Product Mean**: This operation creates a map of relationships between amino acid pairs, encoding pairwise interactions across the protein, critical for spatial understanding.

### 3. **Structure Module**

The **Structure Module** transforms the processed MSA and pairwise features into a 3D protein structure:

- **Invariant Point Attention (IPA)**: IPA computes attention directly over 3D coordinates, ensuring the spatial structure of proteins remains consistent and realistic during modeling. This module treats 3D transformations as invariant, which means it can recognize similar spatial patterns even when rotated or shifted, reflecting real-life protein flexibility.
- **Geometry Reconstruction**: By iteratively refining residue positions based on this spatial attention, the Structure Module builds a 3D structure that satisfies physical constraints, such as bond angles and distances between atoms.

### 4. **End-to-End Differentiable Design**

One of AlphaFold 2’s breakthroughs is its **end-to-end differentiable architecture**, allowing the entire prediction process to be optimized directly from amino acid sequences to 3D structures. This contrasts with traditional methods that rely on multiple independent stages, making the model more accurate and robust by removing hand-designed components.

### 5. **Recycling Mechanism**

AlphaFold 2 incorporates **recycling**, meaning it revisits and refines intermediate predictions multiple times. This iterative refinement helps it correct errors and improve predictions by feeding back updated structure and pairwise data into the model until it converges on a stable 3D structure.

### 6. **Loss Function and pLDDT Confidence Score**

The model’s loss function optimizes **geometry and distance constraints** between residues. Additionally, it generates a **pLDDT (predicted Local Distance Difference Test) score** to indicate the model’s confidence at each residue position, providing users with an understanding of the prediction’s reliability.

>[!Summary]+ Summary
>AlphaFold 2's architecture innovates by combining MSA-based evolutionary context, axial attention in the Evoformer, spatially aware structure prediction with IPA, and iterative refinement, enabling it to predict highly accurate protein structures directly from sequences.


![[Pasted image 20241027231310.png]]
![[Pasted image 20241027231444.png]]

### Quick Rundown

1) The proteins amino acid sequence is entered into the system
2) The algorithm then searches several genetic databases for similar protein sequences found in other organisms
3) These related sequences are then aligned in an array to create a representation called **Multiple Sequence Alignment or MSA**![[Pasted image 20241028091529.png]]
4) The MSA contains information about the evolution of the protein across different organisms 
5) Next, AlphaFold produces a matrix to encode the spatial relationships between every pair of amino acids in the target sequence called a [**Pairwise Representation**](Terms#Pairwise%20Representation%20(in%20AlphaFold))
6) The MSA and the Pairwise Representation are then entered into the Evoformer module which is a [transformer](Terms#Transformers) type neural network.![[Pasted image 20241028141644.png]] *inner workings of the evoformer module*
7) The self attention extracts meaningful information out of the given data while also dynamically augmenting the data in useful ways, setting up a sort of conversation between the evolution of the protein and the hypothetical geometry of the protein.
8) The revised pairwise representation is then passed onto another transformer called the structure module, which calculates the geometry at play to produce the initial guess of the protein's folded structure.
9) This prediction is then refined by cycling it through the whole algorithm before producing a final output.
10) AlphaFold also produces a confidence score for different parts of the protein structure ![[Pasted image 20241028102051.png]] *as taken from [alphafold database](https://alphafold.ebi.ac.uk/entry/A0A1V4AYA7)*

# AlphaFold 3
 The architecture is similar to AlphaFold 2 however the evoformer module has now been switched out for a newer simpler module knows as a pairformer.![[Pasted image 20241028141125.png]]
 *overall architecture*
 ![[Pasted image 20241028141810.png]]
 *difference in the evoformer and the pairformer*

 You would also notice a [diffusion](Terms#Diffusion%20Model) model present at the end as well to create the 3D molecular structure.
 This model consists of a lot of minor adjustments made manually to improve the accuracy by if only a little at a time.

#### Limitations
- It can only create static structures as of now and cannot capture more dynamic behaviors.
- Since the diffusion module starts out with a bunch of randomly generated noise, the state from which the noise is generated can have a very minor impact on the structure that is generated (this can be easily mitigated by running it multiple times from different initial noise)

