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
 

