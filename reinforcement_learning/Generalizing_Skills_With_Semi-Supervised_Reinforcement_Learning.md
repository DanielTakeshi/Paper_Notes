# Generalizing Skills With Semi-Supervised Reinforcement Learning

Running RL for a robot but when we don't always have the reward function, so
it's "semi-supervised". The idea is, we have a set of MDPs, i.e. tasks.  Some
are "labeled" with reward functions that we can evaluate for improving the
agent, but some --- most --- aren't, so it's important to figure out how to use
knowledge from the labeled MDPs and generalize to the unlabeled ones by setting
an appropriate prior on the unlabeled MDPs. The problem formalism is
"Semi-Supervised RL" (SSRL). (The connection with MAML, which would come a few
months later, now seems more apparent.)

This has connections with lifelong learning: how can an agent continually learn
on tasks without access to a reward function, or at least one that isn't as easy
to define as a video game score? They say:

> In this work, we formalize this as the problem of semi-supervised
> reinforcement learning, where the agent must perform RL when the reward
> function is known in some settings, but cannot be evaluated in others. As
> illustrated in Figure 1, we assume that the agent can first learn in a small
> range of “labeled” scenarios, where the reward is available, and then
> experiences a wider range of “unlabeled” scenarios where it must learn to act
> successfully, akin to lifelong learning in the real world.

They propose **semi-supervised skill generalization (S3G)** for doing this,
based on guided cost learning.

Evaluation is based on MuJoCo (yep, that again!) and they defined their own set
of continuous control tasks that allow for generalization tests since the
original MuJoCo environments had testing conditions that were exactly the same
as the training conditions. I imagine if they were doing this today, they'd use
OpenAI's newer environments which allow for better generalization testing?
