# High-Dimensional Continuous Control Using Generalized Advantage Estimation

This is about high dimensional control, which usually means the state space x is
of high dimension, and continuous control, which usually means mapping from raw
pixels to *torques*, which are continuously-valued variables applied by the
agent.

**Main contribution**: a *family* of *policy gradient* estimators, called
"Generalized Advantage Estimators." I interpret this as a subset of policy
gradient algorithms. 

To be clear, what are the different policy gradient methods?

- Stochastic policy gradients which directly obtain/use an unbiased estimate of
  the gradient. (This is what I think of as "policy gradients.")
- Trust Region Policy Optimization, an improvement of the first item. (TRPO was
  published before this GAE paper.)
- Their contribution, GAEs? They also show how to merge it with TRPO.
- Actor-critic methods?

The key advantage of GAEs is with variance reduction with tolerable bias. As
they state in their conclusion:

> Policy gradient methods provide a way to reduce reinforcement learning to
> stochastic gradient descent, by providing unbiased gradient estimates.
> However, so far their success at solving difficult control problems has been
> limited, largely due to their high sample complexity. We have argued that the
> key to variance reduction is to obtain good estimates of the advantage
> function.

Using the **advantage function** yields the best (i.e. smallest) variance among
the estimators for the policy gradient. However, the advantage function itself
has to be estimated first, so they introduce \gamma-just estimators which are
unbiased for the advantage function.
