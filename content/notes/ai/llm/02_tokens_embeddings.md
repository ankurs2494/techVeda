---
title: "Tokens embeddings"
date: 2026-03-01
type: page
---

# 🔤 Token Embeddings Explained (Beginner → Advanced)

## 📌 What Are Tokens?

In Large Language Models (LLMs), **tokens** are the smallest units of text the model understands.

A token can be:

* A word → `"apple"`
* Part of a word → `"play" + "ing"`
* A number → `"2025"`
* A symbol → `"@"`, `"."`, `","`
* Even a space → `" "`

### Example

Sentence:

```
"I love AI"
```

Possible tokens:

```
["I", " love", " AI"]
```

> ⚠️ LLMs do NOT understand words directly — they understand **tokens**.

---

## 🧠 What Is an Embedding? (Beginner Level)

An **embedding** is a **numerical representation** of a token.

Think of embeddings as:

> 📍 A coordinate that places a word in a high-dimensional space based on meaning.

### Simple Analogy

Imagine a map:

* Paris 🇫🇷 and London 🇬🇧 are close
* Paris 🇫🇷 and Banana 🍌 are far apart

LLMs do the same — but in **hundreds or thousands of dimensions**.

### Example

Token → Vector:

```
"king" → [0.21, -0.33, 0.98, ..., 0.44]
```

These numbers **capture meaning**, not spelling.

---

## 🧩 Why Do We Need Embeddings?

Computers understand numbers, not text.

Embeddings allow models to:

* Understand **similarity**
* Capture **context**
* Perform **semantic search**
* Reason about meaning

### Example

```
"doctor" ≈ "physician"
"king" - "man" + "woman" ≈ "queen"
```

This works because of embeddings.

---

## 🔄 Token → Embedding Pipeline

```
Text
 ↓
Tokenization
 ↓
Token IDs
 ↓
Embedding Lookup
 ↓
Vectors
 ↓
Transformer Model
```

---

## 🧪 Intermediate Level: How Embeddings Are Created

### 1️⃣ Tokenization

Text is split using algorithms like:

* BPE (Byte Pair Encoding)
* WordPiece
* SentencePiece

Example:

```
"unbelievable" → ["un", "believ", "able"]
```

---

### 2️⃣ Embedding Matrix

The model maintains a huge table:

```
Embedding Matrix:
Token ID → Vector (size = 768 / 1024 / 4096)
```

Example:

```
Token ID 1023 → [0.12, 0.55, -0.91, ...]
```

Every token has **one learned vector**.

---

### 3️⃣ Positional Embeddings

Since transformers don’t understand order naturally, **position info** is added.

Example:

```
"I love AI"
```

The model adds:

```
Token Embedding + Position Embedding
```

So:

* `"AI"` at position 3 ≠ `"AI"` at position 1

---

## 🧠 Advanced Level: Contextual Embeddings

### Static vs Contextual Embeddings

| Type       | Example   | Meaning                      |
| ---------- | --------- | ---------------------------- |
| Static     | Word2Vec  | Same vector every time       |
| Contextual | GPT, BERT | Meaning changes with context |

### Example

```
"I sat on the bank"
"I deposited money in the bank"
```

➡️ The word **bank** gets different embeddings depending on context.

This happens because:

* Embeddings pass through **self-attention layers**
* Context reshapes meaning dynamically

---

## 🔍 Embeddings in Attention Mechanism

In transformers:

* Tokens interact via **Query, Key, Value (QKV)**
* Similar embeddings → stronger attention

This enables:

* Long-range dependencies
* Context awareness
* Reasoning

---

## 🗂️ Real-World Use Cases

### 1️⃣ Semantic Search

User query:

```
"How to reset password?"
```

Matches document:

```
"Account recovery steps"
```

Even without exact keywords.

Used in:

* Helpdesks
* Knowledge bases
* Search engines

---

### 2️⃣ Retrieval-Augmented Generation (RAG)

Flow:

```
Query → Embedding
Docs → Embeddings
Similarity Search
Relevant Docs → LLM
```

Used in:

* Chatbots
* Internal tools
* Compliance systems

---

### 3️⃣ Recommendation Systems

Embeddings represent:

* Users
* Products
* Content

Close vectors → better recommendations.

---

### 4️⃣ Clustering & Classification

Group:

* Similar tickets
* Similar incidents
* Similar customer issues

---

## ⚙️ Example: Python Embedding (Conceptual)

```python
from openai import OpenAI

client = OpenAI()

embedding = client.embeddings.create(
    model="text-embedding-3-large",
    input="How to reset password?"
)

vector = embedding.data[0].embedding
print(len(vector))  # e.g., 3072
```

---

## ⚠️ Important Notes

* Tokens cost money 💰 in API usage
* Longer text → more tokens → higher cost
* Chunking text improves accuracy
* Embedding dimension ≠ model size

---

## 📝 One-Line Summary

> **Token embeddings convert text into meaningful numerical vectors that allow LLMs to understand, compare, and reason about language.**

---

## 🧠 Memory Tip

* Tokens = text pieces
* Embeddings = meaning vectors
* Similar meaning = closer vectors
* Context reshapes embeddings

---

## 📌 Next Topics You Should Read

* Self-Attention Mechanism
* Transformer Architecture
* Vector Databases
* RAG Pipelines

---

⭐ End of Notes