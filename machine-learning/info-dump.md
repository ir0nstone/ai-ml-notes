---
description: >-
  A place I dump information, for later sorting. Do not expect logical thoughts,
  or things here to be correct!
---

# Info Dump

* Gradient vanishing and blowing up for sigmoid, ReLU
  * Gradient Clipping
  * Stable Sigmoid
  * Leaky ReLU
  * Learning Rate needed for ReLU is roughly `10x` smaller than for sigmoid due to bounds of derivative
* Flipping the kernel in backpropagation
* He and Xavier Initialization + reasons
  * Importance of weight initialization
* im2col
* Importance of Non-Linearity (via activation functions)
* Batch Normalization (mean 0, std 1) vs `/= 255.0`
* CNNs
  * Translation Invariance
  * FC NN vs CNN for MNIST
  * Dead Kernels (constantly `0`)
  * Learning Rate for kernels is much smaller than for the FC NN at the end, due to their size
