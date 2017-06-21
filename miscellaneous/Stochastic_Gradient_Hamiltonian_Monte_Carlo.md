# Stochastic Gradient Hamiltonian Monte Carlo

Again we are concerned about the MH framework, where we have proposals and must
decide whether to accept or reject the proposed theta.

This paper is chiefly concerned about transforming this MH test from (normal) to
(stochastic) where the gradient in the former is intractable to compute, but the
gradient in the latter is easier. Why do we need gradients? They "simulate the
Hamiltonian dynamical system." This, by the way, is how they can *generate* the
proposals. They **DO NOT** want to rely on random walks!!

Oh, and what's key to their paper is that the *naive* way to stochastic gradient
(because, hey, why not just use Hamiltonian Monte Carlo with using n << N data
points, and potentially multiplying by N/n if an unbiased estimate is needed?)
can be **arbitrarily bad** so they have to correct for that. Their "correction"
is the heart of this paper. They use "Second-Order Langevin Dynamics" along with
a "friction" term. Yeah, it's probably easier to grasp this with physics
intuition.

(Note: distant proposals are generally the best kind since they give us rapid
exploration of the state space and don't get stuck in modes, but they can be low
probability, which of course this paper is trying to *avoid*.)


## Notation

Make sure I refer back to this if I have questions on HMC. Section II is
presented nicely. I also read a lot of Neal's paper. A few points:

- Always remember the problem setting: sampling \theta values from some
  distribution, and in our case we will pick it to be p(\theta | D) where D is
  the dataset. This is proportional to exp(-U(\theta)) because, as discussed in
  Neal 2010, we have U(\theta) = -\log p(\theta | D) as a convention. So the
  exponential kills the log, negatives cancel, and we get the p(\theta | D) as
  our distribution we're trying to sample from.

- Also remember that in our setting, we'll be using H(q,p) = U(q)+K(p). Just
  remember that.


## Algorithms and Theory

The naive way of doing stochastic gradients for HMC no longer leads to
Hamiltonian dynamics, they prove. To "correct" for this, we can use another MH
test (by the way, this is what the test is supposed to do, filter out bad
samples) but that requires computation over the entire dataset, as I know all
too well. (Actually, they say they could have used their MH test with
Korattikara 2014 and Bardenet 2014 but don't elaborate.)

Their theory involves using the Central Limit Theorem, as expected.

Their final SGHMC algorithm does not even need an MH test!


## Experiments

Three experiments: (1) simulated data, (2) Bayesian neural networks, and (3)
Bayesian matrix factorization. I want to run the third one.

- **Simulated data** shows that HMC and SGHMC maintain the correct target
  distribution whereas naive stochastic HMC does not ... *unless* it has a
  costly MH test added into it! The figures here are hard to read, by the way,
  and in fact are just visual; we don't get precise measurements.

- **Neural networks**: simple, just two layers and fully connected. They use all
  the MNIST training data with all 10 classes (10k validation, 50k train, then
  the rest is the designated test set). They test SGD, SGD w/momentum, SGLD, and
  SGHMC. They set a gamma prior on the weight regularizer \lambda. How do we do
  that in BIDMach? I see, they run 800 passes over full data (in minibatches of
  500 each) and **resample hyperparameters** after each full pass. (The only
  hyperparameter is \lambda, right? So each minibatch, they are updating the
  **weights** of the net, but the regularizer update only happens at the end of
  the pass? I think this is what they mean.) Also, what makes it a Bayesian NN?
  And the accuracy at the end seems to be 99%; isn't that a bit too high for
  comfort?

  It seems like, for each of those 800 iterations (except for initial burn-in)
  they use the current \theta as the full \theta, not a moving average, etc. The
  graph is surprisingly smooth, and I'm still surprised that we're getting this
  good ... I really need to run this myself.

- **(Probabilistic) Matrix Factorization**: this is applied to the collaborative
  filtering problem and is challenging due to data sparsity, among other things.
  This has to predict a user's preference and to provide recommendations. They
  use the **Movielens** data, which is provided as an online stream, and their
  minibatch size is 4000. They say 3952 movies and 6040 users, so I believe the
  overall matrix is of size (3952,6040) and they have to find two smaller
  matrices which together multiply to approximate this starting matrix.

  I wonder, what's the difference with this and John's code which does *Sparse*
  Matrix Factorization?


## My Thoughts and Takeaways

There are strong connections between this paper and the SGLD paper from a few
years ago ([link to my review here][1]). Both are describing how to get a new
theta from a current theta, with similar but notably different methods.

A nice paper linking together the tradeoffs among two prior methods:

- HMC has the advantage of efficient state exploration.
- SGD has the advantage of big-data computational efficiencies (as do other
  "optimization-based methods", as opposed to "sampling-based methods").

Make sense?

[1]:https://github.com/DanielTakeshi/Paper_Notes/blob/master/mcmc/Bayesian_Learning_via_Stochastic_Gradient_Langevin_Dynamics.md
