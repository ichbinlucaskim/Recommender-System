
## What is BERT4Rec?

> **BERT4Rec is a recommendation model based on the BERT architecture.**
> It treats a user’s **sequence of item interactions** (clicks, views, etc.) like a sentence, and uses a **masked item prediction objective** to learn what a user is likely to engage with next.

It’s inspired by **BERT in NLP**, where missing words in a sentence are predicted using **bidirectional context**.

### Input: User Interaction Sequence

A user u has interacted with a sequence of items:

$S_u = [v_1, v_2, \dots, v_T]$

- $v_t$: the item interacted with at time $t$
- The sequence is typically padded or truncated to a fixed length $T$


### Masking: Randomly Hide Some Items

Some items in the sequence are replaced with **[MASK]** tokens:

$\tilde{S}_u = [v_1, \text{[MASK]}, v_3, \dots, \text{[MASK]}, v_T]$

-  Just like BERT, we **randomly mask some positions**
-  The model learns to **predict the masked items** based on context


### Encoding: Transformers Layers

The masked sequence is passed through a Transformer encoder:

$\mathbf{H} = \text{TransformerEncoder}( \text{Emb}(\tilde{S}_u) )$

- $\text{Emb}(v) = \mathbf{e}_v + \mathbf{p}_t:$
 - the embedding of item v, plus its position embedding $\mathbf{p}_t$

-  $\mathbf{H} = [h_1, h_2, \dots, h_T]:$
 - the contextualized embeddings for each position in the sequence


### Prediction: Fill in the Masked Items


For each masked position t, the model predicts the true item v_t using the hidden vector $h_t$:

$P(v_t | \tilde{S}_u) = \text{softmax}( \mathbf{h}_t \cdot \mathbf{E}^\top )$

-  $\mathbf{E}$: the embedding matrix of all items
-  The dot product gives the similarity between $h_t$ and all items
-  Softmax converts this to a probability distribution


### Objective Function: Cross Entropy Loss

The model is trained to minimize the cross-entropy loss over all masked positions:


$\mathcal{L} = - \sum_{t \in \mathcal{M}_u} \log P(v_t | \tilde{S}_u)$

-  $\mathcal{M}_u$: set of masked positions in the user sequence
-  Encourages the model to assign high probability to the true masked item


### Advandages

|**Feature**|**Description**|
|---|---|
|**Bidirectional context**|Considers both past and future items using self-attention|
|**Masked learning**|Learns robust item representations through prediction tasks|
|**Position embeddings**|Maintains sequence order|
|**Highly parallelizable**|Unlike RNNs, training can be done in parallel|
|**Sequence-aware**|Strong at modeling user behavior patterns over time|