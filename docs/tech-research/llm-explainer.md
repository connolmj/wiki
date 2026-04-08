---
tags: [ai, llm, machine-learning, transformers, open-source, neural-networks]
date: 2026-04-07
---

# Understanding Large Language Models (LLMs)

## Summary

Large Language Models are a class of AI system that has rapidly moved from research curiosity to mainstream tool. This article covers what they are, how they work under the hood, the key architectural innovations that made them possible, where the technology may plateau, and how to get started running open source models locally.

---

## What is an LLM?

A **Large Language Model** is a neural network trained to understand and generate human language. It learns statistical patterns from enormous amounts of text — essentially a large chunk of the internet, books, code, and more — and becomes very good at predicting what text should come next given some input.

That sounds simple, but the emergent capabilities that arise from doing this at sufficient scale are remarkable: coherent reasoning, code generation, translation, summarization, and general-purpose question answering all emerge without being explicitly programmed.

---

## Neural Networks vs. LLMs

A **neural network** is the broad category. It is any computational system loosely inspired by the brain, made of layers of nodes (neurons) connected by weighted edges. Neural networks are used everywhere — recognizing faces in photos, detecting fraud in transactions, controlling self-driving cars, predicting protein structures.

An **LLM is a specific type of neural network** — one built with the Transformer architecture, trained on text at massive scale. Every LLM is a neural network, but most neural networks are not LLMs.

| Type | Specialized For | Examples |
|---|---|---|
| Convolutional Neural Network (CNN) | Images | Image classifiers, object detection |
| Recurrent Neural Network (RNN) | Sequences (older approach) | Early language models |
| Transformer | Sequences with self-attention | GPT-4, Claude, Gemini |
| LLM | Language (large Transformer) | ChatGPT, Claude, Llama |
| Vision Transformer | Images via Transformer architecture | Google ViT |

> **Note:** The term "neural network" is somewhat misleading marketing from the 1980s. These systems don't work like biological neurons — they are mathematical functions trained by gradient descent. The brain analogy breaks down quickly at the implementation level.

---

## How LLMs Work

### Weights

A neural network is a giant mathematical function. It takes in numbers (text converted to numbers) and transforms them through many layers of simple math — mostly multiplication and addition — to produce an output.

**Weights** are the numbers inside those multiplications. A simplified analogy: if you wanted to predict house prices from square footage and bedrooms, you might write:

```
price = (w1 × sqft) + (w2 × bedrooms)
```

`w1` and `w2` are weights. During training, you adjust them until predictions match reality. An LLM does exactly this, just with 100 billion+ weights instead of 2, and the inputs are words instead of house features.

The critical insight is that **you don't hand-code the weights**. You start with random numbers and let the training process figure out what they should be through an algorithm called **backpropagation**: after a wrong prediction, the model calculates how much each weight contributed to the error and nudges every weight slightly in the direction that reduces it. Done billions of times across trillions of examples, the weights gradually encode useful representations of grammar, facts, and reasoning — entirely on their own.

### Why GPUs?

The core operation in a neural network is **matrix multiplication** — multiplying large grids of numbers together. This is computationally intensive but highly parallel: each output value can be computed independently of every other.

- A **CPU** has a few powerful cores optimized for sequential, complex tasks (running an OS, a web browser)
- A **GPU** was originally built for rendering video games — computing millions of pixel colors simultaneously — and has tens of thousands of small cores that can all do arithmetic at once

Because matrix multiplication is embarrassingly parallel, GPUs are orders of magnitude faster than CPUs for this workload. Training GPT-3 required thousands of GPUs running for months — it would have been essentially impossible on CPUs.

---

## The Transformer Architecture

### The Problem It Solved

Before Transformers (2017), the dominant approach was **Recurrent Neural Networks (RNNs)**, which processed text sequentially — one word at a time, left to right — carrying a "memory" of what came before. Two major problems:

1. By word 200, the memory of word 1 had largely faded (the "vanishing gradient problem")
2. Sequential processing can't be parallelized — each step depends on the previous one

### Self-Attention

The Transformer scraps sequential processing entirely. It looks at the entire sequence at once using a mechanism called **self-attention**.

For each word, the model generates three vectors:
- **Query** — "What am I looking for?"
- **Key** — "What do I contain?"
- **Value** — "What information should I pass along?"

