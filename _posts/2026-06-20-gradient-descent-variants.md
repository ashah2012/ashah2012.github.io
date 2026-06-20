---
layout: post
title: "Gradient Descent Variants: Momentum, RMSProp, and Adam Explained"
date: 2026-06-20
description: >-
  A practical, math-first explanation of why vanilla gradient descent
  struggles in practice, and how momentum, RMSProp, and Adam fix it.
---

Vanilla gradient descent updates each weight by a fixed step in the direction opposite its gradient: `w = w - lr * grad`. It works, but it's slow to converge and easy to destabilize — and almost nothing trains with it directly anymore. Momentum, RMSProp, and Adam are the practical fixes, and each one solves a specific failure mode of the one before it.

## Why Vanilla Gradient Descent Struggles

Loss surfaces aren't bowls — they're full of narrow ravines, where the gradient in one direction is much steeper than in another. With a single fixed learning rate, you either oscillate back and forth across the steep direction, or move too slowly along the shallow one. Picking a learning rate that's safe for the steep direction makes progress along the shallow direction painfully slow.

## Momentum

Momentum keeps a running average of past gradients and updates the weight using that average instead of the raw gradient:

```
v = beta * v + (1 - beta) * grad
w = w - lr * v
```

Intuitively, it's a ball rolling downhill: gradients that consistently point the same way accumulate velocity, while oscillating gradients (the back-and-forth across a ravine) cancel out in the average. This damps oscillation and speeds up movement in directions where the gradient is consistent.

## RMSProp

Momentum doesn't address that different parameters can need very different step sizes. RMSProp tracks a moving average of the *squared* gradient per parameter, and divides the update by its square root:

```
s = beta * s + (1 - beta) * grad^2
w = w - lr * grad / (sqrt(s) + eps)
```

Parameters with consistently large gradients get their effective learning rate shrunk; parameters with small, infrequent gradients get it boosted. Each weight effectively gets its own adaptive learning rate.

## Adam

Adam combines both ideas: momentum for the direction, RMSProp-style scaling for the step size, plus a bias correction term because the moving averages start at zero and are biased low early in training.

```
m = beta1 * m + (1 - beta1) * grad
s = beta2 * s + (1 - beta2) * grad^2

m_hat = m / (1 - beta1^t)
s_hat = s / (1 - beta2^t)

w = w - lr * m_hat / (sqrt(s_hat) + eps)
```

`t` is the current training step, and the bias correction terms matter most early on — without them, `m` and `s` are artificially small for the first several updates.

## Why It Matters

This is why `optimizer=Adam` is the default in nearly every training script: it inherits momentum's resistance to oscillation and RMSProp's per-parameter adaptivity, with defaults (`beta1=0.9, beta2=0.999`) that work reasonably well across a huge range of problems without manual tuning. Understanding what each piece corrects for makes it much easier to diagnose training instability — a loss that oscillates wildly often means the effective step size is too large for the steepest direction in the loss surface, regardless of which optimizer is doing the stepping.
