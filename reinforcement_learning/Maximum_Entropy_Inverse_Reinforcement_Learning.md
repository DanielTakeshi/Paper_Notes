# Maximum Entropy Inverse Reinforcement Learning

It's about high time I grew comfortable with this paper! This is the class of
problems known as *inverse reinforcement learning*, where the goal is to recover
a cost/reward function from a provided sequence of expert trajectories (and
where that cost function could then be used for reinforcement learning to
recover the expert's policy, roughly). This paper's proposed algorithm is
*maximum entropy* IRL, i.e. MaxEnt. If there's anything I should remember from
the paper, it's this:

> Our approach provides a well-defined, globally normalized distribution over
> decision sequences.


## The Algorithm

They assume the standard Markov Decision Process setting. They assume that we
are provided expert trajectories \zeta, and we want to *imitate* the expert,
i.e. **Imitation Learning**. We assume the expert is trying to optimize a reward
function which is *linear* in its features. Each state s_i is featurized into
f_{s_i} which is a vector in R^k, and the reward is parameterized by \theta,
i.e. dot product: reward = \theta * f_{s_i}. Yes, their formula at the top of
page 2 makes sense!

Unfortunately, as I remember from CS 287 and CS 294-115, recovering these exact
weights \theta is an ill-posed problem. Several solutions for that from prior
work: *structured maximum margin prediction* and *inverse RL* and *feature
counts* (from Ng & Russell in 2000 and 2004). Problem with their IRL concept is
*ambiguity*, whereby many policies may be optimal for a given reward function,
hence we don't know which one the expert was actually following.  Thus, the goal
in this current work is to *resolve that ambiguity* as they state:

> We employ the principle of maximum entropy (Jaynes 1957) to resolve
> ambiguities in choosing distributions. This principle leads us to the
> distribution over behaviors constrained to match feature expectations, while
> being no more committed to any particular path than this constraint requires.
> [...] Under this model, plans with equivalent rewards have equal
> probabilities, and plans with higher rewards are exponentially more preferred.

Let's break this down:

- [Deterministic case] They're coming up with a formula for P(\zeta_i | \theta),
  the probabilities of paths in the MDP, and which is proportional to exp(\sum_i
  \theta * f_{s_i}), i.e. the sum of rewards, *exponentiated*.  It's common to
  exponentiate things to get this normalized (see: softmax). Make sense? =)
- [Non-deterministic case] Looks like they were playing me; in general we have
  to assume non-determinism in MDPs. =)  I think this is when \pi(a|s) results
  in a nondeterministic next state, such as 80% of the time the action takes us
  where we expect, 20% of the time it doesn't. They add indicator functions and
  a probability P_T(o) of outcomes to again figure out a way to get P(\zeta |
  \theta, T) where T is the transition probability kernel.
- They are able to formulate the problem of finding \theta^* as maximizing the
  log likelihood of P(\zeta | \theta, T). Find it via gradient descent. We saw
  this in CS 294-115: "The gradient is the difference between expected empirical
  feature counts and the learner’s expected feature counts."
- How to compute the gradient? The naive way, enumerating all paths, is
  infeasible so they use a "value iteration-like" algorithm for this; see
  **Algorithm 1**. (I don't know the details, sorry.)

Whew! That's MaxEnt IRL!


## The Experiments

The main experiments have to do with driving cars (*not* self driving cars ...
remember that this is 2008-era work). Specifically, here's the critical
description:

> Our research effort on maximum entropy approaches to IRL was motivated by
> applications of imitation learning of driver route choices. We are interested
> in recovering a utility function useful for predicting driving behavior as
> well as for route recommendation. To our knowledge, this is the largest-scale
> IRL problem investigated to date in terms of demonstrated data size.

They model a deterministic MDP with 300k states (!!) and 900k actions (!!!)
based on the roads in Pittsburgh.

Remember that this is *IRL*. What's the expert trajectory? It's simply:

> We collected GPS trace data from 25 Yellow Cab taxi drivers over a 12 week
> duration at all times of day.

Very cool! =) The drivers are the experts who are optimizing some goal with
respect to costs (fuel, time, etc.). The **features** include road type, speed,
lanes, and transitions. It looks like there are 22 features?

What are the **experimental metrics** on the held-out dataset? They use three:

- The amount of the routes shared (i.e. in common).
- The *percentage* of predicted paths which match the actual ones (I think again
  using route shared, the threshold is 90% of the route shared).
- Average log probability of paths in training set.

Hmm ... how do we extend these to non-driving domains? That will be tricky.


## My Thoughts and Takeaways

This was a groundbreaking paper, and I should definitely look and see what
literature resulted from this. I like the idea of having a well-defined
distribution over paths.

I could complain more about the experiments and ask for depth, but with six
pages it's good enough and the experiment is actually pretty cool and more
realistic than most. And this was 2008, remember. There's surely been a ton of
variants judging from the hundreds of citations for this paper. 

Nitpick: I don't understand how Figure 1 is supposed to convey determinism vs
non-determinism. Fortunately, Figure 2 is intuitive and shows the advantage of
their method over others which form a distribution based on *local* branches
(and thus may assign too high of a probability to less likely policies).
