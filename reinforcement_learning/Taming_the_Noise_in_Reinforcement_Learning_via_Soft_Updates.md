# Taming the Noise in Reinforcement Learning via Soft Updates

This paper introduces a new algorithm, *G-Learning*, which is similar to Q-Learning except it tries to avoid the issue of bias when taking the max over actions. This was a problem observed in the Double Deep-Q-Network paper. It does so by using regularization and soft updates. In particular, it adds a penalizing term to the cost-to-go for divergence (uses information theory) from a prior policy.


## Introductory Stuff

The formulation is the usual MDP with costs, not rewards, so I should be able to grasp it. The most annoying part is the use of \theta as a *function*, which provides the costs. Another trick with Equation 4 is to generalize the standard Q-Learning update by using their \pi function. Normally it's 1 for the best action but it doesn't *have* to be that way. Remember: there's two things here, the *exploration* policy will drive our agent through the world and generate samples, which are then used in the Q-Learning *update rule* (usually max_a, here min_a probably).

Simple example: E[min_a Q(s,a)] <= min_a E[Q(s,a)] = min_a Q\*(s,a), using Jensen's inequality, where Q\* is the optimal action-value.

I don't understand this justification (Section 2.3):

> We conclude that the real value $V^\pi$ of the greedy policy (5) is suboptimal only in the intermediate regime, when the gap is of the order of the noise, and neither is small.

Their algorithm is designed to reach the same end goal as standard Q-Learning, but perhaps does so faster by improving bias during the learning process. The "bias" here refers to the estimation of the Q(s,a) values (sorry, hope that's clear?).


## The Algorithm: G-Learning

Lots of notation:

- \rho is our prior policy
- \pi is our learned policy
- g^\pi is the information cost of \pi
- I^\pi is the total expected, discounted information cost
- F^\pi = V^\pi + (1/beta)I^\pi is the free energy
- G^\pi is the state-action free energy function, analogous to the Q-value! I see.

I think the equations in Section 3 make sense. I have not been able to rigorously verify all of them because my goal now is to get the algorithm running, not understand all the math. The convergence proof relies on a contraction norm proof and then it references other work.

Section 4 is devoted entirely to the \beta schedule, where low \beta indicates we stick with the prior and higher \beta means allowing ourselves to diverge from the prior. We want to go from low to high. Note that:

> When \beta = \infty, the equations for G\* and F\* degenerate into the equations for Q\* and V\*, and G-learning becomes Q-learning.

The other extreme, \beta = 0, is the same as Q^\rho-Learning, which I think is the same as SARSA. In the experiment, they use a linear scheduling.

Here's a "devil's advocate" thought: why can't we run a simple hybrid of SARSA and Q-Learning? If we have a prior already here, let's just set some 0 < \epsilon < 1, where each iteration, with probability \epsilon, we run SARSA w/the prior, else we run Q-Learning. Do this while annealing \epsilon to 0. Isn't that it? Why do we need information theory? Section 4.1 argues for the existence of a \beta which results in unbiased estimates (non-constructive proof). Would that help? And this still doesn't assuage my concern over needing a good prior \rho. 

They also claim:

> [...] but to our knowledge G-learning is the first TD-learning algorithm to explicitly use soft-greedy policies in its updates.

Section 3.1 discusses the "soft-greedy" part, but what makes it "soft"? Soft usually indicates that we have probabilitistic updates instead of deterministic updates. TODO

TODO Understand Section 5's related work description of the "closely-related" algorithm?


## Experiments

The domains are "clean and simple" they say, but this is not entirely a good thing. Maybe I can try something else. At least the schedule for \alpha is clear, and maybe \beta as well. Unsurprisingly, they use a uniform prior policy. They use GridWorld with some stochastic results, and to add more variation, they test with three different cost functions. OK, not too bad, I can easily code up their setting. I should create a GridWorld constructor and simply let the user explicitly define the grid and actions. That increases generality.

Their second (and last) experimental setting is a Cliff-Walking domain. It is similar to GridWorld. I want to test with variations.

They compare:

- G-learning
- Q-learning
- Double-Q-learning
- \Psi-learning
- T_C operator
- Q^\rho (SARSA?) learning

I'm impressed by all their comparisons.

Their results are packed into the GIANT Figure 2 (tough to see for colorblinds like me ... but I will do my best). Three evaluation metrics: first column is for empirical bias, second column is for mean absolute error, and the third is for value of the learned policy. We want all three of these measures to go to zero (remember, "value of learned policy" = "cost of learned policy" here). The first row is the least important one because it uses a fixed cost function, and they're more interested in seeing results for the noisier case.

It turns out that G-learning is indeed better in the noisier cases. The results are claimed to be statistically significant. However, I would be interested in seeing how they tuned the competing algorithms.

Huh, in the cliff-walking example they explicitly mention SARSA ... so what's the difference between SARSA and Q^\rho-learning??

Their cliff-walking domain is supposed to illustrate another strength of G-learning but I'm unsure what they mean. My guess is they argue G-learning, during *exploration*, will prefer to be away from the cliff most of the time, to make exploration less costly. But in some cases we don't care about that, e.g. Atari games.


## My Thoughts and Takeaways

This might be more straightforward for me to immediately get to work on, since it's a variation of the very-familiar Q-Learning algorithm. In addition, this description of G-Learning in the paper makes it sound not too different from Q-Learning:

> With only a small sample to go by, G-learning prefers a more randomized policy, and as samples accumulate, it gradually shifts to a more deterministic and exploiting policy.

I'm concerned about the need for a prior policy, and also if adding a penalized term is enough to make this algorithm better than Q-learning. To add more to the confusion, G-Learning is supposed to take a "Frequentist" view, and not be like "Bayesian Q-learning."

I'm also wondering about the hybrid of SARSA and Q-learning that I thought about earlier.
