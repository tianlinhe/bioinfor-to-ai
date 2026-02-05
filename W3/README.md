# Neural Network and Deep Learning Notes
URL: [Neural Network and Deep Learning](https://www.youtube.com/playlist?list=PLkDaE6sCZn6Ec-XTbcX1uRg2_u4xOEky0)
## W1: Introduction
### W1L02: What is a Neural Network
A simplest neuron can be a Relu (rectified linear unit) as **activation function** with one input and output. A more complex neuron netwokrs can take more features as input.

### W1L03: Supervised Learning and NN
Common types of NN:
* Standard NN: prediction of property price
* Convolutional NN: image
* Recurrent NN: sequential data such as text and audio

Data structure:
* Structured data: tabular
* Unstructured data: audio, image, text

### W1L04: Why is deep learning taking off?

Performance keeps improving from:
* Having more data
* Having bigger NN
So if you only have a small dataset, traditional ML (SVM, logistic regression) probably work better than NN.

Scale drives deep learning progress:

* Data (we have more data thanks to digitalization)
* Computation (GPU)
* Algorithms (eg.to speed up computation by using relu with faster gradient descent instead of sigmoid)

## W2

### W2L02-03 Logistic regression
Recap of logistic regression: given parameters w and b, output of linear regression is: y = w ^ T x + b.
Logistic regression takes a sigmal function of RHS so that LHS is between 0 and 1.
The sigmal function (expressed in z) is sigma(z) = 1 / (1 + e ^ -z). It has this behaviour:

* when z is larger: sigma(z) -> 1
* when z is small: sigma(z) -> 0

Cost function (for a single training sample) is NOT the minimum least square, because it is non-convex will make gradient descent difficult. We will use another cost function L = -(y * log(y_hat) + (1-y) * log(1-y_hat)), with the intuition being:

* y = 1 -> L = -log(y_hat) -> To minimize L, y_hat needs to be large (close 1)
* y = 0 -> L = -log(1-y_hat) -> To minimize L, y_hat needs to be small (close to 0)

So the cost functions (for all training samples *m*) is the summation over L divided by m.

### W2L07-10 Computation Graph
Computation graph can be used to derive gradient descent. How it works:
* From left to right (forward propagation): compute value of cost function by each algebra unit  
* From right to left (backward propagation): compute derivative of cost function, which is typical d(final output variable)/d(inital input variable) using **chain rule**

An example of how to compute gradient in logistic regression using computation graph:

Write logistic regression (with two features x_1, x_2) in three propagation steps:
1. z = w_1 * x_1 + w_2 * x_2 + b, where w_1, w_2, b are parameters
2. a = sigma(z) = 1 / (1 + e ^ -z)
3. Loss function L(a, y) = -(y * log(y_hat) + (1-y) * log(1-y_hat)), where y_hat = a

Back propagation calculates dL/dw1 = x1 * dz, where dz = a - y. Therefore we can update w1 during gradient descent as: w1 := w1 + dL/dw1 = w1 + x1 * dz.

In reality when you can implement logistic regression on m examples by aggregation and average through a double nested for-loop:
```
J, dw1, dw2, b = 0, 0, 0, 0
for i = 1 to m: # first layer of for-loop over samples
  z_i = w * x_i + b
  a_i = sigma(z_i)
  J_i = -(y_i)*log(a_i) + (1-y_i)*log(1-a_i)
  dz_i = a_i - y_i # obtain it from backpropagation formula
  for parameter from 1 to 1: # second layer of for-loop over variables w
    dw1 += x1_i * dz_i # obtain it from backpropagation formula
    ...
J = J / m # cost function is the mean of all samples
dw_1 = dw1 / m
```

## W3

### W3L01-02 Neural Network Overview

* Each layer is a compuation of 1) z=wx+b 2) a=sigma(z);
* Each layer uses the output of the previous layer a as x;
* L(a, y) is computed as the final output of the NN.

The number of nodes in each layer does NOT necessarily equal to number of features.

### W3L02-03 An example of NN with two layers and one training sample 

The example NN has 2 layers: input (not counted), hidden, and output layers.

The input layer is also called a[0], in which a stands for activation. Therefore hidden layer is a[1][i], where i refers to the nth node.

Suppose the NN has 3 features, its hidden layer 1 has 3 nodes

* Dimension of z[1] = w[1] * x + b[1] = (4,3) * (3,1) + (4,1) = (4,1)
* Dimension of a[1] = sigma(z[1]) = (4,1)

Its output layer 2 has only one node as output:

* Dimension of z[2] = w[2] * a[1] + b[2] = (1,4)*(4,1) + (1, 1) = (1,1)
* Dimension of a[2] = (1, 1)

### W3L06-07 Activation Functions
|Activation Function|Formula|Pros|Cons|Usage|
|-------------------|-------|----|----|-----|
|Sigma|1/(1+e^-z)| |slope=0 at both ends, learning very slow|Almost never use besides the output layer of binary classification|
|tanh(z)||Mean is 0, good for learning|Shifted sigma center at origin, span from -1 to 1|slope=0 at both ends, learning very slow|Superior than sigma|
|Relu|max(0,z)| |Not differentiable at z=0|Most common|
|Leaky Relu|Relu with a small slope when x<0| | |Sometimes worth trying|

Note: We almost never use linear function as activation function because it is equivalent to blending the whole NN into one neuron. The rare occasional to use linear function is regression problem (e.g. prediction of house price).

### W3L09-10 Gradient Descent
Backward propagation computes derivatives (gradient) using chain rules, gradient descent uses the calculated gradient to take "steps" to minimize the loss function.

### W3L11 Random Initialization

For logistic regression, we initialize w with np.random.ranint * 0.01:

* Gaussian distrubution
* cannot all 0, otherwise symmetrical issue
* multiply with a small number to avoid saturation at the ends of logistic regression (it doesn't matter if the activation function is NOT logistic regression)

We can initialize b with all zeros because b does not have symmetrical issue.

