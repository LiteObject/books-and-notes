# The Attention Mechanism

## The Problem: Why Do We Need Attention?

Before attention was introduced, sequence-to-sequence (seq2seq) models used an **encoder-decoder** architecture. The encoder would read an entire input sequence (e.g., a sentence in English) and compress it into a single fixed-length vector. The decoder would then use that vector to generate the output sequence (e.g., a sentence in French).

The problem? Cramming an entire sentence into one fixed-size vector is like trying to summarize an entire book onto a sticky note. For short sentences it works fine, but for longer sequences, important information gets lost. This is known as the **information bottleneck**.

## What Is the Attention Mechanism?

Attention is a technique that allows a model to **focus on different parts of the input** when producing each part of the output, rather than relying on a single compressed representation.

Think of it like reading a textbook and highlighting the most relevant sentences for each question on an exam. You don't re-read the entire book for every question — you selectively attend to the parts that matter.

The core idea was introduced by **Bahdanau et al. (2014)** in the paper *"Neural Machine Translation by Jointly Learning to Align and Translate."*

## The Core Idea: Queries, Keys, and Values

The attention mechanism is built around three concepts:

| Concept   | Analogy                                                       |
|-----------|---------------------------------------------------------------|
| **Query** | The question you're asking ("What word should I translate next?") |
| **Key**   | A label on each piece of information ("Here's what I contain")  |
| **Value** | The actual information stored at that location                  |

**How it works step by step:**

1. You have a **query** (what you're looking for).
2. You compare the query against every **key** to compute a **relevance score** (how related each piece of input is to your current question).
3. You use those scores as weights to take a **weighted sum of the values**.
4. The result is a context vector that emphasizes the most relevant information.

This is conceptually similar to a database lookup, except instead of returning one exact match, attention returns a soft, weighted blend of all entries.

## Scaled Dot-Product Attention

The most common form of attention, introduced in the **Transformer** architecture (Vaswani et al., 2017), is **scaled dot-product attention**.

Given matrices **Q** (queries), **K** (keys), and **V** (values):

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

Where:
- **QK^T** computes the dot product between each query and each key, producing a matrix of raw relevance scores.
- **√d_k** is the square root of the key dimension. This scaling factor prevents the dot products from becoming too large, which would push the softmax into regions with extremely small gradients (making learning difficult).
- **softmax** converts the scores into a probability distribution (all positive, summing to 1), so they can serve as weights.
- Multiplying by **V** produces the weighted combination of values.

## Self-Attention

In **self-attention** (also called intra-attention), the queries, keys, and values all come from the **same sequence**. Each element in the sequence attends to every other element (including itself) to build a richer, context-aware representation.

**Example:** Consider the sentence *"The cat sat on the mat because it was tired."*

When processing the word *"it"*, self-attention helps the model figure out that *"it"* refers to *"cat"* and not *"mat"* by assigning a higher attention weight to *"cat"*.

Self-attention is what gives Transformer models their power — every token can directly interact with every other token, regardless of distance. This solves the long-range dependency problem that plagued recurrent neural networks (RNNs).

## Multi-Head Attention

Instead of computing attention once, **multi-head attention** runs multiple attention operations in parallel, each with different learned projection matrices.

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \text{head}_2, \ldots, \text{head}_h)W^O
$$

Where each head is:

$$
\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)
$$

**Why multiple heads?**

Each head can learn to attend to different types of relationships:
- One head might focus on **syntactic relationships** (subject-verb agreement).
- Another might capture **positional proximity** (nearby words).
- Another might learn **semantic similarity** (words with related meanings).

The outputs of all heads are concatenated and linearly transformed, combining these diverse perspectives into a single representation.

## Positional Encoding

Self-attention treats the input as a **set**, not a sequence — it has no inherent notion of word order. The sentence "dog bites man" and "man bites dog" would look the same to pure self-attention.

To fix this, Transformers add **positional encodings** to the input embeddings. The original Transformer uses sinusoidal functions:

$$
PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d}}\right)
$$

$$
PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d}}\right)
$$

Where *pos* is the position in the sequence and *i* is the dimension index. These encodings allow the model to distinguish between tokens at different positions and generalize to sequence lengths not seen during training.

Modern models often use **learned positional embeddings** or **rotary positional embeddings (RoPE)** instead.

## Types of Attention in Transformers

| Type              | Description                                                       | Used In             |
|-------------------|-------------------------------------------------------------------|----------------------|
| **Self-Attention**    | Each token attends to all tokens in the same sequence             | Encoder and Decoder  |
| **Cross-Attention**   | Tokens in one sequence attend to tokens in another sequence       | Decoder (attending to encoder output) |
| **Causal (Masked) Attention** | Each token can only attend to itself and previous tokens (future tokens are masked) | Decoder (autoregressive generation) |

**Causal masking** is essential for autoregressive models like GPT. During generation, the model predicts one token at a time and should not be able to "see" future tokens. This is implemented by setting the attention scores for future positions to negative infinity before the softmax, effectively zeroing out those weights.

## The Transformer Architecture

The **Transformer** (Vaswani et al., 2017, *"Attention Is All You Need"*) replaced recurrence entirely with attention. It consists of:

**Encoder** (stack of N identical layers):
1. Multi-head self-attention
2. Feed-forward neural network
3. Residual connections and layer normalization around each sub-layer

**Decoder** (stack of N identical layers):
1. Masked multi-head self-attention
2. Multi-head cross-attention (over encoder output)
3. Feed-forward neural network
4. Residual connections and layer normalization around each sub-layer

Popular model families and which parts they use:

| Model Family | Architecture     | Examples               |
|-------------|------------------|------------------------|
| Encoder-only | Encoder          | BERT, RoBERTa          |
| Decoder-only | Decoder          | GPT, LLaMA, Claude     |
| Encoder-Decoder | Both          | T5, BART, original Transformer |

## Attention Complexity and Efficiency

Standard self-attention has **O(n²)** time and memory complexity with respect to sequence length *n*, because every token computes a score with every other token.

For long sequences, this becomes expensive. Several approaches have been proposed to address this:

- **Sparse Attention** — Only attend to a subset of positions (e.g., local windows + global tokens). Used in Longformer, BigBird.
- **Linear Attention** — Approximate the softmax attention with kernel methods to reduce complexity to O(n).
- **Flash Attention** — An exact algorithm that reduces memory I/O by restructuring the computation to be more hardware-aware, without changing the mathematical result.
- **Grouped Query Attention (GQA)** — Shares key-value heads across multiple query heads to reduce memory usage during inference. Used in LLaMA 2 and later models.

## Key Takeaways

1. **Attention lets models focus** on relevant parts of the input instead of relying on a fixed-size bottleneck.
2. **Self-attention** enables every token to interact with every other token, capturing long-range dependencies.
3. **Multi-head attention** allows the model to capture multiple types of relationships simultaneously.
4. **Scaled dot-product** is the standard attention computation: compare queries with keys, then use the scores to weight values.
5. **Positional encoding** is necessary because attention has no built-in sense of order.
6. **The Transformer** is built entirely on attention and has become the foundation for modern language models, vision models, and beyond.

## References

- Bahdanau, D., Cho, K., & Bengio, Y. (2014). *Neural Machine Translation by Jointly Learning to Align and Translate.* arXiv:1409.0473.
- Vaswani, A., et al. (2017). *Attention Is All You Need.* arXiv:1706.03762.
- Dao, T., et al. (2022). *FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness.* arXiv:2205.14135.
