# Q-Prop: Sample-Efficient Policy Gradient with an Off-Policy Critic

Main contribution:

> We present Q-Prop, a policy gradient method that uses a Taylor expansion of
> the off-policy critic as a control variate. Q-Prop is both sample efficient
> and stable, and effectively combines the benefits of on-policy and off-policy
> methods.

I haven't thought of policy gradient methods as "stable learning", but perhaps
they're more stable than TD-Style methods (they're definitely better in terms of
*bias* because the policy gradient is technically unbiased)? In terms of
variance and sample efficiency, though, the latter are better than the former,
and the reason might be clearer: Q-Learning (to take an example of an off-policy
method) can use samples derived from its history, whereas on-policy policy
gradients have to use on-policy samples and the distribution of those *changes*
after each parameter update. I *think* that makes sense. I need to get this all
documented somewhere.

To be clear:

- State of the art for **on-policy** is *Trust Region Policy Optimization* (with
  GAE).
- State of the art for **off-policy** is *Deep Deterministic Policy Gradients*.

Huh, interesting ... that must be why people like DDPG so much. It's the best
(well, former?) off-policy method (it's also an actor-critic). Well, I'm glad I
read that paper! Now I just need to implement it in my `rl_algorithms`
repository.

**The Title**: I wonder, why does it say an "off-policy critic"? Most critics
are Q(s,a) values, and the value function is often separate from the policy,
right? For instance, with normal Q-Learning, the Q-values are off-policy anyway?

Their **Q-Prop** estimator is in Equation 8, with two parts: an analytic
gradient of the objective (which uses a value function, just like vanilla policy
gradients) and a second part that tell show to compute the estimated advantage.
Again, the emphasize that they can use **off-policy data** to improve the
critic, which is not the case with on-policy policy gradient methods. A better
critic means better performance!

**TODO go through the derivation?**

To be honest, this paper is hard to read fully. I will have to go through the
math at some point in the Appendix.

**Experimental Results**:

- They build on top of TRPO and GAE

- Their code is open-source and based on rllab. OK, I'll check it later if
  needed.

- Yeah, their curves look good, their curves "rise higher and faster" than the
  others.
