# Neural Network Dynamics for Model-Based Deep Reinforcement Learning with Model-Free Fine-Tuning

https://bair.berkeley.edu/blog/2017/11/30/model-based-rl/

It's also in the CS 294-112 (now named CS 285) course, as HW 4 (in the Fall
2018 version).  The idea is to show how you can use model based RL, in
combination with neural networks for the learned dynamics model. Concretely,
the algorithm works something like this:

- Get a bunch of data, and train a neural network to predict the dynamics. This
  means predicting the *change* in the state from time `t` to time `t+1`. Note
  that we're not predicting the full state, which can be a harder problem.

- Given the dynamics, we can do random shooting to generate a candidate set of
  trajectories of length H. (This is bad, but let's assume we do this for now,
  the only advantage is parallelism.) From these candidates, we perform
  optimization with the model to see which one has lowest cost (i.e., highest
  reward).

- With the best plan, execute *only the first action*. The re-plan. This is the
  MPC approach.

- They argue that their model based RL approach means, given a fixed number of
  steps, their agent gets higher reward compared to model-free approaches. But
  the latter have better asymptotic performance, so one idea is to use model
  based approaches as initialization for model free approaches, which is
  clever! They use TRPO for their model free approach.

- Also, their model based approach can learn off-policy, in fact, they show it
  works (to a point) even with data from a random policy.

- Each iteration, they update their model with a form of data aggregation,
  where they combine the old data with new data from the agent. It's like
  DAgger.

- This paper doesn't parameterize a policy like `pi_theta(a|s)` as I am used to
  seeing in model-free approaches. That policy is implicitly done via (a)
  random shooting to generate a set of actions, (b) using the learned dynamics
  to predict the states and rewards, and (c) the MPC approach which takes the
  first action and then re-plans.

Difference between this and the Levine/Finn JMLR 2016 paper is that the
dynamics model for the JMLR paper were locally linear models, and probably
something else related to policy optimization and supervised learning?

I like this paper. It's surprisingly easy to read and I can understand the
algorithm (and should code it up, BTW). The downside is, I suppose, that it
isn't entirely clear how many runs they ran on MuJoCo, or if this extends to
other domains. (But I think their follow-up work addressed that.) Also, it's a
bit annoying to say "medium sized neural network" in the abstract. What does
that even mean? And when they say "less than four hours of random action data
was needed" this can be misleading because that's simulation (right?) whereas a
physical robot would require much longer to get an equivalent amount of "random
action data."
