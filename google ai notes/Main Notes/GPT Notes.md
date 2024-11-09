
28-10-2024 15:40

Status:

Tags: [[Ai]] [[LLM-LLVM]] 


# What are GPTs 
GPT stands for generative pretrained transformer
The idea of [transformers](Terms#Transformers) were first introduced in Google's groundbreaking paper titled [Attention is all you need](https://proceedings.neurips.cc/paper_files/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf) which has since been cited over 138,000 times and it outlines the approach on how you could train ai models with massive parameter counts in parallel saving huge amounts of time as they could be computed using GPUs.![[Pasted image 20241028155139.png]]
### **1. Tokenization of Input Query**

- **Goal:** Convert the raw text input (query) into a format the model can process.
- **Process:** The input text is split into smaller units called tokens (words, subwords, or characters). Each token is assigned a unique identifier (ID) based on a pre-defined vocabulary.
- **Example:** The input “Hello, how are you?” might be tokenized into ["Hello", ",", "how", "are", "you", "?"] with each token represented by an ID.

### **2. Embedding Layer**

- **Goal:** Transform tokens into vector representations the model can work with.
- **Process:** Each token ID is mapped to a high-dimensional embedding vector. These embeddings encode semantic information, allowing the model to understand relationships between words.
- **Intuition:** Similar words or words used in similar contexts have embeddings that are close together in the vector space, helping the model understand context even for synonyms or similar phrases.

### **3. Adding Positional Encoding**

- **Goal:** Since transformers process input tokens simultaneously (non-sequentially), they lack inherent knowledge of token order. Positional encoding provides this information.
- **Process:** Each token’s embedding is adjusted by adding a positional encoding vector. The _“Attention is All You Need”_ paper introduced sinusoidal functions to generate these vectors, encoding the relative position of each token.
- **Effect:** The model can recognize token order and dependencies across sequences, critical for understanding sentence structure.

### **4. Passing Through the Encoder Stack (Self-Attention Mechanism)**

- **Goal:** The encoder stack analyzes input tokens and learns relationships between them using multi-head self-attention.
- **Self-Attention (Core Concept):** Self-attention computes the relevance of each token in the context of all others. This is done through:
    - **Query, Key, and Value Vectors:** For each token, the model generates these vectors to determine attention weights.
    - **Dot-Product Attention Calculation:** The dot product of queries and keys produces scores that determine how much attention each token should pay to others.
    - **Softmax Normalization:** Scores are normalized, and each token’s value vector is weighted based on these normalized scores.
    - **Multi-Head Attention:** Multiple sets of query-key-value operations (heads) allow the model to capture different aspects of relationships within the data.
- **Example:** In the sentence “The cat sat on the mat,” the self-attention mechanism allows the model to focus on relationships, like "cat" being related to "sat" more than "mat."
[In depth about Attention](More%20on%20Transformers#More%20on%20Attention)

### **5. Applying Feedforward Layers**

- **Goal:** Enhance the model’s representational power.
- **Process:** After each self-attention layer, a feedforward neural network processes the attention output, learning further patterns and relationships. This step is followed by normalization layers to ensure stable gradients and improved performance.

### **6. Repeating Encoder Layers**

- **Goal:** Capture complex, multi-layered dependencies in the input.
- **Process:** The encoder stack comprises multiple layers, each containing self-attention and feedforward sub-layers. These layers progressively refine the model’s understanding of the input query.

### **7. Decoder Stack and Cross-Attention**

- **Goal:** Generate output text by attending to encoder outputs and previously generated tokens.
- **Cross-Attention (New for Decoding):** The decoder contains both self-attention and cross-attention.
    - **Self-Attention in Decoder:** It focuses on previously generated tokens, helping maintain coherence in the response.
    - **Cross-Attention to Encoder:** Cross-attention layers in the decoder pay attention to encoder outputs, grounding the response in the context provided by the input query.
- **Masked Self-Attention (Decoding Specific):** During generation, the decoder only “sees” tokens that have been generated so far (to the left of the current token in the sequence). This prevents the model from “peeking” at future tokens.

### **8. Linear and Softmax Layers (Output Layer)**

- **Goal:** Convert the decoder’s processed vector into probabilities for each possible next token.
- **Process:** The final layer transforms decoder outputs into logits (raw scores) corresponding to each token in the vocabulary. These scores are passed through a softmax function, converting them into probabilities.
- **Token Selection (Next-Token Prediction):** The model selects the next token based on these probabilities. Selection strategies like greedy sampling, beam search, or top-k sampling help ensure coherent and contextually relevant generation.

### **9. Generating the Complete Response**

- **Iterative Process:** Steps 7 and 8 repeat for each token until a stopping condition is met (e.g., a special end-of-sequence token).
- **Constructing the Response:** Each predicted token is appended to the response, forming a coherent output sentence or paragraph.

---

### **Key Innovations from "Attention is All You Need" Paper**

- **Self-Attention over Recurrence:** The paper’s transformer model replaced recurrent structures with self-attention, allowing efficient parallel processing and better long-range dependency handling.
- **Multi-Head Attention:** Using multiple attention “heads” allowed the model to focus on different relationships within the input sequence simultaneously.
- **Positional Encoding:** This addition addressed the lack of inherent sequential structure in transformers, crucial for maintaining word order in natural language.

>[!Summary]+ Summary
>### **Summary**
This flow demonstrates how transformers process an input query through tokenization, embedding, self-attention, and decoding to generate a meaningful, context-aware response. The _“Attention is All You Need”_ paper’s innovations are foundational, enabling transformers like GPT to handle complex language tasks effectively without recurrence.


A very rudimentary implementation of this architecture is given below, but if you want to build it yourself I strongly recommend you to follow along with [Andrej Karpathy](https://youtu.be/kCc8FmEb1nY?si=jJIS7j6SY-8aeNnU) or this tutorial from [freecodecamp](https://www.youtube.com/watch?v=UU1WVnMk4E8&pp=ygUQZ3B0IGZyb20gc2NyYXRjaA%3D%3D) 

```python
import mmap  
import random  
import time  
  
import torch  
import torch.nn as nn  
from torch.nn import functional as F  
import pickle  
from torch.cuda.amp import autocast, GradScaler  
  
#Hyperparameters  
device = 'cuda' if torch.cuda.is_available() else 'cpu'  
block_size = 128  
batch_size = 48  
max_iters = 100000  
learning_rate = 3e-5  
eval_iters = 100  
n_embd = 384  
n_head = 20  
n_layer = 16  
dropout = 0.2  
scaler = GradScaler()  
  
  
  
  
chars = ''  
with open('vocab.txt', 'r', encoding = 'utf-8') as f:  
    text = f.read()  
    chars = sorted(list(set(text)))  
vocab_size = len(chars)  
  
string_to_int = {ch: i for i, ch in enumerate(chars)}  
int_to_string = {i: ch for i, ch in enumerate(chars)}  
encode = lambda s: [string_to_int[c] for c in s]  
decode = lambda k: ''.join([int_to_string[i] for i in k])  
data = torch.tensor(encode(text), dtype=torch.long)  
  
  
def get_random_chunk(split):  
    filename = 'train_split.txt' if split == 'train' else ('val_split.txt')  
    with open(filename, 'rb') as f:  
        with mmap.mmap(f.fileno(), 0, access = mmap.ACCESS_READ) as mm:  
            file_size = len(mm)  
            start_pos = random.randint(0, (file_size)- block_size*batch_size)  
  
            mm.seek(start_pos)  
            block = mm.read(block_size*batch_size-1)  
  
            decoded_block = block.decode('utf-8', errors = 'ignore').replace('\r', '')  
  
            data = torch.tensor(encode(decoded_block), dtype=torch.long)  
    return data  
def get_batch(split):  
    data = get_random_chunk(split)  
    ix = torch.randint(len(data) - block_size, (batch_size,))  
    x = torch.stack([data[i:i+block_size]for i in ix])  
    y = torch.stack([data[i+1:i+block_size+1] for i in ix])  
    x, y = x.to(device), y.to(device)  
    return x, y  
  
  
@torch.no_grad()  
def estimate_loss():  
    out = {}  
    model.eval()  
    for split in ['train', 'val']:  
        losses = torch.zeros(eval_iters)  
        for k in range(eval_iters):  
            X, Y = get_batch(split)  
            logits, loss = model(X, Y)  
            losses[k] = loss.item()  
        out[split] = losses.mean()  
    model.train()  
    return out  
  
  
class FeedForward(nn.Module):  
    def __init__(self, n_embd):  
  
        super().__init__()  
        self.net = nn.Sequential(  
            nn.Linear(n_embd, 4 * n_embd),  
            nn.ReLU(),  
            nn.Linear(4 * n_embd, n_embd),  
            nn.Dropout(dropout))  
  
    def forward(self, x):  
        return self.net(x)  
  
  
class Head(nn.Module):  
    """ one head of self-attention """  
  
    def __init__(self, head_size):  
        super().__init__()  
        self.key = nn.Linear(n_embd, head_size, bias=False)  
        self.query = nn.Linear(n_embd, head_size, bias=False)  
        self.value = nn.Linear(n_embd, head_size, bias=False)  
        self.register_buffer('tril', torch.tril(torch.ones(block_size, block_size)))  
  
        self.dropout = nn.Dropout(dropout)  
  
    def forward(self, x):  
        # input of size (batch, time-step, channels)  
        # output of size (batch, time-step, head size)        B,T,C = x.shape  
        k = self.key(x)   # (B,T,hs)  
        q = self.query(x) # (B,T,hs)  
        # compute attention scores ("affinities")        wei = q @ k.transpose(-2,-1) * k.shape[-1]**-0.5 # (B, T, hs) @ (B, hs, T) -> (B, T, T)  
        wei = wei.masked_fill(self.tril[:T, :T] == 0, float('-inf')) # (B, T, T)  
        wei = F.softmax(wei, dim=-1) # (B, T, T)  
        wei = self.dropout(wei)  
        # perform the weighted aggregation of the values  
        v = self.value(x) # (B,T,hs)  
        out = wei @ v # (B, T, T) @ (B, T, hs) -> (B, T, hs)  
        return out  
  
  
class MultiHeadAttention(nn.Module):  
    """ multiple heads of self-attention in parallel """  
  
    def __init__(self, num_heads, head_size):  
        super().__init__()  
        self.heads = nn.ModuleList([Head(head_size) for _ in range(num_heads)])  
        self.proj = nn.Linear(head_size * num_heads, n_embd)  
        self.dropout = nn.Dropout(dropout)  
  
    def forward(self, x):  
        out = torch.cat([h(x) for h in self.heads], dim=-1) # (B, T, F) -> (B, T, [h1, h1, h1, h1, h2, h2, h2, h2, h3, h3, h3, h3])  
        out = self.dropout(self.proj(out))  
        return out  
  
class Block(nn.Module):  
    def __init__(self, n_embd, n_head):  
        #n_embd embedding dimensions, n_head:the number of heads we'd like  
        super().__init__()  
        head_size = n_embd // n_head  
        self.sa = MultiHeadAttention(n_head, head_size)  
        self.ffwd = FeedForward(n_embd)  
        self.ln1 = nn.LayerNorm(n_embd)  
        self.ln2 = nn.LayerNorm(n_embd)  
    def forward(self, x):  
        y = self.sa(x)  
        x = self.ln1(x+y)  
        y = self.ffwd(x)  
        x = self.ln2(x+y)  
        return x  
class GPTLanguageModel(nn.Module):  
    def __init__(self, vocab_size):  
        super().__init__()  
        self.token_embedding_table = nn.Embedding(vocab_size, n_embd)  
        self.position_embedding_table = nn.Embedding(block_size, n_embd)  
        self.blocks = nn.Sequential(*[Block(n_embd, n_head = n_head) for _ in range(n_layer)])  
  
        self.ln_f = nn.LayerNorm(n_embd) #final layer Norm  
        self.lm_head = nn.Linear(n_embd, vocab_size)  
  
        self.apply(self._init_weights)  
  
    def _init_weights(self, module):  
        if isinstance(module, nn.Linear):  
            torch.nn.init.normal_(module.weight, mean=0, std=0.02)  
            if module.bias is not None:  
                torch.nn.init.zeros_(module.bias)  
        elif isinstance(module, nn.Embedding):  
            torch.nn.init.normal_(module.weight, mean=0, std=0.02)  
  
    def forward(self, index, targets=None):  
        B, T = index.shape  
        #index and targets are both (B, T) tensors  
        tok_emb = self.token_embedding_table(index)  #(B,T,C)  
        pos_emb = self.position_embedding_table(torch.arange(T, device=device)) #T,C  
        x = tok_emb + pos_emb #(B, T, C)  
        x = self.blocks(x)#(B, T, C)  
        x = self.ln_f(x)#(B, T, C)  
        logits = self.lm_head(x)#(B, T, vocab_size)  
        if targets is None:  
            loss = None  
        else:  
            B, T, C = logits.shape  
            logits = logits.view(B*T, C)  
            targets = targets.view(B*T)  
            loss = F.cross_entropy(logits, targets)  
  
        return logits, loss  
  
    def generate(self, index, max_new_tokens):  
        # index is (B, T) array of indices in the current context  
        for _ in range(max_new_tokens):  
            # crop idx to the last block_size tokens  
            index_cond = index[:, -block_size:]  
            # get the predictions  
            logits, loss = self.forward(index_cond)  
            # focus only on the last time step  
            logits = logits[:, -1, :]  # becomes (B, C)  
            # apply softmax to get probabilities            probs = F.softmax(logits, dim=-1)  # (B, C)  
            # sample from the distribution            index_next = torch.multinomial(probs, num_samples=1)  # (B, 1)  
            # append sampled index to the running sequence            index = torch.cat((index, index_next), dim=1)  # (B, T+1)  
        return index  
  
  
x, y = get_batch('train')  
model = GPTLanguageModel(vocab_size)  
  
print('Loading model')  
with open('model-01.pkl', 'rb') as f:  
    model = pickle.load(f)  
print('Model loaded')  
m = model.to(device)  
  
#print('generating text')  
'''context = torch.zeros((1,1), dtype=torch.long, device=device)  
generated_chars = decode(m.generate(context, max_new_tokens=1000)[0].tolist())  
print(generated_chars)'''  
  
optimizer = torch.optim.AdamW(model.parameters(), lr=learning_rate)  
start = time.perf_counter()  
for iter in range(max_iters):  
    if iter % eval_iters == 0:  
        losses = estimate_loss()  
        end = time.perf_counter()  
        print(f"step:{iter}, train loss: {losses['train']:.4f}, val loss: {losses['val']:.4f} in {(end - start):.4f} seconds")  
        start = time.perf_counter()  
        with open('model-01.pkl', 'wb') as f:  
            pickle.dump(model, f)  
  
        print('model-saved')  
  
    xb, yb = get_batch('train')  
    with autocast():  
        logits, loss = model(xb, yb)  
    optimizer.zero_grad(set_to_none=True)  
    scaler.scale(loss).backward()  
    scaler.step(optimizer)  
    scaler.update()  
print(loss.item())  
  
with open('model-01.pkl', 'wb') as f:  
    pickle.dump(model, f)  
  
print('model-saved')  
'''context = torch.zeros((1,1), dtype=torch.long, device=device)  
generated_chars = decode(m.generate(context, max_new_tokens=1000)[0].tolist())  
print(generated_chars)'''
```


**GPTs (Generative Pre-trained Transformers)** are a family of language models developed by OpenAI, known for their impressive natural language processing abilities. They utilize the Transformer architecture, introduced by Vaswani et al. in 2017, which uses self-attention mechanisms to understand relationships within sequences, making it particularly effective for language understanding and generation.

### Key Features and Evolution

1. **GPT-1**:
    
    - Introduced in 2018, GPT-1 was the first in the series and demonstrated that a transformer-based model could be pre-trained on a large corpus of text and then fine-tuned for specific tasks.
    - With 117 million parameters, it was trained using unsupervised learning on vast internet data, achieving basic language comprehension and generation capabilities.
2. **GPT-2**:
    
    - GPT-2, released in 2019, represented a significant leap with 1.5 billion parameters, producing coherent and contextually appropriate text on diverse topics.
    - It gained attention for generating text that appeared remarkably human, raising concerns about potential misuse, which initially led OpenAI to limit its release.
    - GPT-2 is proficient in tasks like summarization, translation, and even simple reasoning.
3. **GPT-3**:
    
    - Released in 2020, GPT-3 has 175 billion parameters and can perform complex tasks without task-specific training, simply by understanding instructions in natural language (known as "few-shot" or "zero-shot" learning).
    - Its scale and training diversity allowed it to excel in language-based applications like code generation, creative writing, and conversational AI.
4. **GPT-4**:
    
    - Launched in 2023, GPT-4 further improved context handling, fine-tuning abilities, and language translation, among other tasks.
    - OpenAI introduced multimodal capabilities in GPT-4, allowing it to handle both text and images, opening new applications in fields such as visual storytelling, diagnostics, and interactive user interfaces.

### Technical Mechanisms

- **Transformer Architecture**: GPT models use transformers, which rely on self-attention to understand context and relationships within data. This approach enables efficient processing of long text sequences compared to older models like RNNs and LSTMs.
- **Pre-training and Fine-tuning**: GPT models are first pre-trained on extensive corpora to understand general language patterns and then can be fine-tuned or prompted for specific tasks, making them highly versatile.
- **Few-shot Learning**: GPT-3 and later models can perform tasks without extensive training examples, demonstrating the potential for broader applications without task-specific datasets.

### Applications

GPT models have been applied in numerous domains:

- **Conversational Agents**: Chatbots and virtual assistants leverage GPT’s language understanding for natural interactions.
- **Content Generation**: Used for generating articles, summaries, and other creative content, GPTs excel at creating coherent and contextually relevant text.
- **Coding**: GPT models like Codex (derived from GPT-3) specialize in code generation, helping developers by generating or debugging code.
- **Education and Research**: GPT-4’s multimodal capabilities support education, research, and accessibility by providing explanations, generating visual interpretations, and assisting with learning material.

### Ethical Considerations

GPT models raise ethical considerations, such as:

- **Misinformation**: Their ability to generate convincing text can lead to the spread of misinformation if misused.
- **Bias**: Being trained on vast internet data, GPTs can inadvertently reflect biases present in that data.
- **Employment Impact**: The automation of text-based tasks can affect industries that rely on manual content creation and curation.

# Why are transformers important?
[**source**](https://aws.amazon.com/what-is/transformers-in-artificial-intelligence/)

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


# More Details on the Attention is All You Need Paper

The _Attention Is All You Need_ paper introduces the **Transformer** architecture, a model that has since become fundamental in natural language processing (NLP) and other domains. The key idea is that attention mechanisms alone can handle sequence processing tasks, removing the need for recurrent networks (like RNNs or LSTMs), which often struggle with long-term dependencies. Here’s a breakdown of the paper's core concepts and structure:

---

### 1. **Introduction**

- The paper challenges traditional sequence-to-sequence (seq2seq) models that use RNNs or LSTMs.
- The **Transformer** model relies on _self-attention mechanisms_ to handle dependencies, instead of recurrence, which speeds up training and improves parallelization.

### 2. **Self-Attention Mechanism**

- Self-attention (or intra-attention) is the backbone of the Transformer, enabling the model to focus on different parts of an input sequence at each layer.
- **Scaled Dot-Product Attention** is the main operation: Attention(Q,K,V)=softmax(QKTdk)V\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)VAttention(Q,K,V)=softmax(dk​​QKT​)V where:
    - QQQ is the Query matrix,
    - KKK is the Key matrix,
    - VVV is the Value matrix,
    - dkd_kdk​ is the dimensionality of the keys.

### 3. **Multi-Head Attention**

- Instead of applying a single self-attention mechanism, the Transformer uses **multi-head attention**:
    - The input is projected into multiple subspaces (heads), which allows the model to attend to different aspects of the sequence.
    - The results from each head are concatenated and linearly transformed to produce the final output.
- **Purpose**: Enables the model to capture more complex dependencies in the sequence by learning multiple relationships simultaneously.

### 4. **Positional Encoding**

- Since the Transformer doesn’t use recurrence, it lacks a natural way to process sequence order.
- **Positional encoding** is added to each input embedding to give the model a sense of order.
- Commonly, a mix of sine and cosine functions is used: PE(pos,2i)=sin⁡(pos100002i/d)PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d}}\right)PE(pos,2i)​=sin(100002i/dpos​) PE(pos,2i+1)=cos⁡(pos100002i/d)PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d}}\right)PE(pos,2i+1)​=cos(100002i/dpos​) where pospospos is the position and iii is the dimension.

