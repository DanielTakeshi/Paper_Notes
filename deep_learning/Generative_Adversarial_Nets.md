# Generative Adversarial Nets

This is the original paper from NIPS 2014 which introduced Generative
Adversarial Networks (GANs). GANs have had a huge impact on Deep Learning and by
now there's lots of code and materials available for learning about it. I'll
read this paper as a start to see how it developed, but if it turns out to be
too vague, then skip to other tutorials.

What do the words refer to?

- **Generative**: Because the *generator* player aims to model the data
  distribution, and therefore might be able to "generate" stuff from it (e.g.,
  images, as we will see later).
- **Adversarial**: Because the training process here relies on a minimax two
  player games between a *generator* and a *discriminator*, hence they are
  "adversaries."
- **Networks**: Because G and D are (usually) neural networks and the entire
  "game" is trained via backpropagation.

Note: G and D don't *have* to be networks, but for complicated models like
images, we need networks due to their greater modeling power.

The goal of this paper was to introduce an effective deep *generative* model,
which is different from deep *discriminative* model which compute P(class|data),
as in image classification.

I'm not very familiar with the related work from Section 2.


## The Generator (Counterfeiters)

**Goal**: accurately capture the data distribution *and* maximize the
probability that D makes a mistake.

How to sample from these? *Forward propagation*, which reminds me of how in
Bayesian networks, we can randomly assign variables one by one (respecting
parent-child relationships, probably) to generate samples. It seems like they
pass random noise through the network.

G(z; \theta_g) is a function with input as noise (z) and weights of the network
representing G. Output: data space. I guess the noise range will help define the
range of possible fake data.

Training objective: minimize log(1-D(G(z))). I see, D(...) will be in (0,1)
since it's a probability (of being "correct" for the discriminator), hence 1-D
represents the opposite, but it's still in (0,1) hence it's appropriate to take
a log for numerical reasons and/or mathematical convenience. [Update: I get it
now, it's basically cross entropy and so forth.]


## The Discriminator (Police Officers)

**Goal**: accurately estimate the probability that a sample came from the
training data ("true") rather than G ("false" or "fake").

D(x; \theta_d) with input data (x) and weights of network representing D, and
output a single scalar representing probability that it came from the data
instead of the generator.

Training objective: maximize correct probability for both classes. Actually, the
*log* probability.

As Figure 1 shows, it goes to p_data / (p_data + p_g), all with input x (though
for p_g that might be p_g(x) = p_g(G(z)), not sure).


## Putting it all Together

See Equation 1 for the official minimax formulation. Problem: gradient
saturation at the beginning, since we know the generative model starts bad (the
discriminator has an easy time). This is key: how to train to enable the
generator to actually improve? They offer a new 'max log(D(G(z)))' objective for G
but IDK why it helps with gradients. Intuitively, the generator wants D(G(z)) to
be *high* because that will mean the discriminator is fooled into thinking the
generator (with *noise* z as input) is making real images.

Figure 1: wow, this is really helpful. That was great for intuition. Thanks!

**Theory**: given a model with infinite capacity (this is almost the same as
saying it's non-parametric), the optimum for the two-player game is p_g = p_data
and the discriminator being stuck at 1/2. This is what we want. They prove that
their Algorithm 1 converges to this optimum. **Section 4.1** argues for p_g =
p_data being optimal and **Section 4.2** shows that Algorithm 1 converges. Their
work seems reasonable and I think it make sense, mostly using tools from
information theory (KL divergences) and convex analysis (subderivatives and
convex functions). The latter, however, do not actually apply to the neural
network case but the performance is good in practice.

In Equation 3, they phase out p_z(z) in the integral over z by turning it into
an integral over x. This works since p_z(z) maps noise z into data x in the
support of the actual data, hence if we integrate over x, we are OK, though the
density is now p_g(x).

Algorithm 1 relies on stochastic gradient descent, but since D and G are neural
networks, it's possible to compute these gradients! (How to do that and
understand it well, of course, is another story.)

The authors claim:

> The disadvantages are primarily that there is no explicit representation of
> p_g(x)

but isn't \theta_g, the weights of G, attempting to approximate this? Not sure
how you can expect to get an exact distribution. Am I missing something here?


## Thoughts and Takeaways

In the experiments, it is claimed that:

> In Figures 2 and 3 we show samples drawn from the generator net after
> training.  While we make no claim that these samples are better than samples
> generated by existing methods, we believe that these samples are at least
> competitive with the better generative models in the literature and highlight
> the potential of the adversarial framework.

This makes me wonder about *evaluation metrics* to compare different generative
models.

Overall thought: Wow. Very creative. Very impactful. Theoretical results
are appreciated though they obviously assume very impractical things. Paper is
surprisingly straightforward to read compared to other NIPS papers. My main
complaint is that this paper never describes the architectures of the networks
(!!) in the experiments, but plenty of others have been able to play with GANs
since then to mitigate this.
