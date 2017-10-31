# Visual Semantic Planning using Deep Successor Representations

This paper continues the theme of navigation and planning in visual environments
using TORCS. (It builds upon Zhu's ICRA paper from earlier in the year). What's
different now is that they are given a planning task as input from the STRIPS
programming language, and the agent must thus learn how to follow the task. They
want the agent to understand what actions to take so that, visually, the scene
"transforms" from the initial state to the goal state.

Another novelty versus their prior work is using the DSR framework. It's not for
distal rewards or for option discovery (neither of which I think are real
benefits, after reading the DSR paper) but they use it for cross-task
generalization, because for different tasks, they can simply keep the successor
predictor while re-training only the reward predictor. Though I have to confess,
I'm still not sure I see the benefit of DSR ...

To derive DSR, just remember that `r(s,a) = \phi(s,a) * w` represents the
*immediate* reward (yeah, it's just `r(s,a)` that we use all the time) and
`\phi` is simply some feature, think of DQNs with `\phi` the processed
state/action features at any layer in the network.

So `\phi(s,a)` is a state-action feature. Then `\psi(s,a)` represents how often
`\phi(s,a)` is "represented" across the entire trajectory, discounting future
occurrences. In their words:

> Intuitively, the successor feature `\psi(s,a)` summarizes the environment
> dynamics under a policy `\pi` in a state-action feature space, which can be
> interpreted as the expected future "feature occupancy".

They restate theoretical results from prior work, but it's unclear if they
actually use it.
