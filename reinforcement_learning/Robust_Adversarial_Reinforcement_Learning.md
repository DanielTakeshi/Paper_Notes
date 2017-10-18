# Robust Adversarial Reinforcement Learning

**What problem are they trying to solve?** In RL, policies may fail to
generalize, particularly if trained in simulation and then transferred to the
real world, e.g. due to mis-modeling of friction, mass, etc. The issue of *lack
of robustness* in RL policies appears to be well-known, and since RL has made
crucial advances recently, this is an important sub-field of Artificial
Intelligence.

**What is their proposed solution?** Their algorithm, RARL, trains the
protagonist (the agent) but *also* has an adversary which is trained to model
forces and disturbances. Indeed it should remind you of GANs right away, though
the authors do not make a connection between the two works for some reason
(other than a cursory mention in related work). The adversary aims to maximize
the negative reward of the protagonist. RARL works by iteratively training a
policy for the protagonist (using TRPO) and then a policy for the adversary
(again using TRPO). The process repeats for some number of steps. Note that the
policy optimization step is designed to approximate the (intractable) method of
solving for the exact "equilibrium state" (I think that is how you describe it)
each iteration.

**Other comments.** Their baseline is TRPO. In fact, in their experiments, they
*only* test with TRPO and RARL, which is something I need to be aware of in my
research: how do I avoid testing so many baselines and just pick one? That means
a faster turnaround for me.) They test entirely in simulation: InvertedPendulum,
Hopper, Swimmer, Walker2d, and HalfCheetah, which I found odd due to some of the
discussion from simulation-to-real-world, but I guess this was still enough for
ICML. 

My concern with the experiments is that it is hard to get intuition for
how much they actually make the problem harder. But I understand their
experiments and the results seem reasonable. In fact, the average RARL policy
outperforms the average TRPO policy even *without* any added noise, and that is
a big deal (at least for these environments). They visualize the adversary's
policies and indeed, it's human interpretable: if the pendulum is tilted in one
way, the adversary learns to push in that same direction to force it to fall
over. Nice! It would be nice, however, to show what happens if we have random
noise instead of adversary noise. How beneficial is the jump from random noise
to adversary noise? That would be interesting to explore.

Also, the way they injected noise can seem somewhat fixed or artificial. Why
can't the adversary do this or that?

Update: read the ICML version, not the arXiv version (which is slightly out of
date). The versions are similar, except they tested with Ant-v1 for the final
camera-ready version, but they often omitted results anyway so there isn't much
additional content.
