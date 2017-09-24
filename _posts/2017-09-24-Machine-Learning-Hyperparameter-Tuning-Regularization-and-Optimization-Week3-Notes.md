---
layout: post
title: "Machine Learning: Hyperparameter tuning, Regularization and Optimization Week3 Notes"
date: 2017-09-24
categories: blog
tags: [Machine Learning]
description: "Notes for Machine Learning Course: Hyperparameter tuning, Regularization and Optimization"
---

# **Hyperparameter Tuning**
## **Tuning Process**
**Try Random Values: Don't use a grid**

**Coarse to Fine**
After found the best performance area, zoom in to the area and sample densely

## **Using an Appropriate Scale to Pick Hyperparameters**
**Appropriate Scale for Hyperparameters**
- For number of layers or number of units in each layer, we can choose random value on linear scale
- For learning rate, it's better to choose random value based on log scale
$$r=-4*np.random.randn()$$
$$\alpha=10^r$$

**Hyperparameters for Exponentially Weighted Averages**
$$r\in[-3,-1]$$
$$1-\beta=10^r$$
$$\beta=1-10^r$$

## **Hyperparameters Tuning in Practice: Pandas vs. Caviar**
**Re-test Hyperparameters Occasionally**
**Babysitting One Model OR Training Many Models in Parallel**

# **Batch Normalization**
## **Normalizing Activations in a Network**
**Normalizing Inputs to Speed up Learning**

**Implementing Batch Norm**

Given some intermediate values in NN,
$$z^{(1)}, ..., z^{(n)}$$
$$\mu=\frac{1}{m}\sum_i z^{(i)}$$
$$\delta^2=\frac{1}{m}\sum_i (z_i-\mu)^2$$
$$z_{norm}^{(i)}=\frac{z^{(i)}-\mu}{\sqrt{\delta^2+\epsilon}}$$
$$\tilde{z}^{(i)}=\gamma z_{norm}^{(i)}+\beta$$
where $$\gamma, \beta$$ learnable parameters of model

If
$$\gamma=\sqrt{\delta^2+\epsilon}$$
$$\beta=\mu$$
then
$$\tilde{z}^{(i)}=z^{(i)}$$

Use $$\tilde{z}^{[l](i)}$$ Instead of $$z^{[l](i)}$$

## **Fitting Batch Norm into a Neural Network**
### **Adding Batch Norm to a Network**
$$X \rightarrow^{W^{[1]}, b^{[1]}} \rightarrow Z^{[1]} \rightarrow_{BatchNum(BN)}^{\beta^{[1]}, \gamma^{[1]}} \rightarrow \tilde{Z}^{[1]} \rightarrow a^{[1]}=g^{[1]}(\tilde{Z}^{[1]}) \rightarrow^{W^{[2]}, b^{[2]}} \rightarrow Z^{[2]} ...$$
Parameters: $$W^{[1]}, b^{[1]}, W^{[2]}, b^{[2]}... \beta^{[1]}, \gamma^{[1]}, \beta^{[2]}, \gamma^{[2]}...$$
$$d\beta^{[l]}$$
$$\beta^{[l]}=\beta^{[l]}-\alpha d\beta^{[l]}$$
$$tf.nn.batch.normalization$$

### **Working with Mini-batches**
$$X^{{1}} \rightarrow^{W^{[1]}, b^{[1]}} \rightarrow Z^{[1]} \rightarrow_{BatchNum(BN)}^{\beta^{[1]}, \gamma^{[1]}} \rightarrow \tilde{Z}^{[1]} \rightarrow a^{[1]}=g^{[1]}(\tilde{Z}^{[1]}) \rightarrow^{W^{[2]}, b^{[2]}} \rightarrow Z^{[2]} ...$$
$$X^{{2}} \rightarrow^{W^{[1]}, b^{[1]}} \rightarrow Z^{[1]} \rightarrow_{BatchNum(BN)}^{\beta^{[1]}, \gamma^{[1]}} \rightarrow \tilde{Z}^{[1]} \rightarrow a^{[1]}=g^{[1]}(\tilde{Z}^{[1]}) \rightarrow^{W^{[2]}, b^{[2]}} \rightarrow Z^{[2]} ...$$
Parameters: $$W^{[1]}, b^{[1]}, W^{[2]}, b^{[2]}... \beta^{[1]}, \gamma^{[1]}, \beta^{[2]}, \gamma^{[2]}...$$
Since ~~b~~
The functions become:
$$Z^{[l]}=W^{[l]}a^{[l-1]}$$
$$Z_{norm}^{[l]}$$
$$\tilde{Z}^{[l]}=\gamma^{[l]}Z_{norm}^{[l]}+\beta^{[l]}$$
where
$$Z^{[l]}, \beta^{[l]}, \gamma^{[l]}$$
has dimension
$$(n^{[l]}, 1)$$

