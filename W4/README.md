# Convolutional Neural Network

URL: [Deep Learning Specialization Course 4](https://www.youtube.com/playlist?list=PLkDaE6sCZn6Gl29AoE31iwdVwSG-KnDzF)

## W1L01 Why NN cannot be used to process large images

Imagine we have a image with 1000 * 1000 * 3 pixels. Then it has:

* 3M input features
* Suppose the first hidden layer has 100 neurons, it will have w matrix with size (100, 3M) -> computationally difficult

## W1L02-03 Edge detection using convolution operation

Instead of using NN, we can use convolution operation to detect edges:

* It uses a filter which is e.g. (3, 3) matrix 
* The filter is multiplied to each frame in the image
* Shifting frames creates another matrix which show the edge

A typical filter to detect vertical edge is:
```
1,0,-1
1,0,-1
1,0,-1
```

In computer vision, we can use backward propation to learn the parameters in a filter instead of using a filter with fixed value. The convolutional operation is the same.

## W1L04-05 Padding & Stridding

### Padding
With convolutional operation, pixels at the edge of the image are used less than those in the middle. To Padding (add extra pixels at the outer frame of the image) provides two advantages:

* Facilitate more balanced use of images in the middle and on the edge
* Does not shrink the image size after convolution: n*n + f*f -> (n-f+1) * (n-f+1), where n, f are image and filter size respectively

Terminology:

* Valid convolution: no padding
* Same convolution: convolution with padding so that resulted image size the same dimention as input image (p=(f-1)/2)

### Stridding

Strided Convolution means hopping frame when shifting the filter. In summary, an n*n image + f*f filter with padding p and stride s returns a image with size: (n+2p-f)/s + 1 * (n+2p-f)/s + 1

## W1L06 Convolution over Volumes
A RGB image has 3 channels, which makes it a 3-d array with dimension (H, W, 3). Correspondingly, the filter needs to be 3-d as well. The convolutional operation results in a 2-d array.

Example: 
* RGB image with size (6, 6, 3), filter with size (3, 3, 3)
* Operation: superimpose filter on RGB image, element-wise multiplication of filter * image, summation of all 3 layers, then slide the filter one position right
* Result: (4, 4) array

If we have multiply filters (for detection of multiple pattern), the resultant image with have size (4, 4, n_number_of_filters)

## W1L07-08 One layer of a convolutional net

Some comparison / similarity of NN we learned in W3 vs CNN in W3
|NN|CNN|
|--|---|
|z[1] = w[1] * x + b[1]|Convolutional operation of input * filter|
|Activation function sigma| Relu function|

The order of operation:
1. Convolutional operation of input * filter of a 2-darray per filter -> e.g. dimension = (4,4)
2. Relu (activation function) apply on 1) + b_filter for each filter -> dimension = (4,4)
3. Result is a stacked -> dimension = (4, 4, n_filters)
4. The result will be the input of next convolutional layer
5. In the final layer, the input array will be flattened to 1d, then feed to logistic/softmax function to get y_hat

The highlight is no matter how big the input data is, number of parameters is not too big because it is independent of input dimension. If we have 10 filters each with (3, 3) size, the number of parameters is 10 * (3*3*3+1) = 270, when channel size=3 (RGB).

NN has too many parameters! 

