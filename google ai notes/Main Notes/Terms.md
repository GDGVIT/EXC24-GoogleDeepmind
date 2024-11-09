
20-10-2024 23:05

Status:

Tags: [[AI]] [[proteins]] 


# AI/ML

#### Machine Learning
**Machine Learning**: is a subset of artificial intelligence that allows computers to learn from data and improve their performance on a specific task without being explicitly programmed. It involves creating algorithms that can analyze data, identify patterns, and make predictions or decisions.

#### Ai
**Artificial Intelligence (AI):** A computer system that can perform tasks typically requiring human intelligence, such as learning, reasoning, problem-solving, and perception.

- **Example:** Self-driving cars, virtual assistants like Siri or Alexa, recommendation systems on platforms like Netflix or Spotify.

#### AGI 
**AGI (Artificial General Intelligence)** refers to a hypothetical type of AI that possesses the ability to understand, learn, and apply knowledge across a wide range of tasks, similar to human intelligence. Unlike narrow AI, which is designed for specific tasks, AGI would be capable of performing any intellectual task that a human can.
Characteristics:
- **General intelligence:** Capable of understanding and learning from a variety of information and tasks.
- **Problem-solving:** Able to solve problems creatively and adapt to new situations.
- **Consciousness:** Potentially possessing subjective experiences and consciousness.
- **Self-awareness:** Understanding its own existence and capabilities.

#### Deep Learning
**Deep Learning:** A type of machine learning that uses artificial neural networks to learn from large amounts of data.

- **Example:** Image and speech recognition, natural language processing, medical diagnosis.
#### Neural Networks
**Neural Network:** A computational model inspired by the human brain, consisting of interconnected nodes (neurons) that process information.

- **Example:** Convolutional neural networks (CNNs) for image recognition, recurrent neural networks (RNNs) for natural language processing.

#### Neural Language Processing
**Natural Language Processing (NLP):** The ability of computers to understand and process human language.

- **Example:** Language translation, sentiment analysis, chatbots.

#### Computer Vision
**Computer Vision:** The ability of computers to interpret and understand visual information.
- **Example:** Object detection, image recognition, facial recognition.

#### Pairwise Representation (in AlphaFold)
**Pairwise Representation**: A two dimensional *image* of which bits of the protein are near to each other. It can be thought of as a 2 dimensional map of the protein's 3D shape 

#### Multimodal
A **multimodal** model is an AI system that can process and understand multiple types of data inputs, such as text, images, audio, and video, simultaneously. Multimodal models are designed to handle information across these various modalities and interpret the relationships between them, allowing for a more holistic understanding and complex reasoning that mimics human perception. For example, a multimodal AI might analyze a video by interpreting both the audio and visual elements to respond more accurately to questions about its content.

