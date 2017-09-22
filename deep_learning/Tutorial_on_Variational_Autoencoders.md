# Tutorial on Variational Autoencoders

Was this the inspiration for Ian Goodfellow's GANs tutorial? :-) I have no idea,
actually. I'm really thankful for him and Carl Doersch for making these easily
accessible tutorials. This should help me as I attempt to code VAEs.

**Good motivation**: sure, we could use `P(X)` to compute the likelihood of `X`
(e.g. how likely is an image a true image and not random noise) but the holy
grail is really to **synthesize** these images, not to assign existing images to
probability values.

**Good intuition**: for making 0-9 digits, we don't want to sequentially
generate images, but to sample a **latent variable** first so that the model
"knows" in advance which number it's building. (Of course, I think PixelCNN
sequentially generates images ... but the intuition still works here.)

**Equation 1** makes sense, it's what we want to maximize. And don't forget that

```
P(X|z;theta) = N(X|f(z;theta),sigma^2*I)
```

VAEs assume that the latent variables `z` are simple, typically a d-dimensional
standard normal, because we can define an arbitrarily complicated function (i.e.
neural networks) to transform it to more complicated (and interesting!) output.
That's what `f` is for above.

**Figure 4** + related discussions are further helping me to understand the
re-parameterization trick. What really helps is when Carl describes it as:

> Stochastic gradient descent via backpropagation can handle stochastic inputs,
> but not stochastic units within the network! The solution, called the
> "reparameterization trick" in [1], is to move the sampling to an input layer.

Indeed, if you move the Gaussian sampling outside, then it is still "input" to
the network! (To be clear: it isn't input to the encoder.) I think that finally
is the thing I needed to grok this concept. Do not put sampling inside a network
unless it is used as some input layer.

Hmm ... there seems to be a different emphasis on "the lower bound of the log
prob" which was featured in the CS 231n course at Stanford. He mentions this
briefly at the end of Section 2.3 and discusses the approximation error in
Section 2.4.

Section 3 introduces the **Conditional VAE**, which I assume is analogous to the
Conditional GAN. He has a good point where standard regression models fail in
cases when the output is multimodal, since they will result in meaningless blurs
which are the "average" over all possible outputs. That's actually a criticism
of VAEs as compared to GANs, but perhaps conditioning should help.

I got a bit confused by his setup in Sections 4 and 5, unfortunately. But things
should be clearer once I get to code this up. For some reason he is assuming
that digits in MNIST are either 0 or 1 ... so he's binarizing it? Instead of
having it be {0/255, 1/255, 2/255, ..., 255/255}?
