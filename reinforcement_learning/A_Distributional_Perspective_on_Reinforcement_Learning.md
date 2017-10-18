# A Distributional Perspective on Reinforcement Learning

**TL;DR: instead of optimizing the expectation of the return, as we all do in
RL, consider the DISTRIBUTION of the return. Approximate it and use it in RL
when optimizing Bellman's (distributional) equation.** 

The **value distribution** is the distribution of the random return received by
an RL agent.

Instead of using Bellman's (optimality) equation, use the *distributional*
Bellman's equation. Study `Z(x,a)` which is the *random return* with expectation
`Q(x,a)`. Side note: they use `x` instead of `s` for the environment "state".


## Technical Details

(Skipping most of the measure theory stuff, because I have no background in
that area.)

Section 4 proposes their algorithm, "Categorical Algorithm" and the DQN version
is "Categorical DQN". They need to do some projections to get certain supports
to line up and to approximate the Wasserstein distance.


## Experiments

They use the DQN architecture (huh, no double DQN, prioritized?) and output
**atom probabilities** instead of the action-values `Q(x,a)`. They replace the
squared error loss (the normal Bellman one) with a Kl divergence between
`Z_\theta(x,a)` and the projection operator on the sampled `Z` values. But
otherwise it's similar to the NATURE paper.

Understanding Figure 3.

- More atoms lead to more performance, rather than saturating the network. And
  WOW on Seaquest!!


Understanding Figure 4 on Space Invaders. What is it saying?

- Space Invaders has six actions here (or at least, ones we can see).

- Three of them will result in basically death, hence their corresponding
  `Z(x,a)` distributions are very skewed toward zero.

- The other three have a Gaussian-like distribution over the rewards. They seem
  to have a mean of roughly 6-8, so on average those actions will provide us
  that amount of discounted return.

- Unfortunately the figure is confusing. I think they have six different
  histograms, one for each action, and each histogram is a probability
  distribution. And if we took an expectation over these histograms, we'd get
  something which mirrors the relative importance of these six distributions??


## Thoughts and Takeaways

- This is a nice insightful perspective, the best kind of research there is! I
  can certainly see how, for instance:

  >  The distributional Bellman operator preserves multimodality in value
  >  distributions, which we believe leads to more stable learning.

  Yeah, because the expectation could be (for instance) midway between two
  modes, but that expectation could have very low probability density.

- Didn't Bellemare argue against using the Atari benchmarks once? (Because
  neural networks could effectively memorize it?) It seems odd to continue using
  it but I guess we don't have easier alternatives. However, if they claim to
  have state of the art results, that's *really* impressive, especially since
  this seems easier to implement than, say, DeepMind's recent "Rainbow" paper
  LOL.

- See [DeepMind's blog post][1] for more information.

[1]:https://deepmind.com/blog/going-beyond-average-reinforcement-learning/
