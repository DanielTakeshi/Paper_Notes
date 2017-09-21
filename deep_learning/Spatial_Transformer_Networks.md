# Spatial Transformer Networks

Wow, these are a nice, simple (for DL research) idea that seems to work. The
idea is to try and "standardize" inputs (e.g., images) so that the rest of the
network can view a standardized image and thus the network will be invariant to
"spatial transforms" which include:

- scaling
- cropping (BTW, this means it's invariant to translation)
- rotations
- non-rigid deformations

All that is needed is to insert a "spatial transform" module inside a network.
It is simple, fully differentiable (hence end-to-end backpropagation is used
instead of RL as in hard attention) and seems to not cause additional overhead
in training.

There are three parts to this module:

- **Localization**, to figure out what transformation to apply. It:

  > [...] takes the input feature map, and through a number of hidden layers
  > outputs the parameters of the spatial transformation that should be applied
  > to the feature map –-- this gives a transformation conditional on the input.
  
  The transformation is applied to all (three) channels, which makes sense; we
  don't want different color channels to get different transforms.

- **Grid Generator**, given the parameters of the spatial transformation, this
  generates a "grid" of samples:

  > [...] the predicted transformation parameters are used to create a sampling
  > grid, which is a set of points where the input map should be sampled to
  > produce the transformed output.

  My intuition is that, e.g. for rotations, some pixels don't matter (e.g. black
  pixels in the corners?) so the sampling grid may ignore those points. But I'm
  not totally sure on this.

- **Sampling**, to sample from the grid in a differentiable way, using kernels.

I'm not totally grokking the technical details on how these are applied (for
instance, how does the localization vary the number of parameters since certain
translations need more parameters than simpler ones or the identity?) but for
me, all I need is to use TensorFlow and I am good. :-)

Experiments are done on distorted MNIST, SVHN, and a bird classification task.
The distorted MNIST was predictable but a good experiment baseline since we can
explicitly cause distortions and see what happens when we have baselines (FC and
CNN without spatial transforms) vs ST. Comparisons might be hard though: is it
fair to take a FC net and just plug in a ST in there? That increases the
parameters. Perhaps they included extra FC layers in the baselines to make the
number of parameters comparable?

Regardless, they improve performance slightly and the baselines are already very
good. Hence, why these modules have grown so popular. Applications are mostly in
computer vision, but they have been used in robotics, such as Dex-Net.
