---
layout: post
title: "abnormal_activations"
author: "Francisco Fonseca"
categories: github
tags: [github]
image: nyc-3.jpg
---

## abnormal_activations
Extend your Keras Nets with non-traditional activation functions.


Activations available:
- alpha_linear
- step
- LeCunSigmoid
- ReSech
- scaled_sigmoid
- penalized_tanh
- trunc_sin
- sin

## Code Snippet
```python
from abnormal_activations.abnormal_activations import ActivationFunctions

# alpha_linear = alpha * x
al = ActivationFunctions.alpha_linear
al(0.3, alpha=0.5)  # 0.15
al(2., alpha=0.1)   # 0.20
```