---
layout: post
title: "Machine Learning: Sequence Models Week2 Notes"
date: 2018-06-25
categories: blog
tags: [Machine Learning]
description: "Notes for machine learning course: Sequence Models"
---

# **Introduction to Word Embeddings**
## **Word Representation**
$$V = [a, aaron, ..., zulu, <UNK>]$$
1-hot representation

**Featurized representation:word embedding**

Feature| Man(5391) | Woman(9853) | King(4914) | Quene(7157) | Apple(456) | Orange(6257)
-------|-----------|-------------|------------|-------------|------------|-------------
Gender |-1         |1            |-0.95       |0.97         |-0.01       |0.00         
Royal  |0.01       |0.02         |0.93        |0.95         |-0.01       |0.00
Age    |0.03       |0.02         |0.7         |0.69         |0.03        |-0.02
Food   |0.04       |0.01         |0.02        |0.01         |0.95        |0.97
Size|...|...|...|...|...|...
Cost|...|...|...|...|...|...
Alive|...|...|...|...|...|...
Verb|...|...|...|...|...|...

**Notation**|$$e^{5391}$$|$$e^{9853}$$|...|...|...|...

**Visualizing word embeddings**

t-SNE algorithm
![](https://lvdmaaten.github.io/tsne/examples/SP500_tsne.png)

## **Using word embeddings**
**Named entity recognition example**

Sally Johnson is an orange farmer
Robert Lin is an apple farmer
xxx xxx is a durian cultivator

**Transfer learning and word embeddings**
1. Learn word embeddings from large text corpus. (1-100B words)
    (Or download pre-trained embedding online)
2. Transfer embedding to new task with smaller training set.
    (say, 100k words)
3. Optional: Continue to finetune the word embeddings with new data.

Word embeddings tend to make the biggest difference when the task has a relatively smaller training set.

Useful for: named entity recognition, text summarization, co-reference resolution, parsing, etc.

Less useful for: language modeling, machine translation, especially when those tasks already have a lot of data.

**Relation to face encoding**

## **Properties of word embeddings**
**Analogies**

Feature| Man(5391) | Woman(9853) | King(4914) | Quene(7157) | Apple(456) | Orange(6257)
-------|-----------|-------------|------------|-------------|------------|-------------
Gender |-1         |1            |-0.95       |0.97         |0.00        |0.01         
Royal  |0.01       |0.02         |0.93        |0.95         |-0.01       |0.00
Age    |0.03       |0.02         |0.7         |0.69         |0.03        |-0.02
Food   |0.09       |0.01         |0.02        |0.01         |0.95        |0.97
Notation|e5391(eman)|ewoman      |eking       |equeen       |eapple      |eorange

Man -> Woman as King -> ?

$${e_{man} - e_{woman} \approx \left[\begin{array}{cccc}-2\\0\\0\\0\end{array}\right]}$$

$${e_{king} - e_{queen} \approx \left[\begin{array}{cccc}-2\\0\\0\\0\end{array}\right]}$$

**Analogies using word vectors**

$${e_{man} - e_{woman} \approx e_{king} - e_{w}}$$

Find word Wi

$${argmax_w sim(e_{w}, e_{king} - e{man} + e{woman})}$$

**Cosine similarity**

$${sim(e_{w}, e_{king} - e_{man} + e_{woman})}$$

$${sim(u, v) = \frac{u^{T}v}{||u||_{2}||v||_{2}}}$$

Man:Woman as Boy:Girl

Ottawa:Canada as Nairobi:Kenya

Big:Bigger as Tall:Taller

Yen:Japan as Ruble:Russia

## **Embedding matrix**
**Embedding matrix**

$$E*O_{6257} = [] = e_{6257}$$

$$E*O_{j} = e_{j} = embedding for word j$$

# **Learning Word Embeddings: Word2vec & GloVe**
## **Learning word embeddings**
**Neural language model**

I      |want       |a            |glass       |of           |orange      |_______.
-------|-----------|-------------|------------|-------------|------------|------------
4343   |9665       |1            |3852        |6163         |6257        |

I         o4343 ---> E ---> e4343 ---> O
want      o9665 ---> E ---> e9665 ---> O
a         o1    ---> E ---> e1    ---> O ---\ Softmax
glass     o3852 ---> E ---> e3852 ---> O ---/ 10000
of        o6163 ---> E ---> e6163 ---> O
orange    o6257 ---> E ---> e6257 ---> O

**Other context/target pairs**

I want a glass of orange juice to go alone with my cereal.

Context:

Last 4 words: a glass of orange ?

4 words on left & right: a glass of orange ? to go along with my ceral

Last 1 word: orange ?

Nearby 1 word(skip gram): glass ... ?

## **Word2Vec**
**Skip-grams**

**Model**

Vocab size = 10000k

x ---> y

Context c ("orange") 6257 ---> Target t ("juice") 4834

Oc ---> E ---> ec ---> O softmax ---> y hat

ec = Eoc

**Problems with softmax classification**

Expensive calculation --- Do Hierarchical softmax classifier (binary classifier with common words in the upper levels)

How to sample the context c?
~~Randomly choose from the context~~ will result in getting the common words like "the", "of", "a"...
In practice, different heuristics will be used to balance out something from the common words together with the less common words.

## **Negative Sampling**
**Defining a new learning problem**

Context |Word   |Target
--------|-------|-------
orange  |juice  |1
orange  |king   |0
orange  |book   |0
orange  |the    |0
orange  |of     |0

k pairs with target 0 where k = 5 to 20 for smaller data sets and 2 to 5 for large data sets
Inputs x as context and word, output y as target

**Model**

Regression model
Instead of doing 10000 calculations, it's doing k + 1

**Selecting negative examples**

Somewhere in-between the extreme of taking uniform distribution and the other extreme of justing taking whatever was observed distribution in training set

## **GloVe word vectors**
**GloVe (global vectors for word representation)**

c, t

Xij = #times i appears in the context of j

where i is t, j is c

Xij = Xji

**Model**

minimize $$\sum_{i=1}^{10,000}\sum_{j=1}^{10,000}f(x_{ij})(\theta_i^Te_j + b_i + b_j^{'} - logX_{ij})^2$$

where $$f(x_{ij})$$ is the weighting function

$$\theta_i$$ and $$e_j$$ are symmetric

**A note on the featurization view of word embeddings**

Word & Properity |Man (5391) |Woman (9853) |King (4914) |Queen (7157)
--------|-----------|-------------|------------|------------
Gender  |-1   |1    |-0.95 |0.97
Royal   |0.01 |0.02 |0.93  |0.95
Age     |0.03 |0.02 |0.70  |0.69
Food    |0.09 |0.01 |0.02  |0.01

minimize $$\sum_{i=1}^{10,000}\sum_{j=1}^{10,000}f(x_{ij})(\theta_i^Te_j + b_i + b_j^{'} - logX_{ij})^2$$

# **Applications using Word Embeddings**
## **Sentiment Classfication**
**Sentiment classification problem**

x|y
-|-
The desert is excellent.|4 stars
Service was quite slow.|2 stars
Good for a quick meal,but nothing special.|3 stars

**Simple sentiment classification model**

The|dessert|is|excellent|4 stars
---|-------|--|---------|-------
8928|2468|4694|3180

The|O8928 ---> E|e8928
---|------------|-----
desert|O2468 ---> E|e2468
is|O4694 ---> E|e4694
excellent|O3180 ---> E|e3180

**Disadvantage: ignore word order**

**RNN for sentiment classification**

Use many-to-one RNN

## **Debiasing word embeddings**
**The problem of bias in word embeddings**

Man:Woman as King:Queen

Man:Computer_Programmer as Woman:~~Homemaker~~

Father:Doctor as Mother:~~Nurse~~

Word embeddings can reflect gender, ethnicity, age, sexual orientation, and other biases of the text used to train the model.

**Addressing bias in word embeddings**
1. Identify bias direction.
ehe - eshe
emale - efemale
average
2. Neutralize: For every word that is not definitional, project to get rid of bias.
3. Equalize pairs.
