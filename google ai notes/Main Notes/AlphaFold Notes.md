
27-10-2024 16:15

Status: under progress

Tags: [[AlphaFold]] [[Ai]] [[google]] 

>What took us months and years to do, AlphaFold was able to do in a weekend.
>-Professor John McGeehan  
Former Director for the Centre for Enzyme Innovation (CEI)
# Origin
When AlphaGo beat a Human master at the highly complex game Go, The Deepmind founder Demin Hassabis was taken back to when he played a game called FoldIt in college. It was a game designed so anyone could try their hand at the protein folding problem. He wondered if a group of scientists at Deepmind could solve this issue whilst they had nearly 0 knowledge about biology using AI. In 2017 theoretical Chemist John Jumper joined the Deepmind team while the development of AlphaFold 1 was already underway. They entered CASP in 2018 and were able to produce results that were more accurate than the competition.
![[Pasted image 20241027221649.png]]

# AlphaFold 2
AlphaFold 2 builds on prior advances in deep learning, combining several neural network architectures with innovations specific to protein structure prediction.

#### A. **Input Features**

AlphaFold 2’s input includes two primary types of features derived from the target amino acid sequence:

- **Multiple Sequence Alignments (MSAs)**: MSAs are constructed from a large set of related protein sequences found in databases. AlphaFold 2 uses MSAs to identify evolutionary relationships, which reveal patterns of mutation and conservation that hint at the target structure.
- **Templates**: Structural information from experimentally resolved proteins in databases like the Protein Data Bank (PDB) is also used as a template, if available. These templates can guide AlphaFold in modeling similar protein folds.

#### B. **Evoformer Module**

The **Evoformer** is a deep neural network that processes and extracts information from the MSA and templates to understand the relationships between amino acids in the sequence. It’s a **Transformer-like architecture**, specifically modified to handle biological data:

- **Pair Representation**: This representation captures interactions between pairs of amino acids. For instance, if two amino acids are likely to be in contact in the folded protein, the pair representation captures this likelihood.
- **Attention Mechanism**: The attention mechanism is adapted to allow the model to focus on important relationships in the MSA data, identifying co-evolving amino acids that suggest 3D structural contacts.

The Evoformer iteratively refines these representations, allowing the model to capture complex patterns in the data, which will later guide the prediction of 3D coordinates.

#### C. **Structure Module**

Once the Evoformer has processed the input data, the **Structure Module** generates a 3D model of the protein. This module operates as a **Geometric Transformer** that gradually builds the protein structure, starting from coarse features and refining to atomic-level precision.

- **3D Coordinate Prediction**: The Structure Module predicts the 3D coordinates for each atom in the protein. It outputs the model in a way that minimizes clashes between atoms and ensures realistic bond angles.
- **End-to-End Differentiability**: The Structure Module is designed so that all steps are differentiable, meaning that the model can be trained end-to-end to improve accuracy.

### 3. Loss Function and Training Process

The model’s training objective is to minimize the error between predicted and experimentally determined structures. AlphaFold 2 uses a **plausibility-based loss function**, focusing on atom distances and structural quality:

- **FAPE (Frame Aligned Point Error)**: FAPE evaluates how well the predicted structure aligns with the true structure, considering the positions of atoms and their orientations.
- **Restraint Satisfaction**: The loss function also includes terms to satisfy physical constraints like bond lengths and angles, ensuring realistic geometries.

AlphaFold 2 was trained on the Protein Data Bank (PDB) and other databases, using a vast dataset of protein sequences and structures. The training required extensive computational resources, enabling AlphaFold 2 to capture patterns in protein structure at a level previously unachievable.

### 4. Inference and Output

During inference, AlphaFold 2 takes an amino acid sequence as input and uses the MSA and template information to generate a predicted 3D structure. The final prediction is output as a **PDB file**, which provides the coordinates of all atoms in the protein.

AlphaFold 2 also assigns a **confidence score** to each prediction, called the **pLDDT (predicted Local Distance Difference Test)**. The pLDDT score indicates the reliability of the prediction for different regions of the protein. Generally, scores above 90 are considered highly accurate, suitable for guiding experimental research.


### Reasons for Success of AlphaFold 2
In CASP 14 during the Covid 19 Lockdown, AlphaFold was able to produce results with an accuracy of over 90% very often, thereby providing great results with regards to competition and on an absolute scale as well. This can be attributed to many reasons
- While engineering the architecture for this model, some understanding that we had of proteins at the time was used, this is known as inductive bias, this enabled the model to learn extraordinarily fast from data.
- The  [PDB](<Protein_Folding#Protein Data Bank>) provided data that was particularly well suited for training in AI.
- The mix of Quality and Quantity of information came together to help AlphaFold do magical things.

By July 2022 Deepmindreleased predictions for the structures of 218 million protein structures, nearly all those that were known in the world.

### Uses
Labs have used this to synthesize new proteins.
- Molecular targets are chosen onto which the protein needs to bind onto.
- the target shape is fed into generative AI systems like RFdiffusion, it works similar to DALL-E/stable diffusion in that it generates an output from a given prompt. Here the target is the prompt and a protein whose structure binds to the target is the output and this is done using a diffusion model.
- once the protein structure is generated, it is fed into another model that gives a set of possible amino acid sequences that could give us a protein with the needed structure.
- Many such sequences are generated but not all of them may be right. A model like AlphaFold is used to then generate the protein structures of these amino acid sequences and the ones that match the required structure are then used to create proteins.
- The structure of this new protein can then be confirmed by using other methods.
### Issues
- Proteins don't work in isolation, they work in conjunction with a lot of other things.
- How do proteins talk to the rest of the cell.
- How do they interact with DNA, RNA and metals, their interaction were completely neglected.
To solve these issues, other AI systems started development

# AlphaFold 3

[alpha fold protein database](https://alphafold.ebi.ac.uk/) [alpha fold server](https://deepmind.google/technologies/alphafold/alphafold-server/)

Released in mid 2024, AlphaFold 3 adds onto the achievements of AlphaFold 2 by using a diffusion model to also predict binding structures and interaction of proteins with other molecules.

Papers have already been released that use AlphaFold to develop enzymes that break down plastics and make them reusable infinitely, amongst other things.
With the original AlphaFold 2 paper having well over 25,000 citations it's clear that it has made waves in the scientific community.

AlphaFold 3 shows marginal improvements over the previous generation in protein structure predictions but the greatest improvements lie in being able to outclass specialized models in numerous tasks all at the same time and in fields AlphaFold 2 didn't have the ability to predict.
- Accuracy in protein antibodies has more than doubled 
- [Ligands](Terms#Ligands) though being small molecules can have large applications in biology and the majority of drugs are small molecules like these. AlphaFold 3 though being a generalist AI performs better at prediction than specialist physics based and machine learning based systems.
- AlphaFold 3 can predict the structures of not only proteins but Ligands, [DNA](Terms#DNA) and [RNA](Terms#RNA) as well, with accuracy beyond previous methods.
- **An  important note to be kept in mind is that AlphaFold 3 follows a trend that has been noted recently, that a generalized AI model is able to outperform specialist models**


In October 2024, David Baker, Demis Hassabis and John Jumper shared the Nobel prize in chemistry for their work on protein structure prediction and design 
![[Pasted image 20241027221622.png]]

[architectural details of these AI models](AlphaFold%20Architecture)
# References
[AlphaFold 2 research paper](https://www.nature.com/articles/s41586-021-03819-2) [AlphaFold 3 research paper](https://www.nature.com/articles/s41586-024-07487-w) 