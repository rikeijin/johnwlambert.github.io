---
layout: post
title:  "Metric Learning"
permalink: /metric-learning/
excerpt: "contrastive loss, triplet loss, angular softmax"
mathjax: true
date:   2019-06-18 11:00:00

---
Table of Contents:
- [Why do we need residual connections?](#need-for-residual)


## A hyperplane is a linear classifier

A hyperplane is a plane in n-dimensions that can split a space into two halfspaces. For the sake of simplicity, consider a hyperplane in 2-dimensional space (a line).

A common and intuitive representation of a line in two-dimensions, $$\mathbb{R}^2$$, is $$y=mx+b$$, where $$y=x_2$$ and  $$x=x_1$$. Here $$m$$ represents slope, and $$b$$ represents a y-intercept. This representation is a poor choice, however, because we cannot represent vertical lines, e.g. $$ x = c $$, with this expression. A vertical line equation would have to be something like $$y = mx$$, where $$m \rightarrow \infty$$. Thus, this isn't a general equation of a line.

Over $$\mathbb{R}^2$$, a better representation is 

$$
w_0 + w_1 x_1 + w_2 x_2 = 0
$$

where $$w_0, w_1, w_2$$ are weights ("parameters"), and $$w_0$$ is often considered the "bias" term, denoting the distance from the origin.  When $$w_2 =0$$, we have a vertical line, and when $$w_1=0$$, we have a horizontal line. We can always convert back to our intuitive representation $$y=mx+b$$ by simple algebra. For example, consider the hyperplane

$$
\begin{array}{ll}
W = \begin{bmatrix} w_1 \\ w_2 \end{bmatrix}^T = \begin{bmatrix} 2 \\ 1 \end{bmatrix}^T, & w_0 = b = -2
\end{array}
$$

By algebra:

$$
\begin{aligned}

w_0 + w_1 x_1 + w_2 x_2 = 0 \\
w_0 + w_1 x + w_2 y = 0 & \mbox{ (let } y = x_2, x = x_1) \\
w_2 y =  -w_1 x - w_0 \\
y = -\frac{w_1}{w_2}x + -\frac{w_0}{w_2} \\
y = -\frac{2}{1}x + -\frac{-2}{1} &  \mbox{ (let } w_0 = -2, w_1 = 2, w_2 = 1) \\
y = -2x + 2
\end{aligned}
$$


```python
import numpy as np
import matplotlib.pyplot as plt
x = np.linspace(-3,3,100)
y = -2*x + 2
plt.scatter(x,y,10,marker='.', color='r')
plt.xlim([-3,3])
plt.ylim([-6,6])
plt.show()
x.shape
(100,)
np.allclose(-2 + 2 * x + 1 * y, np.zeros(100) )
True
```




## Angular Softmax

SphereFace



## Hinge Loss


max-margin


## Contrastive Loss



## Triplet Loss


https://github.com/lugiavn/generalization-dml

Hard negative mining


## Which layer to fine-tune?





## Additive Margin Softmax



[1] Weiyang Liu, Yandong Wen, Zhiding Yu, Ming Li, Bhiksha Raj, Le Song. SphereFace: Deep Hypersphere Embedding for Face Recognition. [arXiv](https://arxiv.org/abs/1704.08063).

[2]

[3]