These models are used in tasks requiring cross-modal reasoning, such as describing images, generating images from text prompts, summarizing audio content, or analyzing complex multimedia inputs (like in Google’s Gemini or OpenAI's GPT-4). By integrating data from different types of sources, multimodal models can provide more contextually aware, accurate, and versatile responses.
### Transformers 
(More [here](GPT%20Notes) and [here](More%20on%20Transformers)
Transformers are a type of deep learning model architecture that have become foundational in natural language processing (NLP) and have expanded into areas like vision, audio, and even reinforcement learning. They rely on **attention mechanisms** to understand context within sequential data, allowing for highly parallelizable and scalable processing of input data, which makes them efficient for large-scale tasks.

#### Core Concepts of Transformers

1. **Self-Attention Mechanism**:
    
    - The transformer’s attention mechanism, particularly **self-attention**, allows each word or token in an input sequence to focus on (or "attend" to) other words in the sequence, regardless of distance. For example, in a sentence, self-attention helps identify dependencies between words by creating a **contextualized representation** of each token based on its relationship with others in the sequence.
    - Self-attention works by creating three vectors for each word/token in the sequence: **query (Q)**, **key (K)**, and **value (V)**. The model then calculates an attention score for each token, determining how much focus it should place on other tokens.
2. **Multi-Headed Attention**:
    
    - Transformers use multiple attention “heads” to capture different aspects of the relationships between tokens, helping the model understand nuances and dependencies in language better than single-head models.
    - Each head processes information independently, and their outputs are combined to provide a richer representation. This multi-head attention enables transformers to capture diverse patterns in sequences, like syntax and semantics, even when training on large and complex datasets.
3. **Positional Encoding**:
    
    - Unlike recurrent models, transformers do not inherently account for sequence order, so positional encoding is added to each token’s embedding to maintain word order information. This encoding is critical for language tasks where word order impacts meaning (e.g., “the cat sat on the mat” is different from “on the mat sat the cat”).
    - Positional encoding is typically based on sine and cosine functions, which add unique patterns to each position that the model can learn and use to retain sequence order information.
4. **Feed-Forward Neural Networks**:
    
    - Each attention layer in a transformer is followed by a fully connected feed-forward network applied independently to each position. This network helps transform the token representations and extract deeper, non-linear relationships between elements in a sequence.
5. **Encoder-Decoder Structure**:
    
    - The original transformer architecture is divided into an **encoder** and a **decoder**. In NLP, the encoder processes the input sequence, capturing its contextualized representation, while the decoder generates the output sequence (like translating text from one language to another).
    - Each encoder and decoder layer has both attention and feed-forward components, and in some applications (e.g., GPT-3), only the encoder is used (for understanding language) or only the decoder (for generating language).

#### Transformer Models in Practice

Since their introduction in 2017, transformers have evolved to support various types of AI tasks beyond NLP:

- **NLP Models**: BERT, GPT, and T5 are well-known transformer-based models used for language understanding, translation, summarization, and generation.
- **Vision Transformers (ViT)**: Transformers are now adapted for image processing, treating image patches as tokens. Vision transformers have shown comparable, and sometimes superior, performance to convolutional neural networks (CNNs) in vision tasks.
- **Multimodal Applications**: Transformers are used in cross-modal applications like DALL-E and CLIP, which link vision and language, allowing for creative AI applications like text-to-image generation.

#### Advantages and Limitations

**Advantages**:

- **Scalability**: Due to their parallelizable structure, transformers can train on large datasets efficiently, making them ideal for extensive tasks like language translation and document generation.
- **Versatility**: Transformers perform well across different types of data and are adaptable to domains like vision, language, and audio.

**Limitations**:

- **High Computational Demand**: Transformers require significant computational resources, especially for very large models.
- **Data Dependency**: They need substantial labeled data for training, which can be expensive and difficult to obtain for specialized tasks.

Transformers have driven advances across various fields, from language models to image synthesis, becoming a staple in modern AI research and applications.

#### Diffusion Model
A diffusion model is a type of generative model that learns to create new data by gradually transforming a simple, random distribution into a complex one that represents real-world data. Inspired by physical diffusion processes, these models simulate data transformation in a sequence of small, reversible steps. Typically, they start with data (like images) that is gradually “noised” until it becomes indistinguishable from pure noise. During training, the model learns to reverse this process by predicting and removing noise in a step-by-step manner, generating coherent outputs from noise.

Key components include:

1. **Noise Schedule**: A systematic way to add increasing levels of noise to data.
2. **Denoising Network**: Trained to remove noise in each step, reconstructing the original data.
3. **Sampling**: During generation, the denoising network reverses the noise steps, producing realistic data samples from random noise.

These models, known for their stability and capacity to generate high-quality images, are used in fields like image synthesis and inpainting, as well as recent advancements in text-to-image AI systems.


### AI Agents (LLMs)
AI agents powered by **Large Language Models (LLMs)** are advanced systems that leverage the language understanding and generation capabilities of LLMs, like GPT-4, Google Gemini, or Claude, to complete tasks that typically involve complex reasoning, interaction, and adaptation to dynamic environments. These agents are particularly useful in situations where natural language understanding, planning, or sophisticated dialogue is needed to solve problems or assist users.

#### Key Aspects of AI Agents Using LLMs

1. **Autonomous Problem-Solving**: These agents are designed to achieve specific objectives, such as handling customer service inquiries, summarizing documents, or even coding assistance. By understanding natural language instructions, they can break down complex tasks, interpret ambiguous inputs, and make contextual adjustments based on user interaction.
    
2. **Multi-Step Task Execution**: LLM-powered agents can follow multi-step instructions, such as booking reservations, conducting research, or generating content. They often use a sequence of prompts and context-based reasoning to perform tasks effectively, keeping track of prior steps and dynamically adjusting responses.
    
3. **Human-Like Interaction**: Leveraging the conversational abilities of LLMs, these agents engage users in a more natural, intuitive dialogue. They interpret questions, offer clarifications, and remember context over conversations, making them ideal for tasks like virtual assistance and tutoring.
    
4. **Integration with Other Tools**: Many LLM-powered agents, such as ChatGPT plugins and AutoGPT, can interact with external APIs, search engines, or databases to augment their capabilities. For instance, an AI agent might use an LLM to generate code but then use a web API to look up documentation or test outputs in real-time.
    
5. **Applications in Various Domains**:
    
    - **Customer Support**: LLM-based agents like ChatGPT or Google's Bard are often employed to assist in handling user queries by retrieving information, resolving issues, or forwarding complex cases.
    - **Education and Tutoring**: AI tutors use LLMs to explain concepts, provide feedback, and engage in interactive learning experiences.
    - **Research Assistance**: LLM-powered research assistants can summarize papers, pull information from various sources, and assist in organizing findings, which is helpful in fields like academia and healthcare.
    - **Programming and Code Assistance**: Agents like GitHub Copilot and OpenAI’s Codex, which are LLM-driven, help programmers by generating code, debugging, and suggesting improvements, streamlining the development process.

#### Examples of LLM-Based AI Agents

- **AutoGPT**: An experimental application using GPT models to autonomously complete tasks by generating and executing prompts in response to its own outputs.
- **ChatGPT with Plugins**: An implementation of ChatGPT that can use plugins to interact with external tools, enhancing its capabilities by allowing it to look up information, calculate, retrieve real-time data, and more.
- **Claude**: An LLM developed by Anthropic, Claude is designed to perform well in multi-turn conversations, retaining information across prompts, making it suited for use cases in customer service and dialogue systems.

These LLM-based agents are rapidly evolving, with future developments expected to increase their accuracy, task complexity, and ethical alignment. The goal is to create agents that can adapt, learn, and respond accurately across a diverse range of tasks, improving productivity and user experience across sectors.
# Biology
#### Ligands
A **ligand** is a molecule that binds to a specific site on a target molecule, usually a protein, forming a complex that can alter the target’s function. Ligands are essential in many biological processes, including cell signaling and enzymatic regulation, and can be ions, small molecules, or even larger biomolecules like antibodies. This binding is crucial in drug discovery, where ligands are designed to interact with proteins to modify biological activity and treat diseases

#### DNA
**DNA (Deoxyribonucleic Acid)** is the molecule that carries genetic information in most living organisms, encoding instructions for the development, function, growth, and reproduction of cells. DNA consists of two strands that form a double helix, with sequences of four nucleotide bases (adenine, thymine, cytosine, and guanine) arranged to convey the genetic code.

#### RNA
**RNA (Ribonucleic Acid)** is a single-stranded molecule involved in various cellular functions, including protein synthesis and gene regulation. RNA transcribes genetic information from DNA and translates it to form proteins. Unlike DNA, RNA contains uracil instead of thymine and plays diverse roles, such as messenger RNA (mRNA), which carries genetic instructions to ribosomes, and transfer RNA (tRNA), which helps assemble amino acids during protein synthesis.

# References