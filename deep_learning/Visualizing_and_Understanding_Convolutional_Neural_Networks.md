# Visualizing and Understanding Convolutional Neural Networks

This is the **ZFNet paper**, the one that won image classification awards in
2013, the year after **AlexNet** did it.

Nomenclature: we view networks as going from bottom to top, where the bottom
consists of the convolutional kernels on the original input images and the top
consists of the softmax. Just be aware of that!

Network design and performance highlights:

- Figure 3 shows details. Differences with AlexNet:

  - Use dense connections in place of the sparse connections across two GPUs. I
    think they used one GPU and nowadays we'd all be using one GPU anyway, no
    more (well, manual at least?) communication across GPUs.

  - Reduced first convolutional kernel size from 11x11 to 7x7 and then did
    stride 2 instead of 4 (AlexNet started with 11x11, stride 4 kernels). This
    was done due to "various problems" observed after looking at the first few
    filters, e.g. "aliasing artifacts" from a large stride step. I like the
    better visualizations from **Fig 6(c)**, ZFNet, in contrast to **Fig 6(b)**,
    from AlexNet.

  - Minor differences: increased the number of filters in the third, fourth, and
    fifth convolutional layers. (Or maybe decrease? Actually I looked at the
    Stanford CS 231n notes from Spring 2017 and it looks *wrong* ... heh,
    amusingly enough there's even a note saying "TODO: remake figure" on that
    slide.)

- They test on other datasets, but use a similar architecture. How does this
  work? Use AlexNet but simply replace and retrain the final softmax layer. With
  AlexNet, the last layer mapped (I think) a 4096-dimensional vector to a
  1000-dimensional vector. All we need to do here is to replace this mapping to
  go from 4096 to X, where X is the number of classes in our new dataset. For
  other layers, we borrow the weights from earlier to speed up training.

- Funny thing: they initialize all non-bias weights to 1e-2. Those were the good
  old dark ages of weight initialization. =)

- **12 days** (!!) of training.

Other insights from the paper, because one could argue its larger contribution
is how to *understand* CNNs:

- A visualization technique, which they say "reveals the input stimuli that
  excite individual feature maps at any layer in the model."  This uses a
  deconvolutional neural network, which proceeds "in reverse. This is attached
  after **every layer** so there are many versions of these.

- Unpooling operation makes sense, what they do is they record which of the 4
  components (since it's 2x2 pooling) were the largest in the forward pass, then
  the signal can be reconstructed by pointing to the maximum. But how do they
  deal with reconstructing the other 3 signals in the 2x2 grid?

- **Figure 2** has lots of visualizations. I'm pretty sure we did the first
  layer filters in CS 280.

- I really like **Figure 6**. It's intuitive enough for me to understand given
  my relative lack of intuition over the entire deconvolution pipeline.

- They perform occlusion sensitivity analysis, a clever experiment where they
  "insert" gray boxes in different locations of the image to cover up the actual
  object. They conclude that their networks are not just learning broad scene
  context but are sensitive to the actual object itself.

BTW: I'm really blown away by this paper. The large amount of insightful and
helpful experiments is very helpful!

**I still need to get more intuition about this deconvolution pipeline**. But I
think I understand how this works for the first layer. The first layer features
should be 7x7, right? Because for a given component in the second layer, if we
set everything else to zero and then look at the receptive field of our chosen
component, it should be 7x7 (spatially), right? Basically, we fix this and map
back to see what receptive field it covers, and it gets larger as you go deeper
into the network. The AlexNet paper said their filters were 11x11(x3 for the
color), which makes sense.

Now, in AlexNet, there were 96 filters, each of 11x11x3, so perhaps the
visualizations there were just the RGB images of those weights. But then that is
NOT dependent on the input image, but the deconvolution **IS** dependent on the
input image, right?

Once again, I'm grateful to Adit Deshpande for [writing an excellent summary
that includes this paper][1]. Read the paper first and *then* read his blog post
to make sure you get the high-level ideas.


[1]:Visualizing_and_Understanding_Convolutional_Neural_Networks.md
