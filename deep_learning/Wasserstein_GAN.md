# Wasserstein GAN

## Theory and Background

The biggest benefit of this paper is their bold claim:

> [...] we empirically show that WGANs cure the main training problems of GANs.
> In particular, training WGANs does not require maintaining a careful balance
> in training of the discriminator and the generator, and does not require a
> careful design of the network architecture either. The mode dropping
> phenomenon that is typical in GANs is also drastically reduced. One of the
> most compelling practical benefits of WGANs is the ability to continuously
> estimate the EM distance by training the discriminator to optimality.
> Plotting these learning curves is not only useful for debugging and
> hyperparameter searches, but also correlate remarkably well with the observed
> sample quality.

Indeed, I've used GANs frequently and sometimes wonder about how often to
"balance" the Generator and the Discriminator, even though Ian Goodfellow
explicitly tells us **not** to do this (in his NIPS tutorial ...).

They start in Section 2 by defining different distances, including the most
important one for WGANs: the **Earth-Mover distance**. I've never heard of that
before reading this paper. But their example is helpful! It's "hand-selected"
for this, but higher-dimensional distributions may satisfy similar properties
since distributions (or "manifolds") grow sparser, as far as I can tell. They
then argue that the EM distance is "weaker" than the JS distance, which I
interpret as needing fewer assumptions, making it more broadly applicable, and
state **Corollary 1** showing the importance of using EM distance with
feed-forward neural networks.


## How to Implement

The theory uses EM distance, but we have to approximate the infimum part. 

Here are additional tricks when implementing WGANs:

- Use RMSProp.

- Apply weight clamping, e.g. `[-0.01, 0.01]^d`, to make it lie in a compact
  space.

- **Train the Discriminator until optimality each iteration**. :-)

  > The fact that the EM distance is continuous and differentiable a.e. means
  > that we can (and should) train the critic till optimality. The argument is
  > simple, the more we train the critic, the more reliable gradient of the
  > Wasserstein we get, which is actually useful by the fact that Wasserstein is
  > differentiable almost everywhere.

  However, Algorithm 1 trains the critic a fixed 5 times rather than checking
  for convergence.

Looks like the algorithm is surprisingly simple to implement (and it removes the
logarithms).

There's also a meaningful loss metric, which is useful to measure the quality of
generative models.


## Implication and Discussion

A very important paper. Fortunately there's lots of online guides/discussion and
open-source code.

- http://www.alexirpan.com/2017/02/22/wasserstein-gan.html
- https://www.reddit.com/r/MachineLearning/comments/5qxoaz/r_170107875_wasserstein_gan/
- https://github.com/martinarjovsky/WassersteinGAN
