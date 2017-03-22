# DRAW: A Recurrent Neural Network For Image Generation

This paper is about the **Deep Recurrent Attention Writer** neural network.


## High-Level Overview

This paper combines (1) a new spatial attention model and (2) variational
autoencoders to generate images that improve on the state of the art for MNIST
images. In addition, they use *recurrent* neural networks, so the generation
works in a sequence, in contrast to "one-shot" approaches, as they say:

> Most approaches to automatic image generation, however, aim to generate
> entire scenes at once. In the context of generative neural networks, this
> typically means that all the pixels are conditioned on a single latent
> distribution.

The "one-shot" approach is hard to scale and is un-natural when considered with
human intuition. I agree.

DRAW contains *two* recurrent neural networks: an encoder and a decoder (gee,
that sounds very much like GANs!) and the system is trained end-to-end with
standard SGD+Backpropagation with a loss function that's similar to what VAEs
use. Is one of the contributions of DRAW that it doesn't have to use
reinforcement learning, unlike say Minh et al's R.A.M. paper (NIPS 2014), which
they contrast to in Section 4.1? That's what it seemed like from reading their
paper, but they also cite Neural Turing Machines which may have similar
functionality.

Experiments are on three datasets:

- **MNIST**: Results look great! I also like Figure 5, which I think shows that
  to classify digits, the attention model will selectively focus on the critical
  parts of the images (i.e. not the corners, which tend to be pure black). Wow,
  and they can generate *two* images at a time!

- **SHVN**: Results look great!

- **CIFAR-10**: Um ... results are OK but blurry. =(


## Technical Details

Unfortunately, I got a little lost reading about the neural network
architecture.

How do their read and writes work?

- **Read**: TODO

- **Write**: TODO

At least their experiments are easy to understand. I'd like to go back and read
through the math a bit, and related code if I have time.


## Thoughts and Takeaways

I wonder how this compares with Generative Adversarial Networks. It's been two
years, after all.  Why didn't this paper cite the GAN paper? It came out
beforehand. They instead cite one of Goodfellow's other, lesser-known papers.

I *really* like the figures. It's easy to motivate for this kind of work, but
still, that doesn't diminish their accomplishments.
