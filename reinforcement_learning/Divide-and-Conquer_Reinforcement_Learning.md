# Divide-and-Conquer Reinforcement Learning

**Main argument/rationale for the importance of this paper**: 

> Our main observation is that, for tasks with a high degree of initial state
> variability, it is often much easier to obtain effective solutions to
> individual parts of the initial state space and then merge these solutions
> into a single policy, than to solve the entire task as a monolithic stochastic
> MDP. To that end, we can autonomously partition the state distribution into a
> set of distinct “slices,” and train a separate policy for each slice.

Basic idea: for initial set of states, run a k-mean clustering procedure to get
different slices of states. These get assigned to their own context. Then we
train policies conditioned on clusters by rolling out trajectories. These are
designed to be locally optimal, and then by using KL divergence constraints, we
iteratively get a centralized policy that can solve the whole thing. 

The algorithm is a modification of TRPO, which is one of the baselines.

From OpenReview, it seems like there's a question of whether it's too close to
the "Distral" algorithm from NISP 2017. I'll need to look at that paper.
