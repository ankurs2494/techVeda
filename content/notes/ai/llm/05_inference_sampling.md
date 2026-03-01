---
title: "Inference sampling"
date: 2026-03-01
type: page
---

# 🎯 Inference & Sampling in Large Language Models (LLMs)

*(Beginner → Intermediate → Advanced)*

---

## 📌 What Is Inference?

**Inference** is the phase where a trained LLM is **used to generate output**.

> Training = learning
> Inference = answering

When you type a prompt into ChatGPT, Copilot, or any AI assistant, the model is performing **inference**.

---

## 🧒 Beginner Level: The Basics

### 🧠 What Happens During Inference?

1. You provide a prompt
2. The model converts text into tokens
3. It predicts the **next token**
4. The token is added to output
5. Steps 3–4 repeat until stopping

Example:

```
Prompt: "AI is"
Model predicts → "transforming"
Then → "the"
Then → "world"
```

---

## 🎲 What Is Sampling?

**Sampling** decides *how* the next token is chosen.

The model does **not always pick the highest-probability token**.
Instead, sampling adds **controlled randomness**.

Why?

* Prevent boring outputs
* Enable creativity
* Avoid repetitive text

---

## 🧠 Beginner Example

Without sampling:

```
AI is very very very very very...
```

With sampling:

```
AI is transforming industries in unexpected ways.
```

---

## 🧩 Intermediate Level: Sampling Techniques

The model produces **probabilities** for next tokens:

| Token   | Probability |
| ------- | ----------- |
| "good"  | 0.45        |
| "great" | 0.30        |
| "bad"   | 0.05        |
| others  | 0.20        |

Sampling controls **how we choose** from this list.

---

## 🔧 Key Sampling Parameters

### 1️⃣ Greedy Decoding

* Always pick the highest probability token
* Deterministic

Example:

```
Output is always the same
```

✔ Fast
❌ Repetitive, boring

---

### 2️⃣ Temperature 🌡️

Controls randomness.

| Temperature | Behavior      |
| ----------- | ------------- |
| 0.0         | Deterministic |
| 0.3         | Conservative  |
| 0.7         | Balanced      |
| 1.0+        | Creative      |

Example:

```
Low temp → factual answer
High temp → creative story
```

---

### 3️⃣ Top-K Sampling

* Only consider top **K** tokens
* Ignore the rest

Example:

```
Top-K = 5
Only top 5 probable tokens are sampled
```

✔ Limits nonsense
❌ Fixed cutoff

---

### 4️⃣ Top-P (Nucleus Sampling)

* Select smallest set of tokens whose probability sum ≥ P

Example:

```
Top-P = 0.9
Use tokens until total probability reaches 90%
```

✔ Adaptive
✔ Most commonly used

---

## 🔁 Typical Production Settings

| Use Case         | Temp | Top-P |
| ---------------- | ---- | ----- |
| Chatbot          | 0.7  | 0.9   |
| Code             | 0.1  | 0.95  |
| Creative writing | 1.0  | 0.95  |
| Summarization    | 0.3  | 0.9   |

---

## 🧠 Intermediate Example (Python-like)

```python
response = model.generate(
    prompt="Explain caching",
    temperature=0.4,
    top_p=0.9
)
```

---

## 🚀 Advanced Level: System & Optimization View

### 🔬 Why Sampling Matters

Without sampling:

* Model collapses into repetitive loops
* No creativity
* Poor user experience

With sampling:

* Better diversity
* Natural language flow
* Human-like responses

---

## ⚠️ Sampling Trade-offs

| Too Low    | Too High       |
| ---------- | -------------- |
| Boring     | Hallucinations |
| Repetitive | Inconsistent   |
| Safe       | Risky          |

---

## 🧠 Advanced Sampling Techniques

### 🔹 Repetition Penalty

Penalizes previously used tokens to avoid loops.

### 🔹 Presence Penalty

Encourages new topics.

### 🔹 Frequency Penalty

Reduces repeated phrases.

---

## 🧪 Real-World Use Cases

### 🔹 Chatbots

Balanced creativity + accuracy

### 🔹 Code Generation

Low temperature, high precision

### 🔹 Story Writing

High temperature, rich diversity

### 🔹 Agents (DevOps / ITSM)

Low randomness for reliability

---

## 🧠 Sampling vs Training

| Aspect          | Training | Inference       |
| --------------- | -------- | --------------- |
| Weights change? | Yes      | No              |
| Speed           | Slow     | Fast            |
| Cost            | High     | Pay-per-use     |
| Control         | Data     | Sampling params |

---

## 📝 One-Line Summary

> **Inference generates answers, sampling controls how creative, safe, or deterministic those answers are.**

---

## 🧠 Memory Trick

* **Inference** = Answering
* **Sampling** = Decision style
* **Temperature** = Creativity dial
* **Top-P** = Smart cutoff

---

## 📌 Next Topics to Learn

* Beam search
* Logit bias
* Streaming inference
* Token limits & truncation
* Cost optimization in inference

---

⭐ End of Notes