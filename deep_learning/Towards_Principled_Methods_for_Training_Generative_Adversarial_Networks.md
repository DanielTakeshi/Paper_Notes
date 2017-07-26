# Towards Principled Methods for Training Generative Adversarial Networks

Lots of papers on GANs! ICLR might be called "The GAN conference" at this rate.
This one was an oral presentation so it should be a good paper. I'd like to see
this to be able to better train GANs in a, well, more principled manner. This
might be considered a more theoretical version of the papers "Improved Techniques
for Training GANs" and "Unsupervised Representation Learning With Deep
Convolutional Generative Adversarial Networks".

It starts by discussing the KL divergence and its asymmetry. Indeed, I remember
from Goodfellow's tutorials/papers some discussion on KL divergence and how the
non-symmetric property can help out by targeting one mode instead of averaging
as in maximum-likelihood style manners. However, this paper argues that the
subject matter is "far from closed." And note that there's some issue with the
"discriminator is optimal" language we use; it is in fact worse to train a GAN
if we try to make the discriminator optimal at each time step. I am guilty of
doing this sometimes, but only during the "pre-training" phase, which I imagine
is a less serious crime.

Interesting insight: suppose we have a trained DCGAN. Then our Generator is very
good. Now, let's fix the Generator and train the Discriminator. Its error will
go down to zero quickly, meaning that it's still able to discriminate! It's
probably due to similar reasons from the co-ICLR 2017 paper "Understanding Deep
Learning Requires Rethinking Generalization."

With respect to the cost function for the Generator that people actually use in
practice, they say in **Section 2.2.2**:

> We now state and prove for the first time which cost function is being
> optimized by this gradient step. Later, we prove that while this gradient
> doesn't necessarily suffer from vanishing gradients, it does cause massively
> unstable updates (that have been widely experienced in practice) under the
> presence of a noisy approximation to the optimal discriminator.
