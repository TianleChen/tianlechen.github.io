---
layout: post
title: "Machine Learning: Structuring Machine Learning Projects Week2 Notes"
date: 2017-10-26
categories: blog
tags: [Machine Learning]
description: "Notes for Machine Learning Course: Structuring Machine Learning Projects"
---

# **Error Analysis**
## **Carrying out error analysis**
**Look at dev examples to evaluate ideas**

Should you try to make your cat classifier do better on dogs?
Let's say 10% error
- Get 100 mislabeled dev set examples
- Count up how many are dogs

If 5 out of 100 are dogs, then the ceiling of improvements is likely to be 9.5%

**Evaluate multiple ideas in parallel**

Ideas for cat detection:
- Fix pictaures of dogs being recognized as cats
- Fix great cats (lions, panthers, etc..) being misrecognized
- Improve performance on blurry images
- Instagram (filters)

## **Cleaning up incorrectly labeled data**
**Incorrectly labeled examples**

For training set, DL algorithms are quite robust to random errors (not systematic/consistent errors) in the training set.
For dev/test set, add extra column for incorrect label and check the percentage and fix it if it's large.

**Correcting incorrect dev/test set examples**

- Apply same process to your dev and test sets to make sure they continue to come from the same distribution.
- Consider examining examples your algorithm got right as well as ones it got wrong.
- Train and dev/test data may now come from slightly different distributions.

## **Build your first system quickly, then iterate**
**Speech recognition example**
- Noisy background
  - Cafe noise
  - Car noise
- Accented speech
- Far from microphone
- Young children's speech
- Stuttering
- ...

- Set up dev/test set and metric
- Build initial system quickly
- Use Bias/Variance analysis & Error analysis to prioritize next steps

**Guildline:**

Build your first system quickly, then iterate

# **Mismatched training and dev/test set**
## **Training and testing on different distributions**
**Cat app example**

Data from webpages | Data from mobile app(care about this)
------------------ | -------------------------------------
200,000            | 10,000

Option 1: 210,000 shuffle into 205,000 for training, 2500 for dev and 2500 for test (not good)
Option 2: 200,000 from web + 5000 from mobile, 2500 for dev and 2500 for test

**Speech recognition example**

Training(500,000) | Dev/test(20,000)
----------------- | ----------------
Purchased data | Speech activated
Smart speaker control | rearview mirror
Voice keyboard | 

Option 1: 500k for training, 10k for Dev and 10k for test
Option 2: 500k + 10k for training, 5k for Dev and 5k for test

## **Bias and Variance with mismatched data distributions**
**Cat classifier example**

Humans | 0% | 0% | 0% | 0%
------ | -- | -- | -- | --
Error | | | Avoidable bias | Avoidable bias
Training error | 1% | 1% | 10% | 10%
Error | Variance | | |
Training-dev error | 9% | 1.5% | 11% | 11%
Error | | Data mismatch | | Data mismatch
Dev error | 10% | 10% | 12% | 20%

Training-dev set: Same distribution as training set, but not used for training

**Bias/variance on mismatched training and dev/test sets**

Human level | 4%
----------- | --
Error | avoidable bias
Training set error | 7%
Error | variance
Training-dev set error | 10%
Error | data mismatch
Dev error | 12%
Error | degree of overfitting
Test error | 12%

## **Addressing data mismatch**
- **Carry out manual error analysis to try to understand difference between training and dev/test sets**

  E.g. noisy - car noise
      street numbers

- **Make training data more similar; or collect more data similar to dev/test sets**

  E.g. Simulate noisy in-car data

**Artificial data synthesis**

Combine data and arfificial synthesis

Notes: If you have 10,000 hours data but only 1 hour car noise, it may overfit to 1 hour of car noise.

# **Learning from multiple tasks**
## **Transfer learning**
Small data set: Take the exist trained model with pre-tuned weight and replace the last layer(with randomly initialized weights) according to the new requirements
Large data set: Re-train the whole model

**When transfer learning makes sense**

Transfer from A -> B
- Task A and B have the same input x.
- You have a lot more data for Task A than Task B.
- Low level features from A could be helpful for learning B.

## **Multi-task learning**
**Simplified autonomous driving example**

Feature | y(i)
------- | --
Pedestrains | 0
Cars | 1
Stop signs | 1
Traffic lights | 0
... | ...

**Neural network architecture**

If there's some feature not available(as ? mark), we can just ignore them and pick whatever is available then sum them up.
Unlike softmax regression:
One image can have multiple labels

**When multi-task learning makes sense**

- Training on a set of tasks that could benifit from having shared lower-level features.
- Usually: Amount of data you have for each task is quite similar.
- Can train a big enough neural network to do well on all the tasks.

# **End-to-end deep learning**
## **What is end-to-end deep learning?**
**Speech recognition example**

x: audio -> feature -> phonemes -> words -> y: transcript
x: audio ---------------------------------> y: transcript (end-to-end)

**Face recognition**

**More examples**

Machine translation:
English -> text analysis -> ... -> French
English -------------------------> French

Estimating child's age:
Image -> bones -> age
Image ----------> age

## **Whether to use end-to-end deep learning**
**Pros and cons of end-to-end deep learning**

Pros:
- Let the data speak
- Less hand-designing of components needed

Cons:
- May need large amount of data
- Excludes potentially useful hand-designed components

**Applying end-to-end deep learning**

Key question: Do you have sufficient data to learn a function of the complexity needed to map x to y?
- Use DL to learn individual components
- Carefully choose X -> Y depending what tasks you can get data for
