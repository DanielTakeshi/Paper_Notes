# Learning the Variance of the Reward-To-Go

**What problem are they trying to solve?** In many cases with RL and MDPs, we
measure the expected reward-to-go, which is the value function `V(s) =
E[reward-to-go]`. But for obvious reasons, we want to know its variance
`Var(reward-to-go)` (e.g. to measure the quality of two policies with similar
reward-to-go from a certain set of starting states), but this has surprisingly
not been studied much. Case in point: the state of the art is expensive Monte
Carlo sampling: roll out the trajectories, gather rewards, and then compute
variance per state. Ouch!

**What is their proposed solution?** They present temporal difference methods
to estimating the variance of reward-to-go, prove various convergence
properties, and demonstrate empirical results. They say:

> Our approach is based on the following observation: the second moment of the
> reward-to-go, denoted by `M`, together with the value function `J`, satisfies
> a linear "Bellman-like" equation. By extending TD methods to jointly estimate
> `J` and `M` with linear function approximation, we obtain a solution for
> estimating the variance, using the relation `V = M − J^2`.

Note: they don't estimate `V` directly because there isn't a *linear*
relationship in its system of equations in MDPs, as there is for `J` (covered in
standard undergraduate AI courses) and `M`. Linearity is key.

They develop equations for getting `J` and `M` (Section 3), then show how to
approximate them with sampling (Section 4).

Oddly, the experiments are with an American-style option pricing MDP. I guess
this was the standard in 2013/2014 when they worked on this? I wonder what
benchmarks they would choose if they were working on this today in late 2017.

**Future work:** I like the idea of using the variance estimates to guide
exploration, since we should have a preference towards visiting states with high
uncertainty. I think that's been addressed (perhaps indirectly) in lots of
recent work.
