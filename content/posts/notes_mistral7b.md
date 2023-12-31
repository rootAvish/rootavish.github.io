---
title: "Notes on \"Mistral 7B’s secrets\" talk by Lample"
date: 2023-12-17T14:17:18+05:30
draft: false
categories:
  - llm
  - deep-learning
  - nlu
tags:
  - notes
  - llm
  - talks
  - deep-learning
  - LLaMA
  - mistal
---

*Note: The talk turned out to actually be about LLaMA*


Link: https://www.youtube.com/watch?v=cWUNlw-5TPs

**Introduction**

* Basically impossible to do anything without LLMs in NLP these days.
* They were trying to solve mathematical problems (GSM8k?)
* Codex was available at the time however API was a limiting factor. (Also imagine like any other competent
ML guy they had ideas on what can be improved).

**Deciding on the Size of the LLM**

* Chinchilla was out
* 70B - 1.4T tokens, scaling laws.
* decide based on compute budget
* Trained 5 times longer (GPT3 - 300B tokens)
* Fater inference and more stable training are obvious advantages of small LMs.
* LLaMA was born
    - 7B (1T tokens)
    - 13B (1T tokens)
    - 33B (1.4T tokens)
    - 65B (1.4T tokens)

**Tokenisation**

* basic well known things
* References an original paper on subword tokenisation - NMT of rare words with subword units (Sennrich et al 2015)

**Finding so many tokens**

* Wikipedia - 6B tokens in English, 3B French, top 20 - 27B (need 50 times more)
* ArXiv - 35B (2M pubs)
* StackExchange - 26B
* Github - 500B - 1T
* CommonCrawl - 240B pages (16 years)
    - very noisy
    - each webpage is html + processed version (text not well done)
    - reprocessed from raw html
    - deduplication
    - english only
    - line repetition
    - noise removal
    - hateful content
    - quality filter (no interesting content) - classifier
    - C4 by Google
    - CC by Meta
    - Books3
    - Gutenberg

**Architecture**

* Rotary embeddings
* SwiGLU instead of ReLU in feed-forward
* Initialise like PaLM?
* Chinchilla paper's arch, but they had extra tokens so could do 70B, these guys could do 65B
* LLaMA 7B - 500 GPUs for 1 week

**Training**

- Hardware failures
- Silently corrupts matrix multiplication output
- Spikes occur, loss diverging and then loss goes wide very quickly
- Manually re-implement gradient computations because at their scale data locality makes a lot of difference (model is sharded across multiple GPUs and for forward pass need to gather a bunch of data on one GPU).


**Evaluation**

* Till BERT you could finetune for some standard benchmarks.
* Now it's zero shot and few shot tasks.
* MMLU
* Noise of even 1-5% is a lot because that's the difference between 7B and 13B LLaMA.
* People don't believe your results because the implementations are different.
* For stability, use few shot evalution


**Their takeaways**

* Train much more than what's suggested in Chinchilla
* "Chinchilla trap"
* Overlooked inference cost in Chinchilla
* SOTA models only need publicly available data
* Conceptaully simple, engineering challenging

**LLaMA2**

* Now with 2T tokens.
* At 1.4T learning capacity was not saturated


**Training small and efficient models for businesses (7B)**

* 8k context
* 7B better than 13B LLaMA2 and 30B LLaMA1 on certain benchmarks
* sliding attention window kernels
* 128k token theoretical max.
