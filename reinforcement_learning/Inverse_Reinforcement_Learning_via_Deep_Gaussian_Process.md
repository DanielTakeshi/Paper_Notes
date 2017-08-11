# Inverse Reinforcement Learning via Deep Gaussian Process

This paper is about inverse reinforcement learning, which is to determine a
reward function that matches the (unknown) rewards that the expert was
following. Of course, once we have the reward function, we can optimize a policy
based on that to effectively imitate the expert. This is one of I would say
three main paradigms to imitation learning, with the others being behavioral
cloning and generative imitation learning (i.e.  GAIL and MGAIL).

Highlights:

- They use the Deep Gaussian Process to introduce nonlinearity. The reason for
  this is that early forms of IRL, i.e. maximum margin and feature expectation
  matching (I really need to read Pieter Abbeel's classical paper) assume
  that the reward function is linear, and of course that is not going to be
  true. They also argue that DGPs can learn a lot based on small amounts of
  data. Unfortunately I don't have enough intuition here. These are "deep belief
  networks" but I don't know why we need to use those.

- Main mathematical part is to develop a variational approximation to make
  previously intractable computation tractable. This is indeed often used in
  Bayesian analysis due to the intractability of normalizing constants, though I
  sadly did not understand the technical details here.

- They benchmark on simulated experiments. I thought all three were fairly
  basic and it would be interesting to see how this scales to more complicated
  environments. Evaluation is based on the expected value difference metric.
