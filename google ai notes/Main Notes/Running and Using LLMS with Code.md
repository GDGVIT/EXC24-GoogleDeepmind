
28-10-2024 21:54

Status:

Tags: [[Ai]] [[LLM-LLVM]] [[Gemini]] [[google]] 


# Gemini api

The Gemini api is an api provided by google that enables you to use Gemini's ai text generation and querying capabilities within your project. There are plenty of other alternatives available as well which will be discussed later

# Gemini Challenge on Kaggle
[Google-Gemini long context](https://www.kaggle.com/competitions/gemini-long-context/overview) 
A competition held on kaggle with a prize pool of a $100k that asks participants to test out Gemini's exceptionally large context window

### Description

Gemini 1.5 introduced a major breakthrough in AI with its notably large context window. It can process up to 2 million tokens at once vs. the typical 32,000 - 128,000 tokens. This is equivalent to being able to remember roughly 100,000 lines of code, 10 years of text messages, or 16 average English novels.

With large context windows, methods like vector databases and [RAG](https://arxiv.org/pdf/2005.11401) (that were built to overcome short context windows) become less important, and more direct methods such as [in-context retrieval](https://arxiv.org/pdf/2406.13121) become viable instead. Likewise, methods like [many-shot prompting](https://arxiv.org/pdf/2404.11018) where models are provided with hundreds or thousands of examples of a task as either a replacement or a supplement for fine-tuning also become possible.

In [initial tests](https://arxiv.org/pdf/2403.05530), the Google Deepmind team saw very promising results, with state-of-the-art performance in long-document QA, long-video QA, and long-context ASR. They [shared an entire code base](https://blog.google/technology/ai/long-context-window-ai-models/) with Gemini 1.5 and had it successfully create documentation. They also had the model "watch" the film Sherlock JR from 1924, and it answered questions correctly.

This competition challenges you to stress test Gemini 1.5’s long context window by building public Kaggle Notebooks and YouTube Videos that demonstrate creative use cases. 

# How to use the Gemini api
[Gemini API Documentation](https://ai.google.dev/gemini-api/docs)
## Install the Gemini API SDK

The Python SDK for the Gemini API is contained in the [`google-generativeai`](https://pypi.org/project/google-generativeai/) package. Install the dependency using pip:

```
pip install -q -U google-generativeai
```


```python
import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")
model = genai.GenerativeModel("gemini-1.5-flash")
response = model.generate_content("Explain how AI works")
print(response.text)
```

Details on how to get your API key are present [here](https://ai.google.dev/pricing#1_5flash)


# Other Notable Ways
- *Groq*: [Groq](https://groq.com/) uses LPUs (Language Processing Unit desgined for LLM inference tasks and are specially suited for the task unlike GPUs) thereby provided extremely fast inference.
  Try out Groq [here](https://console.groq.com/playground)
  After you get your api key
```python
  pip install groq
```

#### Performing a Chat Completion:

```python
import os

from groq import Groq

client = Groq(
    api_key=os.environ.get("GROQ_API_KEY"),
)

chat_completion = client.chat.completions.create(
    messages=[
        {
            "role": "user",
            "content": "Explain the importance of fast language models",
        }
    ],
    model="llama3-8b-8192",
)

print(chat_completion.choices[0].message.content)
```
The Models available on Groq are present [here](https://console.groq.com/docs/models) and more documentation is provided [here](https://console.groq.com/docs/text-chat) too

- [HuggingFace](https://huggingface.co/docs/huggingface_hub/v0.13.2/en/index) has an api as well not just for LLM Models but for pretty much every type of Model you can imagine, also checkout [huggingface hub](https://huggingface.co/models) where you can get to download and use all sorts of AI models and datasets of varying sizes to meet your needs


# References