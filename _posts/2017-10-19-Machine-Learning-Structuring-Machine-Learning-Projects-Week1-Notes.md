---
layout: post
title: "Machine Learning: Structuring Machine Learning Projects Week1 Notes"
date: 2017-10-19
categories: blog
tags: [Machine Learning]
description: "Notes for Machine Learning Course: Structuring Machine Learning Projects"
---

test $s$ s

# **Introduction to ML Strategy**
## **Why ML Strategy**
**Ideas**
- Collect more data
- Collect more diverse traning set
- Train algorithm longer with gradient descent
- Try Adam instead of gradient descent
- Try bigger network
- Try smaller network
- Try dropout
- Add L2 regularization
- Network architecture
  - Activation functions
  - Number of hidden units

## **Orthogonalization**
**Chain of assumptions in ML**
- Fit training set well on cost function (Bigger network, Adam)
- Fit dev set well on cost function (Regularization, bigger training set)
- Fit test set well on cost function (Bigger dev set)
- Performs well in real world (Change dev set or cost function)

# **Setting up your goal**
## **Single number evaluation metric**
### **Using a single number evaluation metric**
**F1 score = average of Precision and Recall**

**Harmonic Mean**

$$frac{2}{frac{1}{P}+frac{1}{R}}$$

Use Dev set + Single real number evaluation metric to speed up iteration

**Average**

## **Satisficing and Optimizing metric**
**Accuracy --- optimizing**

**running time --- satisficing**

## **Train/dev/test distributions**
**Cat classification dev/test sets**

Randomly shuffle data into dev/test set so that both dev and test sets have data from all regions

**Guideline**

Choose a dev set and test set to reflect data you expect to get in the future and consider important to do well on

## **Size of the dev and test sets**
**Old way of splitting data(smaller data sets)**

Train: 70%, Test: 30%
Train: 60%, Dev: 20%, Test: 20%

**New way**

Total: 1,000,000
Train: 98%, Dev: 1%, Test: 1%

**Size of test set**

Set your test set to be big enough to give high confidence in the overall performance of your system

## **When to change dev/test sets and metrics**
**Cat dataset examples**

Metric: classification error
Algorithm A: 3% error ---> pornographic
Algorithm B: 5% error

### Metric + Dev: Prefer A; You/users: Prefer B

$$Error: frac{1}{m_{dev}}\sum_{i=1}^{M_{dev}}w^{(i)}I\{y_{pred}^{(i)}\neq y^{(i)}\}$$

$$
w^{(i)}=
\begin {cases}
1, & if\ x^{(i)}\ is\ non-porn \\
10, & if\ x^{(i)}\ is\ porn
\end {cases}
$$

**Orthogonalization for cat pictures: anti-porn**

1. So far we've only discussed how to define a metric to evaluate classifiers(place the target).
2. Worry separately about how to do well on this metric(aim it).

### **If doing well on your metric + dev/test set does not correspond to doing well on your application, change your metric and/or dev/test set.**

# **Comparing to human-level performance**
## **Why human-level performance?**
**The accurancy of machine learning model will surpass human accurancy and getting close to the Bayes optimal error(best possible error)**

**Humans are quite good at lot of tasks. So long as ML is worse than human, you can:**
- Get labeled data from humans
- Gain insight from manual error analysus:
  Why did a person get this right?
- Better analysis of bias/variance

## **Avoidable bias**
Humans (Bayes)       1 %              7.5 %
Training Error       8 %                8 %
Dev error           10 %               10 %
Result         Forcus on bias   Forcus on variance

**Human-level error as a proxy for Bayes error**


## **Understanding human-level performance**
**Human-level error as proxy for Bayes error**

Medical image classification example:
Suppose:
(a) Typical human                        3% error
(b) Typical doctor                       1% error
(c) Experienced doctor                 0.7% error
(d) Team of experienced doctors        0.5% error

Define human-level error as 0.5% error

**Summary of bias/variance with human-level performance**

        Human-level error        
^                                ^
|(Bias) (proxy for Bayes error)  | "Avoidable bias"
V                                v 
        Training error           
                                 ^
                                 | "Variance"
                                 v
        Dev error

## **Surpassing human-level performance**
Team of humans      0.5%         0.5%
One human             1%           1%
Training error      0.6%         0.3%
Dev error           0.8%         0.4%

**Problems where ML significantly surpasses human-level performance**
- Online advertising
- Product recommendations
- Logistics (predicting transit time)
- Loan approvals

**Learning structed data; Not natural perception**


- Speech recognition
- Some image recognition
- Medical
 - ECG, skin cancer, ...

## **Improving your model performance**
**The two fundamental assumptions of supervised learning**

1. You can fit the training set pretty well. ~ Avoidable bias
2. The training set performance generalizes pretty well to the dev/test set. ~ Variance

**Reducing (avoidable) bias and variance**

Human-level
^
|                    - Train bigger model
| Avoidable bias ->  - Train longer/better optimization algorithms
|                     - Momentum, RMSprop, Adam
|                    - NN architecture/hyperparameters search (RNN, CNN)
v
Training error
^
|                    - More data
| Variance       ->  - Regularization
|                     - L2, dropout, data augmentation
|                    - NN architecture/hyperparamaters search
v
Dev error
