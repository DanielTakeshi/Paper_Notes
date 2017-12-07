# Adversarial Discriminative Domain Adaptation

**What's the main idea and/or problem to solve?**

- Domain shift/adaptation. If we train a network to classify digits of MNIST,
  for example, how do we also get it to work on USPS digits? That's the domain
  shift problem, and it's intuitively very hard but also quite important to know
  how to train networks to generalize. Or how we make the "latent feature space"
  of the network to be *domain invariant*.

- More formal problem statement: we are given *source* images and labels
  `(X_s,Y_s)` drawn from the source domain distribution `p_s(x,y)`, as well as
  *target* images `X_t` from the target distribution `p_t(x,y)` but where we
  have *NO LABELS*. The goal? Learn a target representation `M_t` and classifier
  `C_t` that can correctly classify the target images into one of `K` categories
  at test time.


**Why does their method work?**

- It helps to think of a neural network that does classification as two parts: a
  feature embedding followed by the actual classifier (see my comments & bullet
  points above). So perhaps the "feature embedding" consists of the
  convolutional layers of the net, and then the "classifier" is the fully
  connected part.

- So, what to do? Learn a feature embedding for *both* the source and target
  images that result in similar embeddings. Then the trained *classifier*
  portion for the source can simply be applied on the target. That makes sense.

- Minimize source and target representation distances between alternating
  minimizations of the two steps:

  - A domain discriminator classifies whether a data point is drawn from the
    source or target domain. Not the labels --- the *data points*.  And BTW,
    *these data points are fed through the feature embedding* and that's
    ultimately what we use for the cross entropy loss. Yes, that makes sense.

  - Source and target mappings (i.e. the `M_s` and `M_t` terms) are optimized
    according to a constrained adversarial objective, which varies among
    different instantiations of their framework. In general, `M_s` and `M_t`
    have a similar neural network architecture. They may or may not share
    weights, which is one way to enforce equality (but then it might not make
    `M_t` "discriminative enough" among the target distribution's categories).

- I think I understand why this works. The adversarial objective is trying to
  "overpower" the discriminator so that it creates similar feature embeddings
  for both the source and target. The discriminator gives a good gradient.

- They cast their algorithm definition as picking among three categories:
  generative vs discriminative base model, tie/untie weights, and which
  adversarial objective. The utility of this is subject to some questioning,
  though.


**Could I implement this?**

- I think I can implement the discriminator objective. That's typically the
  easier part with GAN-like formulations anyway.

- The source `M_s` is learned the normal way with cross entropy loss. And during
  adversarial training, it is fixed. I think this makes sense, since in normal
  GANs we have a fixed true distribution.

- Now what about `M_t`?? It is pre-trained with weights initialized from `M_s`.
  This makes sense; it's like how in GAIL we should initialize the generator
  from behavior cloning. It's trained with the adversarial loss, and both `M_t`
  and the discriminator are trained in this stage. 

Yes, I think I could implement this given enough time. Figure 3 is very helpful.
