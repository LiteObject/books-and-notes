# Attention Mechanism in LLMs

The **attention mechanism** is one of the most important ideas behind modern Large Language Models (LLMs).

In simple terms:

> Attention helps the model decide which other tokens in the text are important for each token.

A token can be a word, part of a word, punctuation mark, or symbol. LLMs do not read text exactly like humans do. They first break text into tokens, turn those tokens into numbers, and then use attention to understand how the tokens relate to each other.

## Why Attention Is Needed

Language depends on context.

For example:

```text
I deposited money in the bank.
I sat near the river bank.
```

The word `bank` means different things in these two sentences. The model needs surrounding words to understand the correct meaning.

Attention helps the model look at the relevant parts of the text and decide what matters most.

## Simple Example

Consider this sentence:

```text
Sarah dropped the glass because she was nervous.
```

When the model reads the word `she`, it needs to figure out who `she` refers to.

Attention helps the model connect:

```text
she -> Sarah
```

The model may give more attention to `Sarah` than to `glass`, because `Sarah` is the person who was nervous.

## Self-Attention

The type of attention used in transformers is usually called **self-attention**.

"Self" means the tokens in the same piece of text look at each other.

In this sentence:

```text
The dog chased the ball because it was excited.
```

The token `it` may pay strong attention to `dog`, because `it` probably refers to the dog.

Each token builds a better meaning for itself by looking at other useful tokens.

## Queries, Keys, and Values

Attention uses three main ideas:

- **Query**
- **Key**
- **Value**

A simple way to understand them:

- **Query** means: "What am I looking for?"
- **Key** means: "What information do I contain?"
- **Value** means: "What information should I pass along?"

For example, the token `she` may have a query like:

```text
Which token tells me who this pronoun refers to?
```

The token `Sarah` may have a key that matches this query well. So `she` pays strong attention to `Sarah`.

Then the value from `Sarah` helps update the meaning of `she`.

## Attention Scores

The model compares queries and keys to calculate **attention scores**.

A high score means: "This token is important for this attention calculation."
A low score means: "This token is less important for this attention calculation."

For example:

```text
she attends to:
  Sarah:    70%
  glass:     5%
  dropped:  10%
  nervous:  15%
```

These percentages are not written by humans. The model learns them during training.

In a real LLM, attention weights are calculated separately for each attention head and each layer. So there is not one single attention percentage for the whole model.

## Softmax

After attention scores are calculated, the model uses a function called **softmax**.

Softmax turns raw scores into weights that add up to 1, like percentages.

This helps the model decide how much information to take from each token.

## Scaled Dot-Product Attention

The common attention formula is:

$$
\operatorname{Attention}(Q, K, V) = \operatorname{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right) V
$$

In plain language:

1. Compare queries with keys.
2. Scale the scores by the key-vector dimension, `d_k`, so they are stable.
3. Convert scores into weights using softmax.
4. Use those weights to combine values.

The important idea is:

> Attention computes which tokens should influence each other, and by how much.

## Multi-Head Attention

LLMs do not use only one attention pattern. They use **multi-head attention**.

A "head" is like one way of looking at the text.

Different heads can sometimes learn to focus on different patterns:

- grammar
- pronouns
- nearby words
- long-distance relationships
- code structure
- topic changes

For example, in code:

```python
result = calculate_total(items)
print(result)
```

One attention head may connect `result` in `print(result)` back to the earlier assignment.

Another head may focus on the function call.

Multi-head attention lets the model process text from many angles at once.

This is a useful simplification. In real models, attention heads are not always easy to label, and some heads may overlap or be less important than others.

## Causal Attention

LLMs that generate text usually use **causal attention**.

This means the model can only look backward, not forward.

For example, when predicting the next token after:

```text
The capital of France is
```

the model can look at the words before the answer, but it cannot look at the answer itself.

This is important because LLMs generate text one token at a time.

A **causal mask** is used to block the model from seeing future tokens. This is usually done by making future-token scores extremely small before softmax.

## Positional Information

Attention alone does not know word order.

More precisely, self-attention by itself treats the same tokens similarly even if their order changes. It needs extra position information to tell where each token appears.

These two sentences use the same words but mean different things:

```text
dog bites man
man bites dog
```

So transformers add **positional information** to show where each token appears.

Common methods include:

- positional embeddings
- rotary position embeddings (RoPE)
- relative position techniques

This helps the model understand order and distance.

## Context Window

The **context window** is the amount of text the model can consider at one time.

If a model has a large context window, it can use attention over more tokens.

However, standard attention becomes expensive as the context gets longer, because many tokens need to be compared with many other tokens. The cost grows roughly with the square of the sequence length.

That is why long-context LLMs require special optimizations.

## KV Cache

During text generation, the model predicts one token at a time.

To avoid recalculating everything again and again, LLMs use a **KV cache**.

The KV cache stores previous keys and values.

When a new token is generated, the model creates a new query, key, and value for that token. It uses the new query to compare against the stored keys and values, then appends the new key and value to the cache.

This makes generation much faster.

## What Attention Helps With

Attention helps LLMs:

- understand context
- connect related words
- resolve pronouns
- follow grammar
- track long-distance relationships
- understand code structure
- focus on relevant parts of a prompt
- predict the next token more accurately

## What Attention Is Not

Attention is powerful, but it is not magic.

Attention is not the same as:

- human understanding
- perfect reasoning
- permanent memory
- a database lookup
- guaranteed truth

It is a learned mathematical method for moving information between tokens.

Attention scores can sometimes give clues about what the model is focusing on, but they do not always fully explain the model's behavior.

## Short Summary

The attention mechanism lets an LLM compute which tokens in the context are most important for understanding each token.

It works by comparing queries and keys, creating attention weights, and using those weights to combine values.

In one sentence:

> Attention is how an LLM computes what parts of the text to focus on while understanding language and predicting what comes next.
