# InfoGAN: Interpretable Representation Learning by Information Maximizing Generative Adversarial Nets

In Goodfellow's tutorial, this is considered as one of the research frontiers of
GANs based on **how to effectively utilize the input code**. Recall that in
GANs, the input code **z** can be thought of as being drawn from a simple prior
(e.g. the uniform distribution) which then is passed through the network to get
**x = G(z)**, a draw from the Generator's distribution **p_model**. It's easiest
to think of **z** being drawn before hand and passed as the first layer input to
the network, but there are lots of other options that work well depending on the
scenario.

Goodfellow once proposed to be able to sample from **p(z|x)**, so given an
image **x**, we can obtain the code **z** that produced it, because the
representation (i.e. code) should probably give us some interesting insights.
But this has not worked yet. This paper proposes an alternative line of thinking
based on *training the code to be more useful*:

> InfoGANs (Chen et al., 2016a) regularize some entries in the code vector with
> an extra objective function that encourages them to have high mutual
> information with **x**. Individual entries in the resulting code then
> correspond to specific semantic attributes of **x**, such as the direction of
> lighting on an image of a face.

It's also briefly discussed at the [OpenAI blog][1].

What are the details? Time to find out ...

Notes:

- The paper is about **unsupervised learning** (i.e. lots of data but no labels)
  and to face this problem, they turn to **representation learning**, which can
  be thought of as trying to obtain "interpretable features" that are
  "disentangled," or separated I guess? (The paper provides several examples so
  just think of those.) Previous work used **supervised** learning for this, so
  if their method gets the same quality (and they say they do!) then it's
  probably better!

- InfoGAN can disentangle both discrete and continuous latent factors, scale to
  complicated datasets, and typically requires no more training time than
  regular GANs.

- Intuition: there's very little restriction on the design of the Generator. So,
  for instance, it can use it's code **z** in a naive way. But it's ideal if,
  for MNIST let's say, the Generator could somehow allocate a discrete random
  variable to choose the digit, and then continuous random variables indicating
  the angle and thickness of the digit.

- Following the intuition above, split the noise vector into **incompressible
  noise** and **latent codes**. The latter is really the heart of InfoGANs. Add
  mutual information to avoid the problem of irrelevant codes, i.e. **P_G(x|c) =
  P_G(x)**.

- Reformulate the objective from

  > min_G max_D V(D,G)

  to

  > min_G max_D { V(D,G) - lambda * I(c; G(z,c))}

  where I indicates mutual information.

- They don't use the above because it's hard to compute, but they can get a
  lower bound based on variational stuff, see **Section 5** for details (I
  haven't gone through it carefully). The auxiliary distribution can represent
  discrete latent variables like the MNIST digits with a softmax layer. To be
  clear, the auxiliary distribution is represented as a neural network.

- They designed their networks and experiments based on the DCGAN paper, to
  avoid the need to test lots of distracting parameters.

- Datasets used: **MNIST** (digits), **CelebA** (faces), and **SVHN** (house
  numbers). I don't have experience testing out the last two so perhaps I should
  do so at some point.

- Very interesting figures! Remember to read from left to right for one latent
  code's variation.

[1]:https://blog.openai.com/generative-models/
