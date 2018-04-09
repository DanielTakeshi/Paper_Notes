# Modular Multitask Reinforcement Learning with Policy Sketches

This is an exciting hierarchical reinforcement learning paper. The key idea is
to use **policy sketches**, which can be thought of as (minimally supervised)
high-level actions, but made up of **more specific tasks**, each represented by
its own neural network policy function. Those tasks can be **re-used**
throughout different sketches, as in the simple but helpful Figure 1 example (in
code, this means they have the same parameters).  Indeed, this reminds me of
Neural Module Networks.

It's a bit of a new problem setting, and also, one could argue that there's too
much supervision needed, because we assume that tasks consist of several
hard-coded sketches. Of course, this could all be filled out in a few lines of
text, so I suppose for realistic systems it's not too bad ... 

## Training

The general framework can be trained with curriculum learning and an
actor-critic algorithm. The curriculum learning scheme lets the model start with
easy tasks and then scale up to harder ones. Otherwise if it started with the
harder ones, it would never see a reward, and if it stuck with the easier tasks,
it would miss out on higher-reward stuff. Here, the "difficulty" seems to be the
sketch *length*.

An extension of the normal actor-critic algorithm is necessary because policies
for tasks act on different reward functions, but the policies themselves are the
concatenation of several lower-level policies which are mixed together. It isn't
too bad, see the math for details.

## Experiments

It seems to be robust to architecture choice:

> In all our experiments, we implement each subpolicy as a feedforward neural
> network with ReLU nonlinearities and a hidden layer with 128 hidden units, and
> each critic as a linear function of the current state.

I'm surprised that the paper doesn't have any sort of error bars or statistical
significance. There are a healthy number of baselines, so that's good, but still
not entirely convincing to my liking.
