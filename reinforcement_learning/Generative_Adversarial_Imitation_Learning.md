# Generative Adversarial Imitation Learning

Note: this is very much in draft stage as I'm a bit confused about the paper.

Main idea: The setting is Imitation Learning and the agent's goal is, given a
bunch of expert trajectories, to imitate the expert. No other information is
provided, not even the rewards from that trajectory. They want this to scale. To
do so, they propose a *model-free IL* algorithm, and show that it exhibits with
improvements over the current state of the art. It can be thought of as a
combination of IL and Generative Adversarial Networks. I'm glad I read
about GANS before! Also, unlike most other IL algorithms which are related to
**IRL** (don't forget the "R" there!), their goal is to *directly* learn a
policy (not a cost function) and in fact they bypass intermediate IRL steps.


## Preliminaries and Motivation

Notation, etc.:

- Normal MDP notation with a *cost* function c(s,a). 
- Main IL (and IRL) stuff: \pi_E is expert policy (we don't have access), \tau
  is the expert trajectory, and \hat{E} represents empirical expectation over
  those.
- IRL setting: use *maximum causal entropy IRL*. Huh, not maximum entropy IRL. I
  don't know the difference. Looks like more paper reading for me. =) But the
  point is that it gives us what we hope is an accurate cost function.
  **Update**: After re-reading Equation 1, it makes more sense to me now. For
  instance, since we're maximizing, we want to decrease the cost of the term we
  subtract away, which is the (empirical) cost of our expert policies! But later
  in Equation 2, it looks like they have to find a policy (i.e. do reinforcement
  learning!!) *within* the IRL algorithm. That's precisely what they *don't*
  want!
- Other stuff: \rho for "occupancy measure" metric, probability of visiting
  (s,a) pairs, *very* important for the paper's theory and yes it helps us write
  E[c(s,a)], agreed. This leads to \pi_\rho, the *unique* (according to prior
  work) corresponding policy for an occupancy measure. Also important:
  RL(\tilde{c}), the policy obtained by running RL on the cost function obtained
  by IRL. They prove its characterization.

They don't use "behavioral cloning" which is supervised learning from
(states->actions) data. I tried this on Atari 2600 games on human data and the
agent is terrible, scoring zeros. This is due to *covariate shift*. (In Pieter's
287 class, behavioral cloning was considered as the alternative to IRL, and
Pieter argued that in planning tasks, the reward function is more succinct than
the traces of the expert's policies.)

Thus, they look at IRL. However, they don't use vanilla IRL because:

> Given that learner's true goal often is to take actions imitating the expert
> --- indeed, many IRL algorithms are evaluated on the quality of the optimal
> actions of the costs they learn —-- why, then, must we learn a cost function,
> if doing so possibly incurs significant computational expense yet fails to
> directly yield actions?

Very interesting. I can agree with this. We want to use IRL, to maintain its
benefits over behavior cloning, yet use it to *directly find a policy* instead
of a cost/reward function. How do they figure this out? Don't forget that when
Pieter talked about IRL, he mentioned the cost function being succinct but not
necessarily *computationally feasible to compute*.


## Technical Details and Intuition

**Section 3** helps them do so by looking carefully at the RL policies we get
from IRL. Are there patterns we can identify? If we knew those, we could try
reasoning with those policies and skipping the cost computation. =) **Equation
3** in this section formalizes their first problem (but again, this is only
*part* of the process towards their goal, not their final result, actually a
PRIMITIVE procedure). OK, after studying Equation 1, I understand Equation 3.

Next, they transform the problem from policy optimization into convex
optimization. They introduce occupancy measures and for the set of valid
policies, the corresponding set of occupancy measures can be expressed with
affine constraints. There's a 1-1 correspondence between \rho and \pi, which
actually makes sense to me (that's what I thought, because \rho is defined from
a \pi, and the reverse didn't seem to change anything for me). **Proposition
3.1** simply refers to the definition of conditional probability.

Theoretical results (these are the heavy ones):

- **Proposition 3.2**: This states the characterization for the function which
  FIRST runs the IRL from Equation 3 to get \tilde{c}, then it runs
  reinforcement learning using that cost to get a policy. The characterization
  is quite simple, minimizing negative entropy and the **conjugate** of the
  regularizer's value (values will be worse with larger difference among the
  expert and proposed policy's occupancy measures, well *that* part I get). I
  still don't have intuition on conjugate functions. =( I will [read this
  soon][1]. I also have not verified the proof.

- **Lemma 3.1**: The intuition on their \bar{H} function is that it provides
  equivalence/links between policies (and the *entropy* of those policies) and
  occupancy measures. The formula looks similar to entropy except with an extra
  "scaling" term (I wouldn't call it "normalization"). I verified a little bit
  of this in the Appendix proofs.

- **Corollary 3.2.1**: This is the special case of Proposition 3.2 that looks at
  a *constant* for \psi, a.k.a. there is *no* regularizer, so in other words,
  w/out regularization, we get the expert policy. I have not verified the proof
  but it looks intriguing, somehow we're deriving a dual, and the Lagrangian has
  Lagrange variables that *correspond to the costs* ... so think of those as the
  \lambda and \alpha that I used a lot in EE 227BT and EE 227C.

Key takeaway, from the paper:

> IRL is traditionally defined as the act of finding a cost function such that
> the expert policy is uniquely optimal, but now, we can alternatively view IRL
> as a procedure that tries to *induce a policy that matches the expert's
> occupancy measure*.

**Section 4** bridges the gap between theory and practice. In particular, a
constant \psi is not desirable and again, we don't "see" \pi_E but rather
*trajectories*, and in large problems, most N(s,a) values (# of times visiting
(s,a)) will be zero, as emphasized by Haoran Tang et al. in their #Exploration
paper. Also, how does the Section 3 analysis extend to *function approximation*?

They then develop a connection with *apprenticeship learning*. When am I going
to become an expert in that? 

I understand why Equation 9 is a special case of Equation 8.

A rather obvious conclusion: apprenticeship learning fails when the true expert
cost function is not in the set \mathcal{C} of cost functions being considered.
They then connect with their previous work from ICML 2016 to scale
apprenticeship learning (a.k.a. imitation learning) with larger cost function
spaces.

**Section 5** 


## Experiments

**Experiments**: they use 9 physics-based control tasks, either simple baseline
classics or more advanced MuJoCo ones (yeah, everyone uses that). No Atari 2600
games, though. The advanced ones, according to the paper, were "only recent
solved by model free RL" such as TRPO, so we finally have a competitor.


## My Thoughts and Takeaways

I'm very confused about how this was derived. =(

There are about 4-5 IRL papers that I want to read now. =) Whether I like it or
not, I will have to read those to probably understand this paper.

[1]:http://math.stackexchange.com/questions/223235/please-explain-the-intuition-behind-the-dual-problem-in-optimization
