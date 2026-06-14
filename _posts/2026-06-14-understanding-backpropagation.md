---
layout: post
title: "Understanding Backpropagation from Scratch"
date: 2026-06-14
---

Backpropagation is the algorithm that makes neural networks learn. Despite its reputation as a black box, it's really just the chain rule from calculus applied systematically across a computation graph.

## The Core Idea

A neural network is a function composed of many simpler functions. Training it means finding weights that minimize a loss function — typically the difference between predicted and actual outputs. Gradient descent does this iteratively: compute how much each weight contributes to the loss, then nudge it in the opposite direction.

Backpropagation is how we compute those gradients efficiently.

## A Simple Example

Consider a two-layer network with a single training example. The forward pass computes:

```
z1 = W1 * x + b1
a1 = relu(z1)
z2 = W2 * a1 + b2
loss = (a2 - y)^2
```

The backward pass works in reverse, computing `dLoss/dW` for every weight by repeatedly applying the chain rule:

```
dLoss/dW2 = dLoss/da2 * da2/dz2 * dz2/dW2
```

Each term is easy to compute locally. The key insight is that each layer only needs the gradient flowing in from the layer ahead of it — this is what makes backprop efficient.

## Why It Matters

Before backpropagation, training deep networks was largely intractable. The 1986 paper by Rumelhart, Hinton, and Williams popularized the algorithm and kicked off the first wave of neural network research. Forty years later, the same fundamental algorithm powers systems with hundreds of billions of parameters.

Understanding it at this level — not just using it via `loss.backward()` — changes how you think about network architecture, initialization, and why certain training problems (like vanishing gradients) happen in the first place.

## Next Steps

If you want to go deeper, implement a small autograd engine from scratch. Andrej Karpathy's [micrograd](https://github.com/karpathy/micrograd) is an excellent reference — around 150 lines of Python that implement the core of what PyTorch does under the hood.
