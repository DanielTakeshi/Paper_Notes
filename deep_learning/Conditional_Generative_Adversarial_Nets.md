# Conditional Generative Adversarial Nets

Surprisingly, it doesn't seem to be officially published at a conference,
despite its large number of citations. **Update**: after reading the paper, I
can see why, it's not really "developed enough" for a top-tier conference. For
instance, see their frank quote:

> The conditional adversarial net results that we present are comparable with
> some other network based, but are outperformed by several other approaches -–
> including non-conditional adversarial nets. We present these results more as a
> proof-of-concept than as demonstration of efficacy, and believe that with
> further exploration of hyper-parameter space and architecture that the
> conditional model should match or exceed the non-conditional results.

**Update again**: yeah, and the original GANs paper actually said we could do
conditional stuff. So just think of this paper as a proof of concept. There's
another one that came later from Denton et al 2015 which contains a more
thorough investigation of conditional GANs.

This paper introduces CGANs, which are a popular kind of GAN framework, perhaps
as common as DCGANs?  Or maybe the basic concept of conditioning is used in
other architectures, so there's a lot of hybrid models. I mean, you can do a lot
with conditioning.

The GAN architecture: the generator and discriminator are now `G(x|y)` and
`D(x|y)` with the extra `y` information, which can be thought of as class labels
(e.g. for MNIST) in one-hot vector form. The `y` can be added into the network
by feeding two separate inputs `x` and `y` into the networks and combining them
at some layer (e.g. by concatenation).

I think what happens is that when we sample that prior noise `z` for the
Generator, we also sample a `y`, so we are effectively sampling for a class?

I've got to implement this!!

They use maxout networks for their experiments. At some point, I would like to
read that paper! I wonder why these do not seem to be as common as I thought.
The experiments involve (1) MNIST of course, and (2) tagging of Flickr photos.