### 5. **Encoder-Decoder Architecture**

- The Transformer is a stack of encoders and decoders:
    
    - **Encoder**: Consists of layers with multi-head attention followed by position-wise feed-forward networks.
    - **Decoder**: Similar structure but has an additional masked multi-head attention layer to prevent attending to future tokens.
- Each encoder and decoder layer has two sub-layers:
    
    1. Multi-head attention.
    2. Feed-forward neural network.
- **Residual connections** are used around each sub-layer, followed by layer normalization to stabilize training.
    

### 6. **Layer Normalization and Residual Connections**

- **Residual connections** ensure stable gradient flow and prevent gradient vanishing/explosion issues.
- **Layer normalization** is applied after adding the residual connections, helping each layer to learn more effectively.

### 7. **Feed-Forward Network**

- Each layer has a position-wise feed-forward network: FFN(x)=max⁡(0,xW1+b1)W2+b2\text{FFN}(x) = \max(0, xW_1 + b_1)W_2 + b_2FFN(x)=max(0,xW1​+b1​)W2​+b2​
- This simple two-layer network allows for additional transformation and complexity after attention.

### 8. **Training and Optimization**

- The model uses **cross-entropy loss** for training with **teacher forcing**, where ground truth sequences are used during training to predict the next token.
- **Adam optimizer** is used with learning rate scheduling: lr=dmodel−0.5×min⁡(step−0.5,step×warmup_steps−1.5)\text{lr} = d_{\text{model}}^{-0.5} \times \min(\text{step}^{-0.5}, \text{step} \times \text{warmup\_steps}^{-1.5})lr=dmodel−0.5​×min(step−0.5,step×warmup_steps−1.5)
- This schedule improves training stability, especially in the early stages.

### 9. **Model Efficiency and Scalability**

- The Transformer is highly parallelizable since each token’s computation does not depend on previous tokens.
- This architecture dramatically improves training speed, especially on long sequences, compared to RNN-based models.

### 10. **Key Results and Contributions**

- The model achieves state-of-the-art performance on machine translation tasks (e.g., English to German) with fewer resources.
- Shows the effectiveness of the attention mechanism, especially in capturing long-term dependencies.

### 11. **Future Implications**

- The architecture set the stage for BERT, GPT, and other large language models (LLMs), which all extend the ideas from this Transformer model.

---

### **Summary**

The Transformer introduced a model that relies solely on attention mechanisms to process sequential data, proving that recurrence isn’t essential for high-quality sequence modeling. It emphasizes parallelism, simplicity in architecture, and flexibility, enabling rapid advancements in NLP and influencing models in various fields. The key contributions include:

- Self-attention and multi-head attention.
- Positional encoding for sequence order.
- Layer normalization and residual connections for stable training.
- Highly parallelizable structure that allows efficient training.

This foundational paper has driven innovations across machine learning, making it a landmark in NLP and deep learning.


# References