To compute attention for a word, its Query is compared (via dot product) against every other word's Key. High similarity = high attention weight. The model then takes a weighted sum of all Value vectors, producing a new context-enriched representation of that word.

**Example:** In "The trophy didn't fit in the bag because *it* was too big" — when computing the representation of "it," the attention mechanism learns to point strongly toward "trophy," resolving the reference automatically. This happens in parallel for every word simultaneously.

### Full Architecture

A complete Transformer stacks many attention layers (frontier models have 100+), interleaved with **feed-forward layers** that let each position further process its context-enriched representation. The feed-forward layers are thought to be where much factual knowledge is stored.

Since attention has no inherent sense of word order, **positional encoding** adds a position signal to each word so the model knows word 5 comes before word 6.

### The ChatGPT Moment: RLHF

Raw next-token prediction produces a model that completes text in an uncontrolled way. The breakthrough that turned LLMs into useful assistants was **Reinforcement Learning from Human Feedback (RLHF)**:

1. Start with a pretrained base model
2. Have human trainers write examples of ideal responses (supervised fine-tuning)
3. Have humans compare pairs of outputs and rank which is better
4. Train a "reward model" to predict human preferences from those rankings
5. Use reinforcement learning to fine-tune the main model to maximize that reward

This teaches the model to be helpful, follow instructions, and communicate naturally. OpenAI applied this to GPT-3.5 and launched ChatGPT in November 2022 — it reached 100 million users in two months, the fastest consumer product adoption in history.

---

## How Models Keep Getting Better

| Lever | What It Means |
|---|---|
| **Scale** | More parameters + more data + more compute = better performance, following predictable "scaling laws" |
| **Data quality** | Curated, deduplicated, filtered datasets outperform raw internet text disproportionately |
| **RLHF refinement** | Ongoing improvements in alignment training (Constitutional AI, DPO, etc.) |
| **Longer context** | Early models saw ~4,000 tokens; frontier models now handle 1,000,000+ |
| **Chain-of-thought** | Training models to reason step-by-step before answering dramatically improves complex task performance |
| **Mixture of Experts (MoE)** | Routes each token through only a subset of "expert" sub-networks — bigger model at lower compute cost |

---

## Where Could Progress Plateau?

### Data Limits

High-quality text on the internet is finite — estimated in the tens of trillions of tokens. Frontier models have consumed a very large fraction of it. Compounding this:

- **Model collapse** — the web is increasingly filled with AI-generated text; training on it amplifies errors and reduces diversity, like a photocopy of a photocopy
- **Tacit knowledge gap** — most human expertise (surgical intuition, engineering judgment) is never written down
- **Dataset skew** — training data is heavily weighted toward English and certain domains; rare languages and specialized fields have thin coverage

**Synthetic data** (using existing models to generate training data for future models) is a workaround but risks model collapse if not done carefully.

### Reasoning Ceilings

There is a serious open question about whether next-token prediction — even at massive scale — can produce genuine systematic reasoning. Tasks requiring truly novel chains of logic (not pattern-matched from training data) remain difficult. This may require architectural changes, not just more scale.

### Other Plateau Scenarios

- **Evaluation collapse** — benchmarks keep getting saturated; it becomes hard to tell if models are genuinely improving at things that matter
- **Jagged capability frontier** — models improve unevenly across skills; some capabilities may hit local ceilings while others continue improving
- **Inference economics** — bigger models cost more to run; at some point economics constrain how large models can practically get
- **Alignment difficulty** — as capability increases, reliably aligning models with human intent becomes harder and more consequential

### What Could Break Scaling Laws Entirely?

- The training objective (next-token prediction) may be the wrong one for general intelligence
- Physical compute limits — frontier training runs already cost hundreds of millions of dollars; the next generation may cost billions, running into chip manufacturing and energy constraints
- A paradigm shift — a new architecture could replace the Transformer the way Transformers replaced RNNs, resetting the scaling curves

---

## Types of AI Models

