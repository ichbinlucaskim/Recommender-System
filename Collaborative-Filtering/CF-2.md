Collaborative Filtering (CF) is a technique used in recommendation systems to predict user preferences based on historical interactions. It operates under the assumption that **users with similar past behaviors will have similar future preferences, and that items co-consumed by similar users exhibit latent similarities.**

This approach is fundamentally different from **content-based filtering**, which relies on explicit item features (e.g., genre, director, or product specifications). Instead, CF derives insights purely from **user-item interaction data**—typically represented as a **sparse user-item matrix** where most values are unknown.


# User-Based CF
: A user's preference for an item can be inferred based on the preference of similar users.

- Concept: Predicts user preferences by finding similar users
- Mathematical Representation: Similarity between ***user vectors*** in a user-item Matrix

### Pearson Correlation Coefficient

$$\text{sim}(u, v) = \frac{ \sum\limits_{i \in I_{uv}} (R_{u,i} - \bar{R}_u)(R_{v,i} - \bar{R}_v)}
{\sqrt{ \sum\limits_{i \in I_{uv}} (R_{u,i} - \bar{R}_u)^2 } \cdot \sqrt{ \sum\limits_{i \in I_{uv}} (R_{v,i} - \bar{R}_v)^2 }}$$

- $R_{u,i}$: user u’s rating for item i

- $\bar{R}_u$: average rating by user u

- $R_{u,i} - \bar{R}_u$: > Whether the user rated item **i** **above or below their average rating.**

- $I_{uv}$: set of items rated by both users u and v


$$\sum\limits_{i \in I_{uv}} (R_{u,i} - \bar{R}u)(R{v,i} - \bar{R}_v)$$

- User **u** and user **v** each compute how much they liked (or disliked) item **i** **relative to their own average rating**, and then multiply those deviations.

- These products are summed across all items that both users have rated.

-  This measures whether their **rating deviation directions align**:
	- Example: If both you and I rated an item higher than our averages → the product is positive → similar taste
	- If I liked it but you rated it below your average → the product is negative → opposite taste


$$\sqrt{ \sum (R_{u,i} - \bar{R}u)^2 } \cdot \sqrt{ \sum (R{v,i} - \bar{R}_v)^2 }$$

- Each user’s **square root of the sum of squared deviations** represents the **magnitude (or scale) of their rating behavior**.

- In other words, this acts as a **normalization factor**, since **different users have different levels of rating variance**.
	- For example, one user may have large fluctuations in ratings, while another may rate more consistently.
	- So we divide by this to **ensure fair comparison between users**.

### Summary

The entire formula ultimately quantifies:

> **“How similar the directions of rating deviations are between users u and v,”** expressed as a value between -1 and 1.

-  Close to **1** → users have **very similar preferences**

-  Around **0** → **no meaningful relationship** in preferences

-  Close to **-1** → users have **opposite tastes**



### Prediction Formula

$$
\hat{R}_{u,i} = \bar{R}_u + 
\frac{
\sum\limits_{v \in U_i} \text{sim}(u, v) \cdot (R_{v,i} - \bar{R}_v)
}{
\sum\limits_{v \in U_i} |\text{sim}(u, v)|
}
$$

- $\hat{R}_{u,i}$: predicted rating by user u for item i

- $\text{sim}(u, v)$: similarity between user u and user v

- $\bar{R}_v$: average rating of user v

- $U_i$: set of users who have rated item i


$$ \sum\limits_{v \in U_i} \text{sim}(u, v) \cdot (R_{v,i} - \bar{R}_v)$$

- $v \in U_i$ : All users who have rated item **i**

- $\text{sim}(u, v)$: Similarity between user **u** and user **v**

- **The more similar they are**, the greater the influence their rating has

- $R_{v,i} - \bar{R}_v$:
	- How much user **v** liked item **i** **relative to their own average rating**
	- Example: If user **v** has an average rating of 3 and gave item **i** a 5 → deviation is **+2**

  

**Taken as a whole:**
- **Users who are more similar to user u have greater influence** on the prediction
- **If a similar user rated the item much higher than their average**, user u is also likely to like it
- **If the user is not very similar or didn’t particularly like the item**, their influence diminishes


$$\sum\limits_{v \in U_i} |\text{sim}(u, v)|$$

-  **Normalization Role**

- The denominator sums the **absolute values of the similarities**, so that the numerator becomes a **weighted average**.

  

**Why is this necessary?**

- If we simply summed the values, a few highly similar users could cause the predicted rating to be **unrealistically large**.
- So we divide by the **total similarity** — this turns the result into something like an average.
- If there are **many similar users**, the prediction is based on richer information.
- If there are **few similar users**, the model becomes more **conservative** in its prediction.


# Item-Based CF
: Items that are frequently co-rated by users are likely to be similar.

- Concept: Predicts preference based on item similarities
- Mathematical Representation: Similarity between ***item vectors*** in a user-item Matrix

### Cosine Similarity  

$$\text{sim}(i, j) = \frac{ \sum\limits_{u \in U_{ij}} R_{u,i} \cdot R_{u,j} }{ \sqrt{ \sum\limits_{u \in U_{ij}} R_{u,i}^2 } \cdot \sqrt{ \sum\limits_{u \in U_{ij}} R_{u,j}^2 } }$$

$R_{u,i} \cdot R_{u,j}$ 
- This multiplies the ratings that user _u_ gave to both item _i_ and item _j_.
- It reflects how similarly user _u_ rated both items.

$\sum\limits_{u \in U_{ij}}$
- This sums that product across all users _u_ who rated **both** item _i_ and item _j_.
- The more users agree on both items, the stronger the signal.


**Numerator (top part):**
- This captures **how closely items i and j are rated together** by the same users.
- It’s essentially the dot product between the rating vectors of item _i_ and item _j_.


**Denominator (bottom part):**
- Each square root term is the **magnitude (length)** of the rating vector for item _i_ or _j_.
- It normalizes the result so that similarity is scale-independent.


**The whole fraction:**
- This is the **cosine of the angle** between the two rating vectors.
- If two items are always rated similarly by the same users, the angle is small → cosine value close to 1.
- If the ratings are uncorrelated, the angle is large → cosine value close to 0.

**Final output:**
- The similarity score ranges from 0 to 1.
- **1** → users rate both items very similarly (high similarity)
- **0** → no meaningful similarity in how users rate them

### Prediction 

$$\hat{R}{u,i} = \frac{ \sum\limits{j \in I_u} \text{sim}(i, j) \cdot R_{u,j} }{ \sum\limits_{j \in I_u} |\text{sim}(i, j)| }$$

$I_u$
- This is the set of all items that user u has already rated.
- In other words, **user** u**’s known preferences**.


$\text{sim}(i, j)$

- The similarity between item i (the one we’re trying to predict)
- and item j (an item the user has already rated).
- Usually measured using **cosine similarity** or **adjusted cosine similarity**.


$R_{u,j}$
- The actual rating that user u gave to item j.
- This tells us how much the user liked a similar item.


**Numerator:** $\sum \text{sim}(i, j) \cdot R_{u,j}$
- For each item j that user u rated:
	- Multiply how similar item j is to item i
	-  By how much user u liked item j
	-  Then sum this across all such items

**This gives a weighted influence from the user’s past ratings, based on item similarity.**


**Denominator:** $\sum |\text{sim}(i, j)|$

- This normalizes the weighted sum,
- so the final prediction becomes a kind of **weighted average**
- Without this, the prediction could be too large or biased.