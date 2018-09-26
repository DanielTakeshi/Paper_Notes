# Proximal Policy Optimization Algorithms

This is a high-level, relatively easy to read paper from OpenAI. As far as I
understand, Proximal Policy Optimization itself isn't new (I remember seeing it
in CS 294-112 code) but they are proposing some slight modifications to the
objective function here. Indeed, the CS 294-112 slides from Spring 2017 argue
that PPO is "basically TRPO but only first-order optimization."

Main points:

- We almost never want pure stochastic gradient updates for DeepRL policies
  because a small change in the weights only *weakly* correlates with small
  changes in policies. The better way to deal with this is not to reduce the
  step size but to **constrain the KL divergence** of the before and after
  policies. TRPO does this by first optimizing a surrogate loss and then
  constraining the KL divergence of the update. PPO is claimed to be easier to
  implement while sharing these semantics and having better sample efficiency.
  That's good!

- Their objective function uses "clipped probability ratios" to form a lower
  bound on the policy performance.

- PPO involves alternating between data collection and several weight updates
  ("optimization updates") via gradient ascent. The alternation between sampling
  and updates is typical for policy gradient algorithms, but what's new here is
  the *multiple* updates. I think I can see why. With GANs, I've tried to do
  several updates for the Generator vs one for the Discriminator and while
  appealing, that can be empirically bad (as I've observed) and something
  similar might apply to RL, as implied by the paper.

- Objective function is like the surrogate objective, except it has a min and
  the second part of the min uses a clip on the probability ratios of the
  policies. They argue that if that ratio is too large, then the policy update
  would be too large. I need to look at this again.

- In VPG, I separated value function estimation (for reducing gradient variance)
  and the actual objective function, though it's possible to put them together
  as they imply in Equation 9. Yes, see John Schulman's slides when he talks
  about vanilla policy gradients and practical implementations with automatic
  differentiation software. They don't share parameters so this stuff can be
  done separately like I am used to, and it's probably easier to reason about
  and to debug.

- Their experiments show that `epsilon = 0.2` works best for clipping. And then
  they compare with other algorithms from the literature, including A2C which,
  amusingly, they say gets better performance than A3C. Um ... how? A3C should
  just mean a faster algorithm due to parallelism? Also, I really wish Figure 3
  was more clear, by the way.

Closing thoughts:

- I do not know why this is true:

  > [...] trust region policy optimization (TRPO) is relatively complicated, and
  > is not compatible with architectures that include noise (such as dropout) or
  > parameter sharing (between the policy and value function, or with auxiliary
  > tasks).

- I will try implementing PPO one of these days. I promise! I also have to again
  figure out how to get my TRPO code to work (fortunately I can borrow from
  others online).

- I need to collect a set of all the policy gradient algorithms I need to know,
  DDPG, TNPG, TRPO, PPO, and obviously implement them in my RL repository.

- Well ... hopefully this algorithm is the real deal!

**Evaluation update**:

> Each algorithm was run on all 7 environments, with 3 random seeds on each. We
> scored each run of the algorithm by computing the average total reward of the
> last 100 episodes.

So, for the 7 MuJoCo environments (half cheetah, hopper, etc.) they use the
average total reward over the last 100 episodes. Again, this is during training,
so no held-out trajectories, which is consistent with what I am seeing.

Now for Atari:

> A table of results and learning curves for all 49 games is provided in
> Appendix B. We consider the following two scoring metrics: (1) *average reward
> per episode over entire training period* (which favors fast learning), and (2)
> *average reward per episode over last 100 episodes of training* (which favors
> fast performance).

I see, so again, during training, and they also use the last 100 episode
average. The difference here is using average reward over *entire* training
period. Hmm ...
