---
layout: post
title: "Transformer Learning Notes"
date: 2025-07-14
categories: blog
tags: [GenAI]
description: "Notes for learning the Transformer architecture"
---

# **Transformer**
[Transformer Architecture](https://upload.wikimedia.org/wikipedia/commons/thumb/3/34/Transformer%2C_full_architecture.png/1280px-Transformer%2C_full_architecture.png)
Encoder & Decoder stacks + Self Attention

## **Why Transformer**
- Parallel Processing of Words (vs RNN in sequence)
- Capturing Long-Range Dependencies
- Scalability and Performance
- Improved Contextual Representation (Self Attention)
- Facilitation of Transfer Learning

## **Building Blocks**
### **Core Components**
- Input Embedding: Convert words into numerical vectors
- Positional Encoding: Add sequence information to input
- Multi-Head Self-Attention: Allows the model to weigh input parts for context

### **Encoder and Decoder Layers**
- Encoders: Process the input text
- Decoders: Generate the output sequence
- Interconnected through attention mechanisms

### **Output Generation**
- Linear Layer: Maps decoder output to a higher-dimensional space
- Softmax Layer: Converts the output into probabilities for each token

LLM Emerging Properties
- Contextual Understanding
- Zero-Shot Learning
- Complex Problem-Solving
- Artistic Creation
- Technical Coding

# **The Attention Mechanism**
- Weighted Importance: Determines the relevance of each part of the input to the task at hand
- Contextual Awareness: Helps in understanding the context by considering the entire input sequence
- Sequence Learning: Enables the capture of relations and dependencies between elements in the sequence, regardless of their position

**Self-attention is a mechanism in a neural network that allows each part of the input data, like words in a sentence, to look at other parts of the sentence to better understand the context.**

## **How Does Self Attention Work**
Step 1: Create 3 vectors from the input of the encoder (embedding), for each word
Query Vector: Relevant Inbound Value
Key Vector: Relevant Outbound Value
Value Vector: Actual Value

Step 2: Computing attention scores, which determine how much each word in a sentence should focus on all other words. This is done using the dot product.

Step 3: Scale down the attention scores by dividing them by the square root of the dimension of the K vector

Step 4: Apply the softmax function to the scaled scores to get the attention weights, ensuring they are positive and sum up to 1.

Step 5: Multiply each V vector by its corresponding attention weight, preparing them to be summed.

Step 6: Sum the weighted V vectors to get the output for the current word, which then passes to the next layer in the network.

## **Multi-Head Attention**
Self-attention process is split into multiple "heads". Each head independently attend to information from different representation subspace at different positions.
- Diversified Focus
- Richer Representations
- Improved Learning

<iframe width="420" height="315" src="https://youtu.be/eMlx5fFNoYc?si=tjKK6cej2zIPDCX_" frameborder="0" allowfullscreen></iframe>