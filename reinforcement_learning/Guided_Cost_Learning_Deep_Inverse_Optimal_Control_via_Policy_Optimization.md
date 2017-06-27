# Guided Cost Learning: Deep Inverse Optimal Control via Policy Optimization

Note: **Inverse Reinforcement Learning == Inverse Optimal Control**.

This falls in the area of inverse reinforcement learning, broadly defined as
learning a cost (negative reward) function given expert trajectories. Two
problems with this:

- First, given expert trajectories, there may be many cost functions which
  satisfy (or maybe "induce" is a better word) those trajectories.
- Second, IOC requires effectively solving a reinforcement learning problem
  anyway, learning the optimal policy as a subroutine, and this is quite
  difficult. (Why?? Because then you get the policy, roll it out, then compare
  with the expert trajectories.)
  
(Indeed, the GAIL paper (from NIPS 2016) said that IRL often requires solving
reinforcement learning as a subroutine.)

Their solution is to cleverly combine policy search with determining the cost
function:

> The main contribution of our work is an algorithm that learns nonlinear cost
> functions from user demonstrations, at the same time as learning a policy to
> perform the task. Since the policy optimization "guides" the cost toward good
> regions of the space, we call this method guided cost learning.

As part of this, they use neural networks to parameterize the cost function,
which had only recently been used in trivial/simple domains. Their work builds
upon maximum entropy IOC; I should understand this paper.

I don't have that much intuition yet on how this works, but think of Figure 1, I
guess. We get a finite, precious sample of (expert) human demonstrations. Then
we continually augment our dataset of demonstrations from the robot, kind of
like how we augment the dataset with DAgger. The inner policy optimization step
means that the robot's (or agent's) trajectories should gradually improve, thus
the dataset quality increases.
