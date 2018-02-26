# Diversity is All You Need: Learning Skills without a Reward Function

A somewhat surprising result, all you need to do is:

- Have a prior over a set of skills (each of which is a policy) and in which the
  prior and the skills explore the state space sufficiently while being distinct
  from each other.
- Use maximum entropy to make policies diverse for the prior just set it fixed,
  the uniform distribution.
- Use a (learned) discriminator to infer the skill from a given state. We want
  skills to be distinguished by *states*, NOT actions.
- When we do get lots of diverse skills, *some* will be useful and can solve
  tasks.

The authors have lots of experimental results on simulate control domains, and
indicate that it can be used for imitation learning, for pre-training in RL,
etc.

There seems to be some issues in ML reddit over whether the algorithm is novel.
However, the experiments on the simulated control domains are likely novel, and
I am fairly impressed by the breadth of experimental questions they're
exploring.
