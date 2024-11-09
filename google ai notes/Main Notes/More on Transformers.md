
04-11-2024 09:57

Status:

Tags: [[Ai]] [[GPT Notes]] [[LLM-LLVM]] [[Gemini]] 


# What are transformers in artificial intelligence?

Transformers are a type of neural network architecture that transforms or changes an input sequence into an output sequence. They do this by learning context and tracking relationships between sequence components. For example, consider this input sequence: "What is the color of the sky?" The transformer model uses an internal mathematical representation that identifies the relevancy and relationship between the words color, sky, and blue. It uses that knowledge to generate the output: "The sky is blue." 

Organizations use transformer models for all types of sequence conversions, from speech recognition to machine translation and protein sequence analysis.

[Read about neural networks](https://aws.amazon.com/what-is/neural-network/)

[Read about artificial intelligence (AI)](https://aws.amazon.com/what-is/artificial-intelligence/)

## Why are transformers important?

Early [deep learning](https://aws.amazon.com/what-is/deep-learning/) models that focused extensively on [natural language processing](https://aws.amazon.com/what-is/nlp/) (NLP) tasks aimed at getting computers to understand and respond to natural human language. They guessed the next word in a sequence based on the previous word.

To understand better, consider the autocomplete feature in your smartphone. It makes suggestions based on the frequency of word pairs that you type. For example, if you frequently type "I am fine," your phone autosuggests _fine_ after you type _am._

Early [machine learning](https://aws.amazon.com/what-is/machine-learning/) (ML) models applied similar technology on a broader scale. They mapped the relationship frequency between different word pairs or word groups in their training data set and tried to guess the next word. However, early technology couldn’t retain context beyond a certain input length. For example, an early ML model couldn’t generate a meaningful paragraph because it couldn’t retain context between the first and last sentence in a paragraph. To generate an output such as "I am from Italy. I like horse riding. I speak Italian.", the model needs to remember the connection between Italy and Italian, which early neural networks just couldn’t do.

Transformer models fundamentally changed NLP technologies by enabling models to handle such long-range dependencies in text. The following are more benefits of transformers.

### **Enable large-scale models**

Transformers process long sequences in their entirety with parallel computation, which significantly decreases both training and processing times. This has enabled the training of very large language models (LLM), such as GPT and BERT, that can learn complex language representations. They have billions of parameters that capture a wide range of human language and knowledge, and they’re pushing research toward more generalizable AI systems.

[Read about large language models](https://aws.amazon.com/what-is/large-language-model/)

[Read about GPT](https://aws.amazon.com/what-is/gpt/)

### **Enable faster customization**

With transformer models, you can use techniques such as transfer learning and retrieval augmented generation (RAG). These techniques enable the customization of existing models for industry organization-specific applications. Models can be pretrained on large datasets and then fine-tuned on smaller, task-specific datasets. This approach has democratized the use of sophisticated models and removed resource constraint limitations in training large models from scratch. Models can perform well across multiple domains and tasks for various use cases.

### **Facilitate multi-modal AI systems**

With transformers, you can use AI for tasks that combine complex data sets. For instance, models like DALL-E show that transformers can generate images from textual descriptions, combining NLP and computer vision capabilities. With transformers, you can create AI applications that integrate different information types and mimic human understanding and creativity more closely.

[Read about computer vision](https://aws.amazon.com/what-is/computer-vision/)

### **AI research and industry innovation**

Transformers have created a new generation of AI technologies and AI research, pushing the boundaries of what's possible in ML. Their success has inspired new architectures and applications that solve innovative problems. They have enabled machines to understand and generate human language, resulting in applications that enhance customer experience and create new business opportunities.

## What are the use cases for transformers?

You can train large transformer models on any sequential data like human languages, music compositions, programming languages, and more. The following are some example use cases.

### **Natural language processing**

Transformers enable machines to understand, interpret, and generate human language in a way that's more accurate than ever before. They can summarize large documents and generate coherent and contextually relevant text for all kinds of use cases. Virtual assistants like Alexa use transformer technology to understand and respond to voice commands.

### **Machine translation**

Translation applications use transformers to provide real-time, accurate translations between languages. Transformers have significantly improved the fluency and accuracy of translations as compared to previous technologies.

[Read about machine translation](https://aws.amazon.com/what-is/machine-translation/)

### **DNA sequence analysis**

By treating segments of DNA as a sequence similar to language, transformers can predict the effects of genetic mutations, understand genetic patterns, and help identify regions of DNA that are responsible for certain diseases. This capability is crucial for personalized medicine, where understanding an individual's genetic makeup can lead to more effective treatments.

### **Protein structure analysis**

Transformer models can process sequential data, which makes them well suited for modeling the long chains of amino acids that fold into complex protein structures. Understanding protein structures is vital for drug discovery and understanding biological processes. You can also use transformers in applications that predict the 3D structure of proteins based on their amino acid sequences.

## How do transformers work?

Neural networks have been the leading method in various AI tasks such as image recognition and NLP since the early 2000s. They consist of layers of interconnected computing nodes, or _neurons_, that mimic the human brain and work together to solve complex problems.

Traditional neural networks that deal with data sequences often use an encoder/decoder architecture pattern. The encoder reads and processes the entire input data sequence, such as an English sentence, and transforms it into a compact mathematical representation. This representation is a summary that captures the essence of the input. Then, the decoder takes this summary and, step by step, generates the output sequence, which could be the same sentence translated into French.

This process happens sequentially, which means that it has to process each word or part of the data one after the other. The process is slow and can lose some finer details over long distances.

### **Self-attention mechanism**

Transformer models modify this process by incorporating something called a _self-attention mechanism_. Instead of processing data in order, the mechanism enables the model to look at different parts of the sequence all at once and determine which parts are most important. 

Imagine that you're in a busy room and trying to listen to someone talk. Your brain automatically focuses on their voice while tuning out less important noises. Self-attention enables the model do something similar: it pays more attention to the relevant bits of information and combines them to make better output predictions. This mechanism makes transformers more efficient, enabling them to be trained on larger datasets. It’s also more effective, especially when dealing with long pieces of text where context from far back might influence the meaning of what's coming next.

## What are the components of transformer architecture?

Transformer neural network architecture has several software layers that work together to generate the final output. The following image shows the components of transformation architecture, as explained in the rest of this section.

  
![](https://d1.awsstatic.com/GENAI-1.151ded5440b4c997bac0642ec669a00acff2cca1.png)

### **Input embeddings**

This stage converts the input sequence into the mathematical domain that software algorithms understand. At first, the input sequence is broken down into a series of tokens or individual sequence components. For instance, if the input is a sentence, the tokens are words. Embedding then transforms the token sequence into a mathematical vector sequence. The vectors carry semantic and syntax information, represented as numbers, and their attributes are learned during the training process.

You can visualize vectors as a series of coordinates in an _n_-dimensional space. As a simple example, think of a two-dimensional graph, where _x_ represents the alphanumeric value of the first letter of the word and _y_ represents their categories. The word _banana_ has the value (2,2) because it starts with the letter _b_ and is in the category _fruit_. The word _mango_ has the value (13,2) because it starts with the letter _m_ and is also in the category _fruit._ In this way, the vector (_x,y_) tells the neural network that the words _banana_ and _mango_ are in the same category. 

Now imagine an _n_-dimensional space with thousands of attributes about any word's grammar, meaning, and use in sentences mapped to a series of numbers. Software can use the numbers to calculate the relationships between words in mathematical terms and understand the human language model. Embeddings provide a way to represent discrete tokens as continuous vectors that the model can process and learn from.

### **Positional encoding**

Positional encoding is a crucial component in the transformer architecture because the model itself doesn’t inherently process sequential data in order. The transformer needs a way to consider the order of the tokens in the input sequence. Positional encoding adds information to each token's embedding to indicate its position in the sequence. This is often done by using a set of functions that generate a unique positional signal that is added to the embedding of each token. With positional encoding, the model can preserve the order of the tokens and understand the sequence context.

### **Transformer block**

A typical transformer model has multiple transformer blocks stacked together. Each transformer block has two main components: a multi-head self-attention mechanism and a position-wise feed-forward neural network. The self-attention mechanism enables the model to weigh the importance of different tokens within the sequence. It focuses on relevant parts of the input when making predictions.

For instance, consider the sentences "_Speak no lies_" and "_He lies down.__"_ In both sentences, the meaning of the word _lies_ can’t be understood without looking at the words next to it. The words _speak_ and _down_ are essential to understand the correct meaning. Self-attention enables the grouping of relevant tokens for context.

The feed-forward layer has additional components that help the transformer model train and function more efficiently. For example, each transformer block includes:

- Connections around the two main components that act like shortcuts. They enable the flow of information from one part of the network to another, skipping certain operations in between.
- Layer normalization that keeps the numbers—specifically the outputs of different layers in the network—inside a certain range so that the model trains smoothly.
- Linear transformation functions so that the model adjusts values to better perform the task it's being trained on—like document summary as opposed to translation.

### **Linear and softmax blocks**

Ultimately the model needs to make a concrete prediction, such as choosing the next word in a sequence. This is where the linear block comes in. It’s another fully connected layer, also known as a dense layer, before the final stage. It performs a learned linear mapping from the vector space to the original input domain. This crucial layer is where the decision-making part of the model takes the complex internal representations and turns them back into specific predictions that you can interpret and use. The output of this layer is a set of scores (often called logits) for each possible token.

The softmax function is the final stage that takes the logit scores and normalizes them into a probability distribution. Each element of the softmax output represents the model's confidence in a particular class or token.

## How are transformers different from other neural network architectures?

Recurrent neural networks (RNNs) and convolutional neural networks (CNNs) are other neural networks frequently used in machine learning and deep learning tasks. The following explores their relationships to transformers.

### **Transformers vs. RNNs**

Transformer models and RNNs are both architectures used for processing sequential data.

RNNs process data sequences one element at a time in cyclic iterations. The process starts with the input layer receiving the first element of the sequence. The information is then passed to a hidden layer, which processes the input and passes the output to the next time step. This output, combined with the next element of the sequence, is fed back into the hidden layer. This cycle repeats for each element in the sequence, with the RNN maintaining a hidden state vector that gets updated at each time step. This process effectively enables the RNN to remember information from past inputs.

In contrast, transformers process entire sequences simultaneously. This parallelization enables much faster training times and the ability to handle much longer sequences than RNNs. The self-attention mechanism in transformers also enables the model to consider the entire data sequence simultaneously. This eliminates the need for recurrence or hidden vectors. Instead, positional encoding maintains information about the position of each element in the sequence.

Transformers have largely superseded RNNs in many applications, especially in NLP tasks, because they can handle long-range dependencies more effectively. They also have greater scalability and efficiency than RNNs. RNNs are still useful in certain contexts, especially where model size and computational efficiency are more critical than capturing long-distance interactions.

### **Transformers vs. CNNs**

CNNs are designed for grid-like data, such as images, where spatial hierarchies and locality are key. They use convolutional layers to apply filters across an input, capturing local patterns through these filtered views. For example, in image processing, initial layers might detect edges or textures, and deeper layers recognize more complex structures like shapes or objects.

Transformers were primarily designed to handle sequential data and couldn’t process images. Vision transformer models are now processing images by converting them into a sequential format. However, CNNs continue to remain a highly effective and efficient choice for many practical computer vision applications.

## What are the different types of transformer models?

Transformers have evolved into a diverse family of architectures. The following are some types of transformer models.

### **Bidirectional transformers**

Bidirectional encoder representations from transformers (BERT) models modify the base architecture to process words in relation to all the other words in a sentence rather than in isolation. Technically, it employs a mechanism called the bidirectional masked language model (MLM). During pretraining, BERT randomly masks some percentage of the input tokens and predicts these masked tokens based on their context. The bidirectional aspect comes from the fact that BERT takes into account both the left-to-right and right-to-left token sequences in both layers for greater comprehension.

### **Generative pretrained transformers**

GPT models use stacked transformer decoders that are pretrained on a large corpus of text by using language modeling objectives. They are autoregressive, which means that they regress or predict the next value in a sequence based on all preceding values. By using more than 175 billion parameters, GPT models can generate text sequences that are adjusted for style and tone. GPT models have sparked the research in AI toward achieving artificial general intelligence. This means that organizations can reach new levels of productivity while reinventing their applications and customer experiences.

### **Bidirectional and autoregressive transformers**

A bidirectional and auto-regressive transformer (BART) is a type of transformer model that combines bidirectional and autoregressive properties. It’s like a blend of BERT's bidirectional encoder and GPT's autoregressive decoder. It reads the entire input sequence at once and is bidirectional like BERT. However, it generates the output sequence one token at a time, conditioned on the previously generated tokens and the input provided by the encoder.

### **Transformers for multimodal tasks**

Multimodal transformer models such as ViLBERT and VisualBERT are designed to handle multiple types of input data, typically text and images. They extend the transformer architecture by using dual-stream networks that process visual and textual inputs separately before fusing the information. This design enables the model to learn cross-modal representations. For example, ViLBERT uses co-attentional transformer layers to enable the separate streams to interact. It’s crucial for situations where understanding the relationship between text and images is key, such as visual question-answering tasks.

### **Vision transformers**

Vision transformers (ViT) repurpose the transformer architecture for image classification tasks. Instead of processing an image as a grid of pixels, they view image data as a sequence of fixed-size patches, similar to how words are treated in a sentence. Each patch is flattened, linearly embedded, and then processed sequentially by the standard transformer encoder. Positional embeddings are added to maintain spatial information. This usage of global self-attention enables the model to capture relationships between any pair of patches, regardless of their position.

# More on Attention
The **Attention Mechanism** introduced in the "Attention Is All You Need" paper by Vaswani et al. (2017) is the core component of the Transformer model, designed to address limitations of previous sequence models like RNNs, especially in handling long-range dependencies. Here’s a breakdown of the mechanism as outlined in the paper:

### 1. Overview of Attention in Transformers

The **Attention mechanism** allows the model to focus on different parts of the input sequence when making predictions, giving it the flexibility to consider all positions of the input sequence simultaneously. This mechanism removes the sequential constraints of RNNs and makes parallel processing possible, speeding up computations and improving performance on tasks requiring long-range dependencies.

The Transformer uses **self-attention**, where each token in the input sequence attends to all other tokens in the sequence, including itself. This lets the model capture contextual relationships between tokens, which is particularly valuable in NLP tasks.

### 2. Self-Attention: Key, Query, and Value Vectors

In the Transformer, each token in the input sequence is represented as an embedding vector and transformed into three different vectors:

- **Query (Q)**
- **Key (K)**
- **Value (V)**

These vectors are obtained by multiplying the input embeddings with weight matrices that are learned during training:

Q=XWQ,K=XWK,V=XWVQ = XW_Q, \quad K = XW_K, \quad V = XW_VQ=XWQ​,K=XWK​,V=XWV​

where XXX is the matrix of input embeddings, and WQW_QWQ​, WKW_KWK​, and WVW_VWV​ are learned weight matrices for query, key, and value projections.

### 3. Scaled Dot-Product Attention

The core calculation of self-attention is based on the **Scaled Dot-Product Attention** formula, which computes an attention score between the queries and keys, then applies these scores to the values:

1. **Dot Product of Queries and Keys**: Each query vector is multiplied by every key vector. This measures the relevance of each key with respect to the query, generating an attention score:
    
    Attention Scores=QKT\text{Attention Scores} = QK^TAttention Scores=QKT
2. **Scaling**: The scores are scaled by the square root of the dimension of the key vectors, dk\sqrt{d_k}dk​​, to prevent extremely large values in the softmax, which could make the gradients small and lead to slow learning:
    
    Scaled Scores=QKTdk\text{Scaled Scores} = \frac{QK^T}{\sqrt{d_k}}Scaled Scores=dk​​QKT​
3. **Softmax**: A softmax function is applied to the scaled scores to convert them into a probability distribution, allowing each token to focus on specific parts of the input sequence:
    
    Attention Weights=softmax(QKTdk)\text{Attention Weights} = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)Attention Weights=softmax(dk​​QKT​)
4. **Weighted Sum of Values**: The attention weights are multiplied by the value vectors, which effectively combines information from different parts of the sequence:
    
    Attention Output=Attention Weights×V\text{Attention Output} = \text{Attention Weights} \times VAttention Output=Attention Weights×V

The output of this calculation is a new representation for each token, informed by its relationship with other tokens in the sequence.

### 4. Multi-Head Attention

To allow the model to capture various types of relationships simultaneously, the Transformer employs **Multi-Head Attention**. This involves creating multiple independent self-attention "heads" by projecting queries, keys, and values into multiple subspaces with separate learned projections. The results of each head are concatenated and linearly transformed:

1. **Independent Heads**: Each attention head independently computes its own set of queries, keys, and values, and attends to different parts of the sequence.
    
2. **Concatenation**: The outputs of all heads are concatenated into a single vector and passed through a final linear layer to combine information across all heads.
    

This approach allows each head to focus on different relationships or aspects of the input, providing richer and more nuanced representations.

### 5. Application of Attention in Transformers

The Transformer applies **self-attention** in both the encoder and decoder layers:

- In the **encoder**, each token attends to every other token in the input sequence.
- In the **decoder**, the decoder attends to both the previously generated tokens (masked self-attention) and the encoder output.

This dual use of self-attention and cross-attention in the decoder allows the Transformer to consider both the target sequence and the context from the input sequence, making it highly effective for sequence-to-sequence tasks like machine translation.

### 6. Benefits of the Attention Mechanism

The self-attention mechanism provides several key advantages:

- **Parallelization**: Since attention operates on the entire sequence at once, it allows for significant parallelization compared to RNNs, which rely on sequential processing.
- **Long-Range Dependency Capture**: Self-attention considers all tokens, making it easier to capture relationships over long distances, which is challenging for RNNs.
- **Interpretability**: The attention weights provide insight into which tokens the model focuses on, aiding interpretability and analysis.

### Summary

The attention mechanism introduced in the Transformer model enables efficient and powerful sequence modeling by allowing each token to attend to all others, achieving strong performance without recurrence. Through scaled dot-product attention and multi-head attention, Transformers capture complex relationships across long sequences, setting a new standard in natural language
# References
https://aws.amazon.com/what-is/transformers-in-artificial-intelligence/