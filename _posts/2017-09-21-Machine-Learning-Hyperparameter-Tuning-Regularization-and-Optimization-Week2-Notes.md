---
layout: post
title: "Machine Learning: Hyperparameter tuning, Regularization and Optimization Week2 Notes"
date: 2017-09-21
categories: blog
tags: [Machine Learning]
description: "Notes for Machine Learning Course: Hyperparameter tuning, Regularization and Optimization"
---

# **Optimization Algorithms**
## **Mini-batch Gradient Descent**
- **Training set i:**
$$X^{(i)}$$
- **Neural Network Layer l:**
$$Z^{[l]}$$
- **Mini Batch t:**
$$X^{\{t\}}$$
$$Y^{\{t\}}$$

- **Function Flow**
```
for t = 1, ..., 5000
    forward prop
    compute cost J
    backward prop
    update parameters
```

## **Understanding Mini-batch Gradient Descent**
**Choosing Your Mini-batch Size**

- Size = m: Batch Gradient Descent
> Too Long Per Iteration

- Size = 1: Stochastic Gradient Descent
> Lose Speedup from Vectorization

- In Practice: Somewhere in-between 1 and m
> Fastest Learning
> - Vectorization (~1000)
> - Make Progress without Processing Entire Training Set


If small training set: Use batch gradient descent (m <= 2000)

- Typical mini-batch size: 64, 128, 256, 512
- Make sure mini-batch fits in CPU/GPU memory

## **Exponentially Weighted Averages**
$$V_t=\beta V_{t-1}+(1-\beta)\theta_t$$

**Average over:**

$$\approx\frac{1}{1-\beta} days$$

$$\beta=0.9 \approx 10-days-temperature$$

$$\beta=0.98 \approx 50-days-temperature$$

$$\beta=0.5 \approx 2-days-temperature$$

## **Bias Correction in Exponentially Weighted Averages**
**Bias Correction**

To avoid bias, instead of using $$V_t$$, we use:
$$\frac{V_t}{1-\beta^t}$$

## **Gradient Descent with Momentum**
**Implementation Details**

$$V_{dW}=\beta V_{dW}+(1-\beta)d_W$$

$$V_{db}=\beta V_{db}+(1-\beta)d_b$$

$$W=W-\alpha V_{dW}$$

$$b=b-\alpha V_{db}$$

## **RMSprop (Root Mean Square)**
**Implementation Details**

$$S_{dW}=\beta_2 S_{dW}+(1-\beta_2)dW^2$$

$$S_{db}=\beta_2 S_{db}+(1-\beta_2)db^2$$

$$W:=W-\alpha \frac{dW}{\sqrt{S_{dw}}}$$

$$b:=b-\alpha \frac{db}{\sqrt{S_{db}}}$$

## **Adam(Adaptive Moment Estimation) Optimization Algorithm**
**Implementation Details**

$$V_{dW}=0, S_{dW}=0, V_{db}=0, S_{db}=0$$

On iterater t:

Compute dW, db using mini-batch

$$V_{dW}=\beta_1 V_{dW}+(1-\beta_1)d_W$$

$$V_{db}=\beta_1 V_{db}+(1-\beta_1)d_b$$

$$S_{dW}=\beta_2 S_{dW}+(1-\beta_2)dW^2$$

$$S_{db}=\beta_2 S_{db}+(1-\beta_2)db^2$$

$$V_{dW}^{corrected}=V_{dW}/(1-\beta_1^t)$$

$$V_{db}^{corrected}=V_{db}/(1-\beta_1^t)$$

$$S_{dW}^{corrected}=S_{dW}/(1-\beta_2^t)$$

$$S_{db}^{corrected}=S_{db}/(1-\beta_2^t)$$

$$W:=W-\alpha\frac{V_{dW}^{corrected}}{\sqrt{S_{dW}^{corrected}}+\epsilon}$$

$$b:=b-\alpha\frac{V_{db}^{corrected}}{\sqrt{S_{db}^{corrected}}+\epsilon}$$

**Hyperparameters Choice**

$$\alpha$$ Needs to be tune

$$\beta_1$$ 0.9    $$(dW)$$

$$\beta_2$$ 0.99   $$(dW^2)$$

$$\epsilon 10^{-8}$$

## **Learning Rate Decay**
**Implementation Details**
$$\alpha=\frac{1}{1+decay-rate*epoch-num}\alpha_0$$

**Other Learning Rate Decay Methods**

Exponentially Decay
$$\alpha=0.95^{epoch-num}\alpha_0$$

$$\alpha=\frac{k}{\sqrt{epoch_num}}\alpha_0$$

Discrete Staircase

Manual Decay

## **The Problem of Local Optima**
**Problem of Plateaus**

- Unlikely to get stuck in a bad local optima
- Plateaus can make learning slow