### **Implementing Gradient Descent**
> for t = 1 ... num Mini Batches
>> **Compute forward prop on** $$X^{{t}}$$
>>> In each hidden layer, use BN to replace $$Z^{[l]} with \tilde{Z}^{[l]}$$

>> **Use back prop to compute** $$dW^{[l]}, d\beta^{[l]}, d\gamma^{[l]}$$
>> **Update parameters**
>> $$W^{[l]}:=W-\alpha dW^{[l]}$$
>> $$\beta^{[l]}:=\beta-\alpha d\beta^{[l]}$$
>> $$\gamma^{[l]}:=\gamma-\alpha d\gamma^{[l]}$$

**This also works with moment, RMSprop, Adam**

## **Why Does Batch Norm Work?**
**Especially to the later layers of the NN, the output from earlier layers don't get to shift around as much.**

**Batch Norm as Regularization**
- Each mini-batch is scaled by the mean/variance computed on just that mini-batch
- This adds some noise to the values for Z on lth layer within that minibatch. So similar to dropout, it adds some noise to each hidden layer's activations.
- This has a slight regularization effect.

## **Batch Norm at Test Time**
**Use exponential weighted average across mini-batch**
$$X^{{1}}, X^{{2}}, X^{{3}}, ...$$
$$\mu^{{1}[l]}, \mu^{{2}[l]}, \mu^{{3}[l]}, ...$$
$$\theta_1, \theta_2, \theta_3, ...$$
$$\delta_1, \delta_2, \delta_3, ...$$
$$Z_{norm}=\frac{Z-\mu}{\sqrt{\delta^2+\epsilon}}$$
where $$\mu$$ is the latest one
Then calculate,
$$\tilde{Z}=\gamma Z_{norm}+\beta$$

## **Multi-class Classification**
### **Softmax Regression**
**Softmax Activation Function**
$$Z^{[l]}=W^{[l]}a^{[l-1]}+b^{[l]} ----- (4,1)$$
$$t=e^{Z^{[l]}} ----------- (4,1)$$
$$a^{[l]}=\frac{e^{Z^{[l]}}}{\sum_{j=1}^4 t_i}, a^{[l]}=\frac{t_i}{\sum_{j=1}^4 t_i} -- (4,1)$$

### **Training a Softmax Classifer**
**Understanding Softmax**

**Loss Function**
$$L(\hat{y}, y)=-\sum_{j=1}^4 y_j log\hat{y_j}$$
$$J(w^{[l]}, b^{[l]}, ...)=\frac{1}{m}\sum_{i=1}^m L(\hat{y}^{(i)}, y^{(i)})$$

**Vectorization**
$$Y --- [4, m]$$
$$\hat{Y} --- [4, m]$$

**Gradient Descent with Softmax**
**Backprop**
$$\frac{\alpha J}{\alpha z^{[L]}} = dz^{[L]}=\hat{y}-y$$

# **Introduction to Programming Frameworks**
## **Deep Learning Frameworks**
**Choosing Deep Learning Frameworks**
- Ease of programming (development and deployment)
- Running speed
- Truly open (open source with good governance)

## **TensorFlow**
### **Motivation Problem**
**Cost Function**
$$J(w)=w^2-10w+25$$

**Implement in TensorFlow**
```python
coefficients = np.array([1.], [-10.], [25.])

w = tf.Variable(0, dtype=tf.float32)
x = tf.placeholder(tf.float32, [3,1])
#cost = tf.add(tf.add(w**2, tf.multiply(-10.,w)), 25)
#cost = w**2 - 10*w + 25
cost = x[0][0]*w**2 + x[1][0]*w + x[2][0]
train = tf.train.GradientDescentOptimizer(0.01).minimize(cost)

init = tf.global_variables_initializer()
#session = tf.Session()
#session.run(init)
#print(session.run(w))
#Previous three lines can be replaced with
with tf.Session() as session:
    session.run(init)
    print(session.run(w))
session.run(train, feed_dict={x:coefficients})
print(session.run(w))
for i in range(1000):
    session.run(train)
print(session.run(w))
```
