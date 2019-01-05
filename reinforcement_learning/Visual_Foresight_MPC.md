# Visual Foresight: Model-Based Deep Reinforcement Learning for Vision-Based Robotic Control

A nice paper under review (likely to a journal) which summarizes some of the
prior work from robot learning, as it pertains to visual MPC. I recommend
reading this and then reading the earlier papers (from NIPS, CoRL, ICRA, etc) to
see the details.

Main idea is to run MPC but base it on visual observations. That is, we develop a
plan (via MPC and Cross Entropy Method, similar to the way DexNet does it),
execute the first action of our best plan, then re-plan with the new image we
see.

Three main stages:

- Data collection (this can be parallelized, no labeled data needed)
- Predictive model training (depends on the loss function and task)
- Planning (use Cross Entropy Method as stated earlier, with a few heuristics)

Experiments help to summarize some of the contributions of the prior papers.
Very interesting, the tasks are still pretty primitive but this is
generalization so the videos will be less impressive than if we knew the exact
task.

See also the blog about this paper: https://bair.berkeley.edu/blog/2018/11/30/visual-rl/
