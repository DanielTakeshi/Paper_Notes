# Evolution Strategies as a Scalable Alternative to Reinforcement Learning

Just to be clear, how do we define these algorithms? See:

> [...] Evolution strategies (ES), the approach of derivative-free hill-climbing
> on policy parameters to maximize a fitness function.

This reminds me of the *Cross Entropy Method*. I also remember *Covariance
Matrix Adaptation*.  Take note of the perceived advantages/disadvantages of ES
vs reinforcement learning methods. Hopefully I'll get comparisons in [my RL/IL
code base][4]. Note: there's a third set of evolution strategies, called
*Natural Evolution Strategies*, and this is what their stuff is based on mostly.


## The Algorithm

NES: They represent the objective as F(\theta), where here F is the return (i.e.
reward sum in some form) from the agent, and \theta represents the policy's
neural network weights. NESs also use a \Phi parameter for a distribution over
\theta, and here they treat it as a Gaussian (I "guessed" this correctly =)) so
we can ignore \Phi and deal with \theta only. They derive the score function
estimator, which looks very similar to the policy gradient estimate.  Note that
despite how I think of ES as "derivative free" like cross entropy, we actually
use one derivative here: the score function derivative. Of course, it's not a
particularly expensive derivative ... in fact, the cross entropy method might be
a special case of a gradient-style update anyway (actually, any x <- x +
(lr)*stuff would qualify).

See **Algorithm 1**, which is what Karpathy implemented. They alternate between
perturbing parameters (plus evaluating them by running episodes), and using a
stochastic gradient update to combine the results in a useful way. Also see
**Algorithm 2** about the parallelized version. They mention a few tricks there.
One is to make sure all workers are roughly used together, so there are no
"early finishers," so they cap the episode lengths. That makes sense.

Another important trick for Atari games: naive random perturbation of parameters
does not always lead to sufficiently different results among the policies, so
they use **virtual batch normalization**. I will need to read more about that.

Wait, they cite the "Deep Learning Without Poor Local Minima" to partially
justify the use of larger networks over smaller networks. I wonder how much of
that paper they read in detail (it's hard to read). Note: I'm surprised they
didn't cite the 2015 paper "The Loss Surfaces of Multilayer Networks."


## Experiments

Their goal is not to show that ES are *better* than other reinforcement learning
algorithms (that would be really hard) but to just achieve "comparable"
performance. That's a great goal for their paper. And actually, they're able to
get much better *timing* performance, which itself would probably be enough for
a good paper.

They use the standard **MuJoCo** and **Atari** environments as benchmarks.


## Thoughts and Takeaways

ES are old, as we all know, and this paper seems to be the first to be able to
scale them (including parallelization) and to use them to solve challenging RL
tasks. At this point, I'm wondering if we should move on to other MuJoCo stuff
or broader tasks. They claim that:

> We found the evolution strategies method to be highly parallelizable: we
> observe linear speedups in run time even when using over a thousand workers.
> In particular, using 1,440 workers, we have been able to solve the MuJoCo 3D
> humanoid task in under 10 minutes.

How much is 1440 "workers", relatively? This sounds like far more than anyone's
ever done.  Also:

> Our 1-hour ES results require about the same amount of computation as the
> published 1-day results for A3C, while performing better on 23 games tested,
> and worse on 28.

Wow. One hour.

Their discussion about gradients and about PG vs ES is interesting, but I cannot
for now verify their results. I will simply have to trust what they say.

The fact that they **do not need a value function** is quite useful!

Jurgen Schmidhuber is cited 11 times in this paper!!

It seems like this is going to be a relatively popular paper. See the following
links:

- [OpenAI blog post][1]
- reddit [discussion 1][2] and [discussion 2][3], including some skeptics. =)

Unfortunately, I am not sure if there is much room for improvement, but we will
see.

[1]:https://blog.openai.com/evolution-strategies/
[2]:https://www.reddit.com/r/MachineLearning/comments/5zbap7/r_170303864_evolution_strategies_as_a_scalable/
[3]:https://www.reddit.com/r/MachineLearning/comments/619x1g/revolution_strategies_as_a_scalable_alternative/
[4]:https://github.com/DanielTakeshi/rl_algorithms
