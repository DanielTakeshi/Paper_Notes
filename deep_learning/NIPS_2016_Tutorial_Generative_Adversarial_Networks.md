# NIPS 2016 Tutorial: Generative Adversarial Networks

This is an expository-style paper which I hope will be useful to me.

I'm not sure why Figure 1 from the original GANs paper ([see notes here][1])
isn't here, because that figure was really helpful for me.

Why study GANs? Well, you didn't have to motivate me. =) But Goodfellow explains
it pretty well. I particularly like the rationale based on multi-modal outputs.

The maximum likelihood section is excellent. It provides the definitions, then
also provides a taxonomy of models which are generative and which use (or can be
*made* to use) the principle of maximum likelihood. Here's the key insight from
Figure 9:

> While the models used for GANs can sometimes be constructed to define an
> explicit density, the training algorithm for GANs makes use only of the
> model's ability to generate samples. GANs are thus trained using the strategy
> from the rightmost leaf of the tree: using an implicit model that samples
> directly from the distribution represented by the model.

I really want to learn how all of these work, particularly variational
autoencoders (VAEs). There are also some methods which I have not heard of
before, such as fully visible belief networks (FVBNs).

VAEs (and variational methods, more generally, and note here that this is
slightly different from how I view the word "variational") define a lower bound
on the log likelihood. Therefore, if VAEs maximize that lower bound, the
actual log likelihood will be higher than that, hence better.

Subjective opinion: GANs produce better-looking images than VAEs. But Goodfellow
emphasizes that this is empirical because there is no good way of comparing
image quality. Can someone come up with a way?


[1]:https://github.com/DanielTakeshi/Paper_Notes/blob/master/deep_learning/Generative_Adversarial_Nets.md
