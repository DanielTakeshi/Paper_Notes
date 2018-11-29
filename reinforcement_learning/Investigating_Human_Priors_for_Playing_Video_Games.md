# Investigating Human Priors for Playing Video Games

Interesting ICML paper on understanding what makes humans good at playing video
games. The authors perform systematic ablation studies to remove several aspects
of the game, and find that some removals result in severe degradations in
performance.

Some of the conclusions appear rather obvious (e.g., Figure 1) but I suppose
this is w.r.t the human side --- an RL agent (again, see Figure 1) can do well
even if the colors are a bit nonsensical to humans. Note: the RL agents are the
curiosity-driven RL agents from the ICML 2017 paper, because that can solve
sparse reward environments.

They benchmark on video games because it is easy to do lots of ablations, and
because it's a common RL benchmark. Agreed, looks good. Though a potential
downside is that they designed their own video game. It ... would probably be
better to use some existing game but I understand if that would be too hard to
code.

Details on the humans:

> We quantified human performance on each version of the game by recruiting 120
> participants from Amazon Mechanical Turk. Each participant was instructed to
> finish the game as quickly as possible using the arrow keys as controls, but
> no information about the goals or the reward structure of the game was
> communicated. Each participant was paid $1 for successfully completing the
> game. The maximum time allowed for playing the game was set to 30 minutes.

Look at the paper's figures. They are informative.
