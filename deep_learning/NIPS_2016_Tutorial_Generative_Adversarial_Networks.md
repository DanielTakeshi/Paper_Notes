# NIPS 2016 Tutorial: Generative Adversarial Networks

This is an expository-style paper which I hope will be useful to me.

## Background and Motivation

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
before, such as fully visible belief networks (FVBNs). Some of these are
probably in their Deep Learning book.

VAEs (and variational methods, more generally, and note here that this is
slightly different from how I view the word "variational") define a lower bound
on the log likelihood. Therefore, if VAEs maximize that lower bound, the
actual log likelihood will be higher than that, hence better.

Subjective opinion: GANs produce better-looking images than VAEs. But Goodfellow
emphasizes that this is empirical because there is no good way of comparing
image quality. Can someone come up with a way? 

Note: I remember, [the OpenAI gym blog post][2] said almost the same thing. Wow,
that was almost a year ago ...

The treatment of MCMC methods is cursory so I better take a look at the Deep
Learning book. I wonder if there is a research opportunity to help MCMC scale
better to higher dimensions? Both in regards to the number of samples and in
regard to the parameter size of the model.

GANs helped to overcome many of the disadvantages of other methods, but they
have their own downside:

> At the same time, GANs have taken on a new disadvantage: training them
> requires finding the Nash equilibrium of a game, which is a more difficult
> problem than optimizing an objective function.

Interesting ...

## How do GANs Work?

Here's my gripe with Figure 13. It's a simple graphical model showing a GAN. But
the graphical model provides absolutely no help to understanding GANs
whatsoever! I understand graphical models; I want to know why *GANs* work. How
is it that the gradients will flow and update to the correct spots, for
instance? (PS: I like other figures, such as Figure 12.)

But anyway, yes it's a two-player game, generators and discriminators, etc.
Generators and discriminators need to be implemented with differentiable
functions, which is where the deep learning part comes in. So when coding a GAN,
I guess, we will need to form two neural networks.

Key note:

> If both models have sufficient capacity, then the Nash equilibrium of this game
> corresponds to the G(z) being drawn from the same distribution as the training
> data, and D(x) = 1/2 for all x.

Now *that's* interesting. I've wondered why discriminators might not just
dominate and get everything right?

The written description of the math and parameters is really clear (page
18-ish).  Thanks! Thinking of it in terms of a game rather than an optimization
problem may help.

Another connection with VAEs:

> The relationship with variational autoencoders is more complicated; the GAN
> framework can train some models that the VAE framework cannot and vice versa,
> but the two frameworks also have a large intersection.

Yeah ... I'm not sure why. =(

Regarding training:

> Many authors recommend running more steps of one player than the other, but as
> of late 2016, the author's [Ian Goodfellow] opinion is that the protocol that
> works the best in practice is simultaneous gradient descent, with one step for
> each player.

Um ... then I'll side with him, I guess. =)

Discriminators use the standard cross entropy loss function. Ah, *that's* why
the formulation made sense to me ... I didn't make the connection with cross
entropy at first. Think: the cost *could* be, say, mean square error, but cross
entropy is ... better. A few refreshers on Wikipedia and it's clear. =) I will put
this as one of my extended insights in a forthcoming blog post.

Section 3.2.4: not sure I understand this yet. I will re-read it.

Section 3.2.5: heh, now *this* is helpful. I never tried understanding why the
KL-divergence was not symmetric. Figure 14 is really helpful! Sadly, it no
longer seems to be the major hypothesis as to why GANs are able to select modes
(rather than taking maximum likelihoods and averaging over modes, producing
blurry images). Key here is that we're making an assumption on the model family
for p_model.

I may want to read the DCGAN paper. It was featured in the [OpenAI blog post][2].


## Tips and Tricks

I hope I can use some of these.

- **Using labels**: I'm assuming the previous discussion on GANs did not say
  anything about labels because we assume they don't exist? I assume these are
  labels *other* than the "real" vs "generator-generated" images?

- **Virtual Batch Normalization**: batch normalization is *part of the model*.
  I.e. think of it as a layer (because it is!). It has a slight downside so use
  the *virtual* variant where we have a reference batch. Ian argues that with
  normal batch normalization, the small minibatches will cause too much
  fluctuation with the mean and standard deviation statistics each iteration.
  This "fluctuation" will impact the generator's image more than the input code
  z.

- I was originally wondering about the "balance" between G and D so it is good
  that Ian brings this up here.

- One-sided label smoothing. Yes, I've tried it and I like it.

There are lots of other tricks online.


## Research Frontiers

Well, I *hope* I can do something that has impact.  The key frontiers have to do
with convergence (or lack of it!), along with how to *evaluate* the output
(nowadays, we're just looking at images and subjectively rating them), discrete
outputs, semi-supervised learning, and connections with reinforcement learning.

I can kind of see why mode collapse --- and convergence more generally --- is a
problem. How do we tell if we're converging in a two-player game? In a
"one-player game" or "normal optimization" we use stochastic gradient descent to
make incremental progress towards a local minimum which is often good enough.
But this may be harder with two players because one player's progress downward
(in loss function space) may make the other player's progress upward.

The TL;DR problem of mode collapse is that the generator should be able to, in
principle, generate images that fall in one of N modes, but in practice, it only
generates images falling in one of n << N modes.

One possible solution: **minibatch features**, allowing discriminators to look
at a sample image and then compare the features in latent space with features
that are known to be from real vs generated samples. That seems logical.

(Wait, now Ian Goodfellow says minibatch features have "reduced the mode
collapse problem enough that other problems [...] become the most obvious
defects." So ... I guess maybe mode collapse is not the biggest and/or pressing
issue?)

Another possible mode-collapse solution: **unrolled GANs**. More paper reading
for me!  =)

This was something I wondered about earlier: given x, can we find z, the code
that generated it?


## Exercises

Don't cheat!

### 7.1: Discriminator's Strategy

OK, *this* is what I was looking for, that helpful figure! I see, this was from
Section 4 of the GANs paper, and I was confused, we have to actually and where 


### 7.2:

The first part is clear. The second was tricker, I wasn't aware that sinusoids
formed the basis of these types of solutions.


### 7.3:

I'm not sure how they're deriving Equation 33?


[1]:https://github.com/DanielTakeshi/Paper_Notes/blob/master/deep_learning/Generative_Adversarial_Nets.md
[2]:https://openai.com/blog/generative-models/
