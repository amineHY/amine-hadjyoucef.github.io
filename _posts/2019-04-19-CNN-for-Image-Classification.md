---
title: Convolutional Neural Network for image classification
subtitle: A Dive into an Artificial Intelligence algorithm
layout: single
classes: wide
tag: [Machine Learning, Computer Vision, AI, Data Science]
header:
  image: /assets/images/CNN-image-classification.png
---
# A Dive into an Artificial Intelligence algorithm

This article will explain the Convolutional Neural Network (CNN) with an illustration of image classification. It provides a simple implementation of the CNN algorithm using the framework PyTorch on Python. There are many free courses that can be found on the internet. Personally, I suggest the course of *Andrej Karpathy* at Stanford university ([@karpathy](http://twitter.com/karpathy)). You will learn a lot, it is a step by step course. In addition, it provides many practical strategies to implement the CNN architecture.

Link to the course: [CS231n Convolutional Neural Networks for Visual Recognition](http://cs231n.github.io/convolutional-networks/)

# What is a Convolutional Neural Network?

Before going deep into Convolutional Neural Network, it is worth understanding their concept. CNN falls in the category of the supervised algorithms. The algorithm learns from training data, e,g, a set of images in the input and theirs associated labels at the output.

It consists of feeding the convolutional neural network with images of the training set, x, and their associated labels (targets), y, in order to learn network’s function, y=f(x). After learning the parameter of the network’s function (namely weight and bias), we test the network with an unseen images in order to predict their labels.The architecture of a Convolutional Neural Network (CNN or ConvNet)

The CNN architecture we used in this article is proposed in [this paper](http://personal.ie.cuhk.edu.hk/~ccloy/project_target_code/index.html).

Here is an illustration of the image classification using CNN architecture
<p align="center">
  <img src="https://cdn-images-1.medium.com/max/6096/1*JAoTFAsN5HCrRgrJPaBgrg.png" alt="pooling"/>
</p>

# Implementation of the CNN in Python using the PyTorch library

The network is implemented as a class called CNN. It contains two main methods. The first method (__init__) defines layers components of the network. In the second method (*forward*) we wire the network and put every component in the desired order (as shown in the picture).

The python code bellow is straightforward. The network is defined using the neural network module of Torch. Notice that we already choose hyper-parameters of the network, such as Padding (P), Stride (S) and Kernel_size (F). Also the number of filters at each layer, … .

The input image has four dimensions, (batch_size, num_channel, height, width). The algorithm outputs an array with ten values, corresponding to the score (or amount of energy) of the predicted labels of the image. Therefore, the maximum score is the predicted label (or class) to retain for the tested image.

In the following bullet we will explain the role of each layer of the algorithm:

* *Conv layer*: This is the main layer of the algorithm. It consists of extracting the key features in the input image (sharp edge, smoothness, rounded shape, …). This is done through a set of 2-dimensional convolutions of the image inthe input with one or many filters. Note that the convolution is performed simultaneously for each channel of the input image, e.g. a color image has C=3 channels, RGB: Red, Green, and Blue. The filters are set to have odd size for practical purpose CxFxF, e.g, 3x3x3, 3x5x5. The output of this operation is one scalar value, an artificial neuron. An illustrative animation for the convolution layer is given in [http://cs231n.github.io/convolutional-networks/#conv](http://cs231n.github.io/convolutional-networks/#conv).

![Source: [http://cs231n.github.io/convolutional-networks/#conv](http://cs231n.github.io/convolutional-networks/#conv)](https://cdn-images-1.medium.com/max/2000/1*qtinjiZct2w7Dr4XoFixnA.gif)

*Source: [http://cs231n.github.io/convolutional-networks/#conv](http://cs231n.github.io/convolutional-networks/#conv)*

Furthermore, the Conv layer is applied repeatedly to extract fine features that characterize the input image. The outputs of the Conv layer are called features map (or activation map), where each spatial position (or pixel) represents an artificial neuron.

* *ReLU (Rectifier Linear Units)*: It performs hard thresholding for negative values to zero, and leave positive values untouched, i,e, ReLU(x)=max(0, x). This layer preserves the dynamic range of the feature map.


<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/1*njuH4XVXf-l9pR_RorUOrA.png" alt="pooling"/>
</p>
* *Maxpooling layer*: It performs spatial down-sampling of the feature map and retains only the most relevant information. See the picture below for a visual illustration of this operation. From a practical point of view, a pooling of size 2x2 with a stride of 2 gives good results on most applications. Having said that, other types of pooling exist, e,g, average pooling, median pooling, sum pooling, …

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/1*gaD6SJ6kQNVOclE_WkwLNQ.png" alt="pooling"/>
</p>


# How about the Python implementation of CNN?

For this article, I used the neural network framework PyTorch to implement the CNN architecture detailed above.

The full code is available in my GitHub repository: [amineHY/Image-classification-of-MNIST](https://github.com/amineHY/Image-classification-of-MNIST/blob/master/pytorch_mnist_deep_cnn.ipynb)
This repository contains notebooks for image classification of the MNIST dataset. The code is quite simple to understand, hopefully, since it mentions all the layers we discussed earlier in an intuitive way.

``` python

# Implementation of CNN/ConvNet Model using PyTorch (depicted in the picture above)

class CNN(torch.nn.Module):

    def __init__(self):
        super(CNN, self).__init__()
        # L1 ImgIn shape=(?, 28, 28, 1)
        #    Conv     -> (?, 28, 28, 32)
        #    Pool     -> (?, 14, 14, 32)
        self.layer1 = torch.nn.Sequential(
            torch.nn.Conv2d(1, 32, kernel_size=3, stride=1, padding=1),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(kernel_size=2, stride=2),
            torch.nn.Dropout(p=1 - keep_prob))
        # L2 ImgIn shape=(?, 14, 14, 32)
        #    Conv      ->(?, 14, 14, 64)
        #    Pool      ->(?, 7, 7, 64)
        self.layer2 = torch.nn.Sequential(
            torch.nn.Conv2d(32, 64, kernel_size=3, stride=1, padding=1),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(kernel_size=2, stride=2),
            torch.nn.Dropout(p=1 - keep_prob))
        # L3 ImgIn shape=(?, 7, 7, 64)
        #    Conv      ->(?, 7, 7, 128)
        #    Pool      ->(?, 4, 4, 128)
        self.layer3 = torch.nn.Sequential(
            torch.nn.Conv2d(64, 128, kernel_size=3, stride=1, padding=1),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(kernel_size=2, stride=2, padding=1),
            torch.nn.Dropout(p=1 - keep_prob))

        # L4 FC 4x4x128 inputs -> 625 outputs
        self.fc1 = torch.nn.Linear(4 * 4 * 128, 625, bias=True)
        torch.nn.init.xavier_uniform(self.fc1.weight)
        self.layer4 = torch.nn.Sequential(
            self.fc1,
            torch.nn.ReLU(),
            torch.nn.Dropout(p=1 - keep_prob))
        # L5 Final FC 625 inputs -> 10 outputs
        self.fc2 = torch.nn.Linear(625, 10, bias=True)
        torch.nn.init.xavier_uniform_(self.fc2.weight) # initialize parameters

    def forward(self, x):
        out = self.layer1(x)
        out = self.layer2(out)
        out = self.layer3(out)
        out = out.view(out.size(0), -1)   # Flatten them for FC
        out = self.fc1(out)
        out = self.fc2(out)
        return out


# instantiate CNN model
model = CNN()
model
```

Note that all the number mentioned in the input of the methods is parameters. They define the CNN architecture: kernel_size, stride, padding, input/output of each Conv layer. The code below defines a class called CNN where we define the CNN architecture in order.

The output of the above code summarizes the network architecture:

``` bash
  CNN(
    (layer1): Sequential(
      (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
      (1): ReLU()
      (2): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
      (3): Dropout(p=0.30)
    )
    (layer2): Sequential(
      (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
      (1): ReLU()
      (2): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
      (3): Dropout(p=0.30)
    )
    (layer3): Sequential(
      (0): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
      (1): ReLU()
      (2): MaxPool2d(kernel_size=2, stride=2, padding=1, dilation=1, ceil_mode=False)
      (3): Dropout(p=0.30)
    )
    (fc1): Linear(in_features=2048, out_features=625, bias=True)
    (layer4): Sequential(
      (0): Linear(in_features=2048, out_features=625, bias=True)
      (1): ReLU()
      (2): Dropout(p=0.30)
    )
    (fc2): Linear(in_features=625, out_features=10, bias=True)
  )
```



#  Importing MNIST Dataset

In order to acquire the MNIST images, we use a method of torchvision library. Just copy paste this code to download the data. Basically, two datasets are loaded. The training dataset serves as ground truth to compute the network parameters. The testing images

``` python
  import torch
  import torchvision.datasets as dsets


  batch_size = 32

  # MNIST dataset
  mnist_train = dsets.MNIST(root='MNIST_data/',
                            train=True,
                            transform=transforms.ToTensor(),
                            download=True)

  mnist_test = dsets.MNIST(root='MNIST_data/',
                           train=False,
                           transform=transforms.ToTensor(),
                           download=True)

  # dataset loader
  data_loader = torch.utils.data.DataLoader(dataset=mnist_train,
                                            batch_size=batch_size,
                                            shuffle=True)
```

# Training the classification model

This is an essential stage of a supervised algorithm. By feeding the algorithm with many examples of image and their associated labels, we teach the algorithm to find patterns of each class. This is done by computing filter’s parameters (weight and bias).

The training of the network is composed of two major steps, forward and backward:

* During the forward pass, we feed the network by images of the training set and compute the features map till the end of the network, then we compute the loss function to measure how far / close the solution (predicted label) is from the ground truth label.

* The backward pass performs the computation of the loss function’s gradient and updates the filters' parameters.

We also need to define an loss function, e.g. [Cross-entropy loss function](https://en.wikipedia.org/wiki/Cross_entropy#Cross-entropy_error_function_and_logistic_regression), and an optimization algorithm, such as [Gradient descent, SGD, Adam](https://arxiv.org/pdf/1412.6980.pdf) (Adaptive moment estimation[)](https://arxiv.org/pdf/1412.6980.pdf)…

![[Source: https://arxiv.org/pdf/1412.6980.pdf](https://arxiv.org/pdf/1412.6980.pdf)](https://cdn-images-1.medium.com/max/2000/1*MOVbwX27vXjzYmfV1NSswQ.png)*[Source: https://arxiv.org/pdf/1412.6980.pdf](https://arxiv.org/pdf/1412.6980.pdf)*

**What to remember** for training a deep learning algorithm:

— Initialize the parameters (weights : w, bias: b)
 — Optimize the loss iteratively to learn parameters (w,b)
 — Compute the loss function and its gradients
 — Update parameters using an optimization algorithm (e,g, Adam)
 — Use the learned parameters to predict the label for a given input image

### Python implementation of training CNN in Python using PyTorch
``` python
print('Training the Deep Learning network ...')

learning_rate = 0.001
criterion = torch.nn.CrossEntropyLoss()    # Softmax is internally computed.
optimizer = torch.optim.Adam(params=model.parameters(), lr=learning_rate)

train_cost = []
train_accu = []
batch_size = 32
training_epochs = 15
total_batch = len(mnist_train) // batch_size

print('Size of the training dataset is {}'.format(mnist_train.data.size()))
print('Size of the testing dataset'.format(mnist_test.data.size()))
print('Batch size is : {}'.format(batch_size))
print('Total number of batches is : {0:2.0f}'.format(total_batch))
print('\nTotal number of epochs is : {0:2.0f}'.format(training_epochs))

def compute_accuracy(Y_target, hypothesis):
    Y_prediction = hypothesis.data.max(dim=1)[1]
    accuracy = ((Y_prediction.data == Y_target.data).float().mean())    
    return accuracy.item()


for epoch in range(training_epochs):
    avg_cost = 0
    for i, (batch_X, batch_Y) in enumerate(data_loader):

        # Select a minibatch
        X = Variable(batch_X)    # image is already size of (28x28), no reshape
        Y = Variable(batch_Y)    # label is not one-hot encoded

        # initialization of the gradients
        optimizer.zero_grad()

        # Forward propagation: compute the output
        hypothesis = model(X)

        # Computation of the cost J
        cost = criterion(hypothesis, Y) # <= compute the loss function

        # Backward propagation
        cost.backward() # <= compute the gradients

        # Update parameters (weights and biais)
        optimizer.step()

        # Print some performance to monitor the training
        train_accu.append(compute_accuracy(Y, hypothesis))
        train_cost.append(cost.item())   
        if i % 200 == 0:
            print("Epoch= {},\t batch = {},\t cost = {:2.4f},\t accuracy = {}".format(epoch+1, i, train_cost[-1], train_accu[-1]))

        avg_cost += cost.data / total_batch

    print("[Epoch: {:>4}], averaged cost = {:>.9}".format(epoch + 1, avg_cost.item()))


print('Learning Finished!')
```

## Monitoring the training by plotting the Loss-Function and the Accuracy

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/1*us1x6tgDfYBRhme0j3dgFg.png" alt="pooling"/>
</p>

Plotting these plots help monitor understanding the convergence of the algorithm. The loss plot is decreasing during the training which a what we want since the goal of the optimization algorithm (Adam) is to minimize the loss function. On the right, the plot shows the evolution of the classification accuracy during the training. The more we train the algorithm, the better the classification accuracy. Notice the fluctuation of the accuracy between ~90 and 100 %. Better tuning of hyper-parameters will provide a precise classification.

## Display of Classification Results

The classification algorithm is able to understand the content of these images thanks to the training of CNN.

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/1*bgm3yCrLs1wvE9UskdBhRA.png" alt="pooling"/>
</p>


# Conclusion
This article attempts to explain briefly the convolutional neural network without going deep into mathematical development. An illustration is provided at each step with a visual explanation, as well as an application of image classification of MNIST dataset. Finally, a python implementation using PyTorch library is presented in order to provide a concrete example of application. Hopefully, you will find it interesting and easy to read.