| Category | Input → Output | Examples |
|---|---|---|
| LLM (text) | Text → Text | GPT-4, Claude, Llama |
| Image generation | Text → Image | Stable Diffusion, DALL-E, Midjourney |
| Image understanding | Image → Text | Vision Transformers |
| Audio / speech | Audio → Text or Text → Audio | Whisper, ElevenLabs |
| Music generation | Text → Audio | MusicGen, Suno |
| Code models | Text → Code | GitHub Copilot, CodeLlama |
| Embedding models | Text → Vector | Sentence-BERT, text-embedding-ada |
| Multimodal | Text + Image + Audio → Any | GPT-4o, Claude, Gemini |

---

## Open Source Models

### What is Hugging Face?

[Hugging Face](https://huggingface.co) is the GitHub of AI models — a platform where researchers and companies publish trained models, datasets, and demo apps. Over 500,000 models are hosted there. It also provides a Python library (`transformers`) that makes it straightforward to download and run these models in code.

### When to Use Open Source vs. Closed API

| Use Open Source When | Use Closed API When |
|---|---|
| Privacy matters — data stays on your machine | Maximum capability is needed |
| Running at scale (per-token API costs add up) | You want managed infrastructure |
| You want to fine-tune on your own data | You're prototyping quickly |
| Offline operation is required | The task requires nuanced judgment |
| Transparency about training data matters | |

The gap between open and closed models has narrowed significantly. The best open models are now competitive for many everyday tasks.

### Key Open Source Models

| Model | Creator | Strengths | Sizes |
|---|---|---|---|
| **Llama 3.x** | Meta | General purpose, widely supported | 1B – 405B |
| **Mistral / Mixtral** | Mistral AI | Very efficient at smaller sizes, MoE variant | 7B – 8x22B |
| **DeepSeek R1** | DeepSeek | Reasoning, matched o1 at fraction of cost | Various |
| **Qwen 2.5** | Alibaba | Multilingual, strong coding | 0.5B – 72B |
| **Phi-3 / Phi-4** | Microsoft | Optimized for edge/local devices | 3B – 14B |
| **Gemma 2** | Google | Strong small-size performance | 2B – 27B |
| **CodeLlama** | Meta | Code generation | 7B – 34B |
| **Stable Diffusion / FLUX** | Stability AI / Black Forest Labs | Image generation | — |

---

## Running Models Locally

### User Interfaces

| Tool | Type | Best For |
|---|---|---|
| **Ollama** | CLI + local API | Easy terminal-based setup, powers other UIs |
| **Open WebUI** | Web browser UI | ChatGPT-like interface connecting to Ollama |
| **LM Studio** | Desktop app | Beginner-friendly, no terminal needed |
| **Hugging Face Spaces** | Browser (hosted) | Trying models without installing anything |
| **AUTOMATIC1111 / ComfyUI** | Web browser UI | Stable Diffusion image generation |

### Apple Silicon (M1 Pro, 16GB RAM)

Apple Silicon Macs are well-suited for local model inference due to their **unified memory architecture** — unlike a traditional PC where the GPU has separate VRAM (typically 8-16GB), the M1 Pro shares all 16GB between CPU and GPU, making the full amount available for model inference.

**Quantization** makes this practical: model weights are compressed from 32-bit floats down to 4-bit integers, cutting memory requirements by ~8x with modest quality loss.

| Model | Quantized Size | Performance on M1 Pro 16GB |
|---|---|---|
| Llama 3.2 3B (4-bit) | ~2GB | Fast, good for everyday tasks |
| Llama 3 8B (4-bit) | ~4.5GB | Good balance of speed and quality |
| Mistral 7B (4-bit) | ~4.5GB | Excellent for the size |
| Phi-3 Mini (4-bit) | ~2.5GB | Optimized for exactly this hardware class |
| Llama 3 13B (4-bit) | ~8GB | Possible but slow, little RAM headroom |
| Stable Diffusion 1.5 | ~2GB | Image generation, runs well |

### Getting Started on Mac

```bash
# 1. Install Ollama (ollama.com)
brew install ollama

# 2. Pull and run a model
ollama run llama3.2

# 3. Or install LM Studio for a GUI — no terminal needed
# Download from lmstudio.ai
```

For a full ChatGPT-like UI, install **Open WebUI** alongside Ollama — it connects automatically and gives you a polished browser-based interface, conversation history, and model switching.

---

## Related Research

- [Curve Finance](../crypto-defi/curve-finance.md)

---

*Published: April 7, 2026*
