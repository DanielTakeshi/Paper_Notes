# Automatic Goal Generation for Reinforcement Learning Agents

How does their algorithm work?

> Our approach can be broken down into three parts: First, we label a set of
> goals based on whether they are at the appropriate level of difficulty for the
> current policy. Second, using these labeled goals, we construct and train a
> generator to output new goals that are at the appropriate level of difficulty.
> Finally, we use these new goals to efficiently train the policy, improving its
> coverage objective. We iterate through each of these steps until the policy
> converges.

Side comment: much of the content here is also present in their related paper,
"Reverse Curriculum Generation for Reinforcement Learning." That paper used a
simulated robot inserting an object on a peg, and didn't use a GAN (while this
paper uses a GAN).

The main thing to be aware of is how to use a GAN to generate goals that are of
an appropriate level of difficulty for the current agent. We can test the
success rate with Monte Carlo estimates based on rollouts. Yes, this makes
sense.

Experiments are on simulated mazes, which are a good test-bed for this.
