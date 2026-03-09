---
description: Various cost functions for various scenarios
---

# Cost Functions

Let $$a^L = \sigma(z^L)$$ be the output of the neural network and $$y$$ the label (expected output). Different cost functions come up for different goals, and each has a "canonical" activation associated with it that optimises calculations.

<table><thead><tr><th width="332">Goal</th><th width="225">Cost Function</th><th>Activation</th></tr></thead><tbody><tr><td>Regression (continuous from <span class="math">-\infin</span> to <span class="math">\infin</span>)</td><td>MSE</td><td>Linear - <span class="math">\sigma(z) = z</span></td></tr><tr><td>Binary Classification</td><td>Binary Cross-Entropy</td><td>Sigmoid</td></tr><tr><td>Multi-Class Classification</td><td>Categorical Cross-Entropy</td><td>Softmax</td></tr></tbody></table>

## MSE (Mean Squared Error)

Also known as the **half-squared error**, MSE is simply half the **Euclidean distance** between $$a^L$$ and $$y$$:

$$
C(a^L,y) =\frac{1}{2} \lVert a^L-y \rVert^2 = \frac{1}{2}\sum_{i=1}^n (a^L_i - y_i)^2
$$

The purpose of the half become clear when we note that

$$
\frac{\partial C}{\partial a^L_i} = a^L_i - y_i
$$

So, in terms of vectors,

$$
\frac{\partial C}{\partial a^L} = a^L - y
$$

This makes the calculation of $$\delta^L$$ quite easy:

$$
\delta^L = (a^L - y) \odot \sigma^\prime(z^L)
$$

Note that with **linear activation** $$\sigma(z) = z$$, we would get $$\delta^L = a^L - y$$. This is desirable!

## Binary Cross-Entropy Loss

Given a label $$y$$, which is either $$0$$ or $$1$$ (hence binary), we have

$$
C(a^L, y) = -[y\ln (a^L) + (1-y)\ln (1- a^L)]
$$

Then we have

$$
\frac{\partial C}{\partial a^L} = -\frac{y}{a^L} + \frac{1-y}{1-a^L} = \frac{a-y}{a(1-a)}
$$

Beautifully, if $$\sigma(z)$$ is the sigmoid function then we have $$\sigma^\prime(z) = \sigma(z)(1-\sigma(z))$$, so

$$
\delta^L = \frac{a-y}{a(1-a)} \cdot a(1-a) = a-y
$$

as we'd like!

## Categorical Cross-Entropy Loss + Softmax

CE loss itself can look like a weird choice:

$$
C(a^L,y) = -\sum_{i=1}^n y_i\ln(a^L_i)
$$

However, we can make it highly efficient by using **softmax** on the final neurons rather than **sigmoid** again. Softmax is simply

$$
a_i = \frac{e^{z_i}}{\sum_j e^{z_j}}
$$

When combined with **softmax**, we get a very nice derivation for $$\delta$$:

$$
\delta^L = a^L - y
$$

### Proof

This proof is a bit more involved. Using multivariate chain rule:

$$
\delta^L_i = \frac{\partial C}{\partial z_i} = \sum_k \frac{\partial C}{\partial a^L_k} \frac{\partial a^L_k}{\partial z_i}
$$

We can see, from the definition of $$C$$, that

$$
\frac{\partial C}{\partial a^L_i} = -\frac{y_i}{a^L_i}
$$

It is a bit harder to calculate $$\frac{\partial a_k}{\partial z_i}$$. If $$k = i$$, then by the quotient rule we get

$$
\begin{align*}
\frac{\partial a^L_i}{\partial z_i} &= \frac{\partial}{\partial z_i}\left[\frac{e^{z_i}}{\sum_j e^{z_j}}\right] \\
&= \frac{e^{z_i}\sum_j e^{z_j} - e^{z_i}e^{z_i}}{\left(\sum_j e^{z_j}\right)^2} \\
&= \frac{e^{z_i}}{\sum_j e^{z_j}} - \left(\frac{e^{z_i}}{\sum_j e^{z_j}}\right)^2 \\
&= a^L_i - (a^L_i)^2 \\
&= a^L_i(1- a^L_i)
\end{align*}
$$

If $$k \neq i$$, it's a bit different:

$$
\begin{align*}
\frac{\partial a^L_k}{\partial z_i} &= \frac{\partial}{\partial z_i}\left[\frac{e^{z_k}}{\sum_j e^{z_j}}\right] \\
&= \frac{0 \cdot \sum_j e^{z_j} - e^{z_i}e^{z_k}}{\left(\sum_j e^{z_j}\right)^2} \\
&= -\frac{e^{z_i}}{\sum_j e^{z_j}} \cdot \frac{e^{z_k}}{\sum_j e^{z_j}} \\
&= -a^L_ia^L_k
\end{align*}
$$

Therefore

$$
\begin{align*}
\delta^L_i &=  \frac{\partial C}{\partial a^L_i} \frac{\partial a^L_i}{\partial z_i} + \sum_{k\neq i} \frac{\partial C}{\partial a^L_k} \frac{\partial a^L_k}{\partial z_i} \\
&= -\frac{y_i}{a^L_i} \cdot a^L_i(1- a^L_i) + \sum_{k \neq i} -\frac{y_k}{a^L_k} \cdot -a^L_ka^L_i \\
&= -y_i(1-a^L_i) + \sum_{k \neq i} y_ka^L_i \\
&= -y_i + a^L_iy_i + \sum_{k \neq i} y_ka^L_i \\
&= -y_i + a^L_i\sum_{k} y_k \\
&= a^L_i - y_i
\end{align*}
$$

