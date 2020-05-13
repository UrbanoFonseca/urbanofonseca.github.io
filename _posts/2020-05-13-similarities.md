---
layout: post
title: "Similarities"
author: "Francisco Fonseca"
categories: tutorial
tags: [tutorial]
image: nyc-4.jpg
---

## Computing Similarities
We often need to get measures of similarities between vectors. This may be useful to find similar users/items in a Recommender System or similar entities in the same embedding.

### Dot Product
The dot product is simply the multiplication of values inside two same-length vectors, which results in a number. 

Consider a vector with individual preferences for apples and oranges, `likes_vector = [likes_apples, likes_oranges]`, in which higher values indicate a greater preference. We can compute the similarity between two persons as the inner product.

Remember that, if both vectors have the same size, we need to transpose one of them to get a single value.

$$ john = [0.8, 0.3] \\ mary = [0.1, 0.9] \\ $$
$$ similarity(a,\ b) \approx a^{T} \cdot b $$
$$ similarity (john, mary) \approx \begin{bmatrix} 0.8 \\ 0.3 \end{bmatrix} \cdot
    \begin{bmatrix} 0.1 & 0.9 \end{bmatrix}
    = 0.8*0.1 + 0.3*0.9 = 0.35 $$

Note that John likes apples much more than oranges and Mary has the inverse preference. What should happen if I compare Mary's preferences with someone who also likes oranges more? Let it be Emma.

$$ emma = [0.2, 0.8] $$
$$
    similarity(emma, mary) \approx \begin{bmatrix} 0.2 \\ 0.8\end{bmatrix} \cdot
    \begin{bmatrix} 0.1 & 0.9\end{bmatrix}= 0.2*0.1 + 0.8*0.9 = 0.74
$$

Intuitively, the similarity score increased since Emma has a much more similar taste to Mary than John.

The downside of using the dot product as a similarity score is that it may vary according to the magnitude of the values (absolute preference) instead of the direction of the vector (relative preference). Consider that Emma has a evil twin (let's call her Karen), which likes oranges 9 times more than apples (relative preference similar to Mary) but she likes everything two times more, so that her preferences vector would be `[0.4, 1.6]`. Computing the similarities, we would get:

$$ karen = [0.4, 1.6] $$
$$
    similarity(karen, mary) \approx \begin{bmatrix} 0.4 \\ 1.6 \end{bmatrix} \cdot
    \begin{bmatrix} 0.1 & 0.9\end{bmatrix}= 0.4*0.1 + 1.6*0.9 = 1.48
$$

The similarity between Karen and Mary is twice as much as the similarity between Emma and Mary, just because Emma's evil twin likes everything Emma does but two times more. Thus, we may want to have a similarity score that takes into account only relative preferences *(pro tip: cosine similarity)*.
