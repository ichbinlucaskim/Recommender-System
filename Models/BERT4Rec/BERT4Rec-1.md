
# BERT4Rec

> **BERT4Rec is a recommendation model that uses the BERT architecture to predict which item a user will interact with next based on the sequence of their past behavior.**


## Where is this come from?

BERT originally comes from **Natural Language Processing (NLP)**.

For example, in a sentence like:

  
> “I had ___ for breakfast today.”

BERT predicts the missing word by looking at **both the left and right context**.

That’s the key idea behind BERT: **Bidirectional understanding of context.**


## How can we use this?

Think of the **order of items** a user interacts with (movies, clicks, songs, etc.)

as being similar to a **sequence of words in a sentence**.

Example:

> “Iron Man → Avengers → Doctor Strange → ???”
  
→ What should be recommended next?

**BERT4Rec treats this sequence like a sentence**, masks some items in the middle,

and learns to **predict those missing items** — just like predicting missing words in NLP.


## How it works

1. The model takes a **sequence of items** that a user interacted with (e.g., what they watched or clicked, in order)

2. It randomly **masks some of the items** in the sequence

3. It uses the rest of the sequence to **predict the masked items**

4. The model learns patterns in user behavior by doing this over and over