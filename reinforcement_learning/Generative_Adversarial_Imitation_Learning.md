# Generative Adversarial Imitation Learning

Note: this is very much in draft stage as I'm a bit confused about the paper.

Main idea: The setting is Imitation Learning and the agent's goal is, given a
bunch of expert trajectories, to ... imitate the expert! (No other information
is provided, not even the rewards from that trajectory.) They want this to
scale. To do so, they propose a *model-free IL* algorithm, and show that it
exhibits with improvements over the current state of the art. It can be thought
of as a combination of IL and Generative Adversarial Networks. Gee, am I glad I
read about GANS before =). 


## Preliminaries and Motivation

Notation, etc.:

- Normal MDP notation with a *cost* function c(s,a). 
- Main IRL stuff: \pi_E is expert policy (we don't have access), \tau is the
  expert trajectory, and \hat{E} represents empirical expectation over those.
- IRL setting: use *maximum causal entropy IRL*. Huh, not maximum entropy IRL. I
  don't know the difference. Looks like more paper reading for me. =) But the
  point is that it gives us what we hope is an accurate cost function.
- Other stuff: \rho for "occupancy measure" metric, how often we visit (s,a)
  pairs, *very* important for the paper's theory and yes it helps us write
  E[c(s,a)], agreed. This leads to \pi_\rho, the *unique* (according to prior
  work) corresponding policy for an occupancy measure. Also important:
  RL(\tilde{c}), the policy obtained by running RL on the cost function obtained
  by IRL. They prove its characterization.

They don't use "behavioral cloning" which is supervised learning from
(states->actions) data. I tried this on Atari 2600 games on human data and the
agent is terrible, scoring zeros. This is due to *covariate shift*. 

Thus, they look at IRL. However, they don't use vanilla IRL because:

> Given that learner's true goal often is to take actions imitating the expert
> --- indeed, many IRL algorithms are evaluated on the quality of the optimal
> actions of the costs they learn —-- why, then, must we learn a cost function,
> if doing so possibly incurs significant computational expense yet fails to
> directly yield actions?

Very interesting. I can agree with this. We want to use IRL, to maintain its
benefits over behavior cloning, yet use it to *directly find a policy* instead
of a cost/reward function. How do they figure this out? 


## Technical Details and Intuition

**Section 3** helps them do so by looking carefully at the RL policies we get
from IRL. Are there patterns we can identify? **Equation 3** in this section
formalizes their first problem (but again, this is only *part* of the process
towards their goal, not their final result). They use some information theory
and convex optimization to show two critical facts:

- TODO
- TODO

OK, what comes after this? **Section 4** shows that ...

**Section 5** shows that ...

**Experiments**: they use 9 physics-based control tasks, either simple baseline
classics or more advanced MuJoCo ones (yeah, everyone uses that). No Atari 2600
games, though. The advanced ones, according to the paper, were "only recent
solved by model free RL" such as TRPO, so we finally have a competitor.


## My Thoughts and Takeaways

I'm very confused about how this was derived. =(

There are about 4-5 IRL papers that I want to read now. =) Whether I like it or
not, I will have to read those to probably understand this paper.
