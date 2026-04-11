---
layout: post
title: "Understanding Attention Mechanisms: From Theory to Implementation"
date: 2024-08-10
category: "ML & Math"
---

Attention mechanisms are the foundation of transformers, but they can feel magical until you understand
the mechanics. Let's break it down from first principles.

## The Core Idea

**Intuition**: Instead of forcing the model to compress all information into a fixed-size context,
let each position "look" at other positions and decide what's relevant. This is attention.

**Mathematically**, attention answers: "For my current position, which other positions should I focus on?"

### The Attention Formula

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

Where:
- **Q (Query)**: "What am I looking for?" - What the current position is interested in
- **K (Key)**: "What do I represent?" - What each position offers
- **V (Value)**: "What should I actually pass along?" - The actual information

The process:
1. **Compute scores**: $QK^T$ - How much does the query match each key?
2. **Scale**: Divide by $\sqrt{d_k}$ to keep gradients stable (prevents saturation in softmax)
3. **Normalize**: Apply softmax to get attention weights (sum to 1)
4. **Weight values**: Multiply normalized weights by value vectors

## Why $\sqrt{d_k}$?

Without scaling, the dot product $QK^T$ can grow very large.

**Example**: If $d_k = 512$:
- Random vectors: $E[q_i \cdot k_i] = 0$, but $\text{Var}(q_i \cdot k_i) = 512$
- Unscaled dot product has huge variance → softmax becomes near one-hot → no gradient flow
- Dividing by $\sqrt{512} \approx 23$ brings variance back to reasonable range

This keeps the softmax distribution "smooth" enough to train effectively.

## Multi-Head Attention

Single attention head is limited—it learns one way to look at the input.

**Multi-Head Attention** runs $h$ attention heads in parallel:

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, ..., \text{head}_h)W^O$$

where each head:

$$\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)$$

**Why multiple heads?**
- Different heads learn different representation subspaces
- One head might focus on syntax, another on semantics
- More robust than single attention mechanism

## A Concrete Example

Suppose we're encoding the sentence: "The cat sat on the mat"

**Word vectors** (d_k = 4, simplified):
```
the:  [0.1, 0.2, 0.3, 0.1]
cat:  [0.5, 0.2, 0.1, 0.8]
sat:  [0.3, 0.6, 0.2, 0.4]
```

**Computing attention for "cat"** (position 2):

Q for "cat" (what it's looking for):
```
Q = [0.5, 0.2, 0.1, 0.8]
```

All K vectors (what others represent):
```
K = [[0.1, 0.2, 0.3, 0.1],   # "the"
     [0.5, 0.2, 0.1, 0.8],   # "cat"
     [0.3, 0.6, 0.2, 0.4]]   # "sat"
```

Attention scores ($QK^T$):
```
score_the = 0.5*0.1 + 0.2*0.2 + 0.1*0.3 + 0.8*0.1 = 0.17
score_cat = 0.5*0.5 + 0.2*0.2 + 0.1*0.1 + 0.8*0.8 = 0.91 (highest—pays attention to itself!)
score_sat = 0.5*0.3 + 0.2*0.6 + 0.1*0.2 + 0.8*0.4 = 0.47
```

After softmax (scaled):
```
attention_weights ≈ [0.1, 0.7, 0.2]
```

The "cat" position focuses 70% on itself, 20% on "sat", 10% on "the".

## Implementation in PyTorch

```python
import torch
import torch.nn.functional as F

class Attention(torch.nn.Module):
    def __init__(self, d_model, num_heads):
        super().__init__()
        self.num_heads = num_heads
        self.d_k = d_model // num_heads
        
        # Linear projections
        self.W_q = torch.nn.Linear(d_model, d_model)
        self.W_k = torch.nn.Linear(d_model, d_model)
        self.W_v = torch.nn.Linear(d_model, d_model)
        self.W_o = torch.nn.Linear(d_model, d_model)
    
    def forward(self, Q, K, V, mask=None):
        batch_size = Q.shape[0]
        
        # Linear projections
        Q = self.W_q(Q)  # (batch, seq_len, d_model)
        K = self.W_k(K)
        V = self.W_v(V)
        
        # Reshape for multi-head
        Q = Q.view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        K = K.view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        V = V.view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        
        # Compute attention scores
        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        
        # Apply softmax and dropout
        attention_weights = F.softmax(scores, dim=-1)
        output = torch.matmul(attention_weights, V)
        
        # Concatenate heads
        output = output.transpose(1, 2).contiguous()
        output = output.view(batch_size, -1, self.num_heads * self.d_k)
        
        # Final linear projection
        return self.W_o(output)
```

## Key Takeaways

1. **Attention is learned**: The model learns which positions to focus on through gradient descent
2. **Parallelizable**: All positions compute attention in parallel (unlike RNNs)
3. **Interpretable**: You can visualize attention weights to understand model decisions
4. **Scalable**: Transformer architecture powers LLMs at million-token scales
5. **Not magic**: Just weighted averaging with learned weights

## Further Reading

- Original paper: [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
- Interactive visualization: [Transformer Explainer](https://poloclub.github.io/transformer-explainer/)
- My post on [Transformers in Production](/blog/build-logs/transformers-prod)
