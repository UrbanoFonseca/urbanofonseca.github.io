---
layout: post
title: "Normalizator"
author: "Francisco Fonseca"
categories: github
tags: [github]
image: nyc-2.jpg
---

## Normalizator
A Python library for normalization of continuous variables. Inspired in Scikit-learn scalers.

Scalers available:
- Standard (aka Z-Score)
- Min-Max
- Decimal
- Median
- MMAD (Median and Median Absolute Deviation)
- Max
- Modified Tanh


## Code Snippet
```python
from normalizator.normalizator import StandardScaler

sc = StandardScaler()

# print params
sc.means
sc.stds
```