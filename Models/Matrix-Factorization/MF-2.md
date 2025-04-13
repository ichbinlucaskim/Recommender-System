
> **Matrix Factorization breaks users and items into learned feature vectors,then predicts ratings based on how well those vectors align.**

> Predict how much **user** u will like **item** i using learned **latent features** of both users and items.


$$\hat{R}{u,i} = \mathbf{p}u^\top \mathbf{q}i = \sum{f=1}^{k} p{u,f} \cdot q{i,f}$$

$\hat{R}_{u,i}$
-  The predicted rating that **user** u will give to **item** i.

 
$\mathbf{p}_u$
  
- A **latent vector** representing **user** u**’s preferences**, across k hidden dimensions. (Think: action-lover, animation-fan, serious-movie-hater, etc.)

$\mathbf{q}_i$

- A **latent vector** representing **item** i**’s characteristics**, across the same k dimensions.


$\mathbf{p}_u^\top \mathbf{q}_i$

-  A **dot product** between the two vectors — this measures **how well the user’s preferences match the item’s traits**. If they align well, the prediction is high.


**Expanded version:**
$$\hat{R}{u,i} = p{u,1} \cdot q_{i,1} + p_{u,2} \cdot q_{i,2} + \dots + p_{u,k} \cdot q_{i,k}$$

  
> Each $f$-th component is like:
> “How much the user cares about factor $f$ × how much the item contains factor $f$”
> → Then all these weighted matches are summed up.


 **Why It’s Better Than Pure CF**

| Feature                     | CF             | MF                            |
| --------------------------- | -------------- | ----------------------------- |
| Needs similarity?           | Yes (explicit) | No (implicit through vectors) |
| Learns hidden structure?    | ❌              | ✅                             |
| Works well on sparse data?  | ❌              | ✅                             |
| Generalizes to unseen data? | Limited        | Strong                        |
