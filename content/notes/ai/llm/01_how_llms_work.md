---
title: "How llms work"
date: 2026-03-01
type: page
---

# 🧠 How Large Language Models (LLMs) Work Internally

> Beginner → Intermediate → Advanced  
> With examples, concepts, and real-world use cases

---

## 🟢 Beginner Level — Core Idea

### What is an LLM?
A **Large Language Model (LLM)** is a system that predicts the **next token** based on previous tokens.

📌 It does **not think or know facts** like humans.  
📌 It learns **patterns from massive text data**.

**Example**
Input: "The capital of France is"
Output: "Paris"


---

## 🔹 Tokens (Not Words)

LLMs process **tokens**, not words.

**Example**
"ChatGPT is amazing"
→ ["Chat", "G", "PT", " is", " amazing"]


Tokens can be:
- Full words
- Word parts
- Punctuation

📌 Tokens matter because:
- Cost is calculated per token
- Context window is measured in tokens

---

## 🔁 How Prediction Works (Simple Loop)

1. Input text → tokens
2. Model predicts next token
3. Token is added to input
4. Repeat until completion

**Example**


User: "Hello, how are"
Model: predicts " you"


---

## 🧩 Beginner Use Cases

- Chatbots
- Auto-complete
- Text summarization
- FAQ assistants

---

## 🟡 Intermediate Level — How LLMs Understand Context

---

## 🔹 Embeddings (Meaning as Numbers)

LLMs convert tokens into **vectors (numbers)** called embeddings.

**Example**
"King" → [0.21, 0.88, -0.15, ...]
"Queen" → [0.22, 0.87, -0.14, ...]


✔ Similar meanings → closer vectors  
✔ Enables semantic search & similarity

---

## 🔹 Transformer Architecture

LLMs are built using **Transformers**.

A transformer consists of:
- Multiple layers
- Self-attention
- Feed-forward networks

Each layer improves understanding step by step.

---

## 🔹 Attention (Most Important Concept)

Attention helps the model decide:
> **Which words matter most** in a sentence

**Example**
"The trophy doesn't fit in the suitcase because it is too small."
"it" refers to **suitcase**, not trophy.
This is solved using **self-attention**.

---

## 🔹 Self-Attention Example

Sentence:
"I love programming because it is creative"
"it" attends to **programming**, not "love".

---

## 🔹 Training (Simplified)

During training:
1. Hide a word
2. Model predicts it
3. Compare with actual word
4. Adjust weights

This happens **trillions of times**.

---

## 🧩 Intermediate Use Cases

- Code generation
- Search engines
- Chat with documents
- Sentiment analysis

---

## 🔵 Advanced Level — Production & Internals

---

## 🔹 Pretraining vs Fine-Tuning

### Pretraining
- Learns language from large datasets
- Objective: predict next token

### Fine-Tuning
- Teaches behavior and alignment
- Instruction following
- Safety rules

---

## 🔹 RLHF (Reinforcement Learning from Human Feedback)

1. Humans rank model outputs
2. Model learns preferred responses
3. Reward helpful & safe answers

This is why ChatGPT feels **human-like**.

---

## 🔹 Inference (When You Use the Model)

Pipeline:


Prompt
→ Tokens
→ Embeddings
→ Transformer Layers
→ Token Probabilities
→ Sampling
→ Output


**Example probabilities:**
Paris: 0.92
London: 0.04
Berlin: 0.02


---

## 🔹 Sampling Controls

### Temperature
- `0.0` → deterministic
- `1.0+` → creative

### Top-K
- Choose from top K tokens

### Top-P (Nucleus Sampling)
- Choose smallest token set whose probability ≥ P

---

## 🔹 Context Window (Memory Limit)

LLMs have **no long-term memory**.

They only remember:
- What fits inside the context window

Example:
- GPT-4 → up to ~128k tokens
- LLaMA 3 → 8k–128k (variant)

---

## 🔹 Hallucinations (Why They Happen)

LLMs:
- Predict likely text
- Do NOT verify truth

If knowledge is missing → model guesses confidently.

### Solutions
- RAG (Retrieval-Augmented Generation)
- Tool calling
- Validation layers

---

## 🧩 Advanced Use Cases

- Agentic AI systems
- DevOps automation
- ITSM assistants
- Code review bots
- Autonomous workflows

---

## 🔄 Real-World Architecture Example (RAG)

**Chat with PDF**
1. Split PDF into chunks
2. Convert chunks to embeddings
3. Store in vector database
4. Embed user query
5. Retrieve similar chunks
6. Inject into prompt
7. LLM generates response

---

## 🧠 Key Takeaways

- LLMs predict text, not facts
- Tokens ≠ words
- Embeddings represent meaning
- Attention provides context understanding
- RAG reduces hallucinations
- Agents combine reasoning + actions

---

## 🚀 What to Learn Next

1. Transformers (visual explanations)
2. Embeddings & vector databases
3. RAG pipelines
4. Agent frameworks (LangGraph, CrewAI)
5. Model serving (FastAPI, Docker)

---

📌 **One-line Summary**  
LLMs are transformer-based systems that predict tokens using attention and embeddings, and become powerful when com