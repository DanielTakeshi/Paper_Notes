# VIME: Variational Information Maximizing Exploration

I'm reading this paper as I want to know more about how information theory gets
applied to reinforcement learning.

**TL;DR: VIME provides a better exploration strategy than the heuristics such as
epsilon-greedy methods by ensuring that the agent selects actions that maximize
information gain about the environment.**

More specifically:

- They use a **Bayesian Neural Network** (BNN) so that they can get posterior
  estimates of \theta, the concatenated set of weights of the neural network
  representing the agent's *belief about the environment*. Not the policy!! So
  in fact, this is a form of **model-based reinforcement learning**, which we
  don't seem to see too often in DeepRL.

- They use information theory and optimizing a variational lower bound to train
  their BNN. (Man, I really need to read *and understand* the VAE paper at some
  point...) It's algebraically *a bit* more tractable than one would expect
  since they assume they can factorize \theta into the product of univariate
  Gaussians.

- **Experiments**: they follow the standard technique of reducing the first
  experiment to the simplest setting, where environments are altered to give +1
  rewards only in very specific scenarios. In those cases, VIME works much
  better than naive exploration.

Ultimately, this is a paper about how to *improve exploration* in reinforcement
learning. It's orthogonal to many contributions and can definitely be merged
with algorithms, as they do here with TRPO (as expected).
