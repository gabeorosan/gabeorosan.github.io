---
layout: post
title:  "CNN From Scratch - Code, Lessons and Insights"
excerpt: "I did it. I finally finished building a ConvNet from scratch, only using numpy. This is why I will probably
never do it again."
date:   2022-02-26 12:20:00
---

[Source Code](https://github.com/gabeorosan/cnn\_from\_scratch)

**Why?** I wanted to gain a deeper understanding of not just the mathematics behind neural networks, but actually get a
feel for the architecture and be able to visualize the operations being performed. In hindsight, it probably would have
been better to start with a Pytorch model and work incrementally towards a net of my own, using visualizations and test
cases along the way. But anyway, here's what I took away from the project:

### What I learned

-   Convolutions are cool! I am intrigued by the idea of using convolutions to extract features over different
    dimensions, and they combine neatly with max pooling and relu operations to shape the data in a way that is useful
    for the final linear layer. Building this from scratch helped me appreciate the properties of stride length,
        padding, and back propogration (did you know the gradient with respect to the convolution input is computed by
        convolving the loss derivative on the 180 degree rotated filter? Read more
        [here](https://pavisj.medium.com/convolutions-and-backpropagations-46026a8f5d2c)).<br><br>

-   Looping is slow. I obviously knew writing for loops to iterate over 4-d matrices in python wouldn't be the most
    efficient, but it surprised me how long it took to send an image through. It ended up taking about a second for a
    single forward and backward pass, and since I only accounted for a batch size of 1, this meant my model could hardly
    learn anything even once I had all the gradients right. This gave me an appreciation for the importance of being
    able to formulate the operations in a model in a way that is efficient, and made me curious to learn more about the
    hardware implementation of different kinds of operations.


### Lessons

- There are an incredible number of ways to mess up convolution operations in numpy. Like, wow. I have written a linear
  neural network before, and I kind of had the impression going into this that you basically just have to figure out
  which sizes line up so that you can dot multiply to get the output you want. But I was quite wrong; convolutions were
  hard for me to wrap my head around at first because the intuition for doing operations on matrices with multiple
  dimensions can take a while to develop. For example: what kinds of rearrangements can you do to a convolutional filter
  matrix and the input matrix without changing the result of the convoluation operation? I ended up having to test out
  different matrices and reshapings to understand how I could and couldn't formulate the convolution operation, and I'm
  sure the way I settled on isn't the best.<br><br> 
  
- Not writing evaluation methods is a huge mistake. Don't get me wrong, there are many situations where you don't need
  thorough error-checking and unit tests. But I have definitely made a note to myself to do so in the future, especially
  when trying to sort through a lot of variables and operations at once. Not like array manipulation is the hardest
  thing in the world, but like I said, I surprised myself with the number of invisible ways to make tiny mistakes, and I
  ended up training alongside a PyTorch CNN to check by gradients against anyway, so I really should have just done that
  from the start.

### Insights

  - Partial differentials are amazing, but 'normal' ways of computing gradients are mostly feature engineering. Why should the
  batch size be a predetermined number, why should all the inputs in the batch be weighted equally, why should the
  learning rate be a linear scalar (or quadratic, or any other predetermined function)? The way I see it, there is a lot
  to be gained by having smarted gradient selectivity and distribution. One of the reasons I say this is because I
  decided to use batch sizes of one, and I noticed that the PyTorch model I trained alongsize pretty much had no
  learning rate that would make it learn much of anything within the number of input samples I could process (my code is
  extremely slow since I wasn't thinking about efficiency at all). We learn from much fewer examples than machine
  learning models, and we tend to learn more (at least in some ways) the more we know. Smarter gradient operations could
  be a way of trying to adopt that trait in machine learning models.<br><br>

  - Convolutional architectures, and dare I say it, traditional architectures in general, are needlessly arbitrary and force
  machine learning as a field to cling to specific and narrow problems. On one hand, computers are inherently digital so
  you need to provide the input data in a series of numbers. This makes it very natural to think about learning
  arithmetical operations to combine the inputs, but in my mind, this is going the wrong direction. Computers are
  ultimately operating on the bit level, and by representing inputs as decimal numbers, you lose a lot of computing
  power. I see no reason why the layers of a network should not be composed of logical operators and bits. This does
  produce a lot of interesting questions about the input processing of the model, but I tend to think that it would be
  better to formulate ways of addressing the questions than skirt around it with hand-coded data augmentation and
  arbitrary decimal initialization and gradient scaling.

I have started working on my next project, which aims to learn logical operations to beat me at chess! I am excited to
apply my ideas to a more custom architecture, but I know I also have a lot to learn in the space of things like search
algorithms and reinforcement learning as a whole. You can check out the (as of yet, in progress) repo
[here](https://github.com/gabeorosan/logical\_rl\_chess)

Thanks to: George Hotz for the inspiration and examples in his [AI notebooks](https://github.com/geohot/ai-notebook),
Pavithra Solai for the best explanation of [CNN back propogation](https://pavisj.medium.com/convolutions-and-backpropagations-46026a8f5d2c), and last but certainly not least Andrej
Karpathy for the best machine learning [videos](https://www.youtube.com/watch?v=LxfUGhug-iQ),
[articles](http://karpathy.github.io/), and [projects](https://gist.github.com/karpathy/d4dee566867f8291f086) I've come
across - not to mention the [blog
framework](https://github.com/karpathy/karpathy.github.io)! 
