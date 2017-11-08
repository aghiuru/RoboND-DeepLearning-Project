[arch]: ./images/arch.jpg

#### Project: Follow me
---

##### The write-up conveys the an understanding of the network architecture.

The student clearly explains each layer of the network architecture and the role that it plays in the overall network. The student can demonstrate the benefits and/or drawbacks different network architectures pertaining to this project and can justify the current network with factual data. Any choice of configurable parameters should also be explained in the network architecture.

The student shall also provide a graph, table, diagram, illustration or figure for the overall network to serve as a reference for the reviewer.

---

Since what the network should output is an image with each pixel representing the likelihood of it being part of the target, I used a fully convolutional network, with encoders and decoders and a normalizing layer in-between.

For the decoders to be able to better reconstruct the output image from the 1x1 convolutional layer, I added skip connections to the decoding layers. The network is very similar to the one we had to build for the last lab before the assignment.

The number of encoders and decoders is 3. The kernel size for the convolution layers is 3. The size of the 1x1 layer I went with was 256. It was enough to get a score above 0.40, but most likely the network can be improved by deepening it. However, given the fact that the input image is quite small (160x160), I don't think a whole lot more can be done since it becomes hard even for a human to tell people apart in an image that size.

![architecture][arch]
---

##### The write-up conveys the student's understanding of the parameters chosen for the the neural network.

The student explains their neural network parameters including the values selected and how these values were obtained (i.e. how was hyper tuning performed? Brute force, etc.)
All configurable parameters should be explicitly stated and justified.

---

First, the values for the parameters:

```
learning_rate = 0.0016
batch_size = 35
num_epochs = 25
steps_per_epoch = 152
validation_steps = 30
workers = 4
```

To get to these values, I started with a set of values (0.001 for learning rate, 50 for batch size, 20 epochs) and then changed them until I got a passing score. Unfortunately, I realized at some point that on most of my tweaking runs, I was continuing from the same model (by re-running the training cell) instead of re-running the whole notebook which reinitializes the model weights. :(

When doing the search, I was looking for a less steep decline in loss in the first three epochs and for a decent training speed since I did a lot of runs -- almost used all my $100 credit, most likely wasted more than half of it unfortunately.

I recorded extra images for the training, so in the end I had 5244 samples for training, hence the value for steps_per_epoch, to be able to cover all samples every epoch.

---

##### The student has a clear understanding and is able to identify the use of various techniques and concepts in network layers indicated by the write-up.

The student is demonstrates a clear understanding of 1 by 1 convolutions and where/when/how it should be used.
The student demonstrates a clear understanding of a fully connected layer and where/when/how it should be used.

---

1x1 convolutions are used for dimensionality reduction, since they summarize pixel data from multiple features to a smaller number of new features. They are used in the encoding part of the FCN.

In fully connected layers, all outputs are connected to all the activations in the previous layer, so they can be used in a FCN to reconstruct more data from encoded data.

---

##### The student has a clear understanding of image manipulation in the context of the project indicated by the write-up.

The student is able to identify the use of various reasons for encoding / decoding images, when it should be used, why it is useful, and any problems that may arise.

---

While I don't think I understand this question very well, by looking at the images returned by the network, it seems to be identifying parts of the human body that are colored red (like the model it's supposed to be detecting), and then trying to make up the whole body out of them.

Once in a while, you get a different person wearing a piece of clothing that looks red, and that part of the person is identified as being part of the target, obviously the other parts are not. This wrong identification makes me think that the network could be improved by either adding another encoding layer or something like max pooling so even if one part of the body is red, it's ignored if the most of the other parts are not.

---

##### The student displays a solid understanding of the limitations to the neural network with the given data chosen for various follow-me scenarios which are conveyed in the write-up.

The student is able to clearly articulate whether this model and data would work well for following another object (dog, cat, car, etc.) instead of a human and if not, what changes would be required.

---

This model is obviously trained for detecting the person wearing red, but with different training data, the same architecture most likely can be trained to detect other types of objects.
