---
layout: post
title: Machine Learning: Hyperparameter tuning, Regularization and Optimization Week1 Notes
date: 2017-09-17
categories: blog
tags: [Machine Learning]
description: Notes for Machine Learning Course: Hyperparameter tuning, Regularization and Optimization
---

# **Setting up your Machine Learning Application**
## **Train/Dev/Test sets**

- For small number of Data, 70/30 or 60/20/20, but for large data say 1 million, dev and test sets just need to be big enough for evaluation. In this case, the percentage can be only 1% or even 0.5% (around 10,000)
- Make sure the dev and test sets come from the same distribution

## **Bias/Variance**
### **Assumption:**

- Human prediction(optimal) error is nearly 0%
- Training and dev sets are drawn from the same distribution

### **Result:**

- High Bias(underfitting): Training set error is **HIGH**; Dev set error is **HIGH**
- High Variance(overfitting): Training set error is **LOW**; Dev set error is **HIGH**
- High Bias & High Variance: Training set error is **HIGH**; Dev set error is **MUCH HIGHER** (15/30%)
- Low Bias & Low Variance: Training set error is **LOW**; Dev set error is **LOW**

## **Basic Recipe for Machine Learning**
```flow
st=>start: Start
condbias=>condition: High Bias?
(Training set performance)
opbias1=>operation: Bigger Network
opbias2=>operation: Train Longer
opbias3=>operation: (NN Architecture search)
condvariance=>condition: High Variance?
(Dev set performance)
opvariance1=>operation: More Data
opvariance2=>operation: Regularization
opvariance3=>operation: (NN Architecture search)
e=>end

st->condbias
condbias(yes)->opbias1->opbias2->opbias3->condbias
condbias(no)->condvariance
condvariance(yes)->opvariance1->opvariance2->opvariance3->condvariance
condvariance(no)->e
```

# **Regularizing Your Neural Network**
## **Regularization**
- **L2 Regularization:**
$$||w||_2^2=\sum_{j=1}^{x_n}w_j^2=w^Tw$$
- **Frobenius Norm:**
$$||w^{[l]}||_F^2=\sum_{i=1}^{n^{[l-1]}}\sum_{j=1}^{n^{[l]}}(w_{ij}^{[l]})^2$$

## **Dropout Regularization**
- You can apply higher prob for dropout for larger layers and maybe no dropout for small layers

## **Other Regularization Methods**
- Data Augmentation
- Early Stopping

# **Optimization Problem**
## **Normalizing Inputs**
- **Subtract Mean:**
$$\mu=\frac{1}{m}\sum_{i=1}^mx^{(i)}$$
$$x:=x-\mu$$

- **Normalize Variance**
$$\sigma^2=\frac{1}{m}\sum_{i=1}^m(x^{(i)})^2$$
$$x/=\sigma^2$$

## **Vanishing/Exploding Gradients**
If the NN is very deep, any weight which is less than 1 is likely to vanish and any weight which is larger than 1 is likely to explode

## **Weight Initialization For Deep Networks**
- **Relu**
$$w^{[l]}=np.random.randn(shape)*np.sqrt(\frac{2}{n^{[l-1]}})$$

- **Tanh**
$$w^{[l]}=np.random.randn(shape)*np.sqrt(\frac{1}{n^{[l-1]}})$$


## **Numerical Approximation of Gradients**
### **Checking Your Derivative Computation**
$$g(\theta)\approx\frac{f(\theta+\epsilon)-f(\theta-\epsilon)}{2\epsilon}$$

## **Grandient Checking**

## **Gradient Checking Implementation Notes**
- Don't use in training - only to debug
- If algorithm fails grad check, look at components to try to identify bug
- Remember regularization
- Doesn't work with dropout
- Run at random initialization; perhaps again after some training
