# How LLMs Actually Generate Text (Every Dev Should Know This)

This document summarizes the internal pipeline that Large Language Models (LLMs) such as ChatGPT and Claude use to process and generate language.

---

## 1. Tokenization

### What it does
Breaks down input text into smaller pieces called **tokens**.

### Why it matters
LLMs read tokens instead of raw words. Token counts directly affect:

- Processing efficiency
- Context window usage
- API costs

### Example

Input:

```
I love programming.
```

Possible tokens:

```
["I", " love", " programming", "."]
```

---

## 2. Embeddings

### What it does
Maps text tokens into numerical vectors.

### Why it matters
Embeddings create a high-dimensional "meaning space" where semantically related concepts are positioned near each other.

For example:

- King ≈ Queen
- Cat ≈ Dog
- Paris ≈ France

Concepts with similar meanings tend to cluster together.

---

## 3. The Transformer & Attention

### What it does
Processes token embeddings through multiple neural network layers.

### Why it matters
The attention mechanism allows the model to determine which words and concepts are most relevant when interpreting context.

### Example

Sentence:

> The trophy didn't fit into the suitcase because it was too big.

Attention helps determine that:

> "it" refers to the trophy.

rather than the suitcase.

This ability to dynamically weight relationships is one of the key breakthroughs behind modern LLMs.

---

## 4. Probability Distributions

### What it does
Evaluates possible next tokens and assigns probabilities to them.

### Why it matters
The model does not know the next word with certainty.

Instead, it generates a probability distribution over its vocabulary.

### Example

Input:

```
The capital of France is
```

Possible outputs:

| Token | Probability |
|---------|------------|
| Paris | 98% |
| Lyon | 1% |
| London | 0.5% |
| Berlin | 0.2% |

The model predicts likelihoods rather than certainties.

---

## 5. Sampling

### What it does
Selects the final output token from the probability distribution.

### Why it matters
Sampling parameters influence creativity, randomness, and predictability.

Common controls include:

### Temperature

- Low temperature → deterministic and focused
- High temperature → creative and diverse

Examples:

- Temperature 0.1 → highly predictable
- Temperature 1.0 → balanced
- Temperature 1.5 → more random

### Top-p (Nucleus Sampling)

Limits selection to the smallest group of tokens whose cumulative probability exceeds a threshold.

Examples:

- Top-p = 0.9
- Top-p = 0.95

This helps maintain coherence while allowing variation.

---

# Complete Pipeline

```

User Input
    ↓
Tokenization
    ↓
Embeddings
    ↓
Transformer + Attention
    ↓
Probability Distribution
    ↓
Sampling
    ↓
Generated Token
    ↓
Repeat Until Completion

```

---

# Key Takeaway

LLMs do not retrieve fully formed answers from memory.

Instead they repeatedly:

1. Convert text into tokens
2. Represent tokens as vectors
3. Process context using attention
4. Predict probability distributions
5. Sample the next token

This cycle repeats token-by-token until a complete response is generated.

---

## Sources

Video:
https://www.youtube.com/watch?v=NKnZYvZA7w4

Article:
https://medium.com/@iam-abdulmoiz/how-llms-work-tokens-embeddings-and-transformers-a54d0468b42e
