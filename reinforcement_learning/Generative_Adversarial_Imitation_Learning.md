# Generative Adversarial Imitation Learning

Main idea: The setting is Imitation Learning and the agent's goal is, given a
bunch of expert trajectories, to imitate the expert. No other information is
provided, not even the rewards from that trajectory. They want this to scale. To
do so, they propose a *model-free IL* algorithm, and show that it exhibits
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
spaces. Can this work be boiled down to: extension of ICML 2016 paper with
generalized cost functions?

Unfortunately, I don't understand Equation 12 that well and it seems like I'd
have to read their previous paper. Fortunately, it relates to policy gradients
and TRPO so I have some intuition.

**Section 5** describes their G.A.I.L. algorithm at last. They derive a new
convex regularizer that lets them extend to generalized cost functions. I
plotted the function g(x) and it looks interesting, it seems like it decreases
as it approaches something like -1.5 (whatever its minimizer is) and then it
shoots up to infinity with x=0 and beyond that they explicitly set it to be
+infty.  Another insight is that they include the expert's performance *inside*
the regularizer's metric. I'll have to think more carefully about what this
means.

Equation 14 reminds me of the formulas for GANs. And naturally they used "D" to
help intuition. =) I will have to read these papers side-by-side and compare.
They argue it is also related to the Jensen-Shannon divergence (similar to KL
divergence). Ultimately, they derive Equation 15 which describes how to derive
*a policy* ... whose *occupancy measure* has minimal "divergence" from the
expert's occupancy measure.

Insight: the generator G is trying to generate a distribution, which here is
\rho_\pi, and is attempting to make it as hard as possible for a discriminator D
to distinguish between \rho_\pi and \rho_{\pi_E}, the expert!

Is this line of thinking right?

- Equation 14 shows that \psi_{GA}^* is picking the best classifier that can
  distinguish among the current policy and expert policy? I.e. the *best
  discriminator*?
- Thus Equation 15 is showing that the generator needs to *minimize* Equation
  15.

See **Algorithm 1** for details, where they explicitly find saddle points
(why?). This is an *iterative* procedure, just like training GANs.

PS: the generator tries to generate draws from a true distribution, where the
"draws" are policies that closely match the expert! In practice, the paper says
the generator draws *occupancy measures*, but that's the same thing as policies.


## Experiments

**Experiments**: they use 9 physics-based control tasks, either simple baseline
classics or more advanced MuJoCo ones (yeah, everyone uses that). No Atari 2600
games, though. The advanced ones, according to the paper, were "only recent
solved by model free RL" such as TRPO, so we finally have a competitor.

I see, **the experts are derived from TRPO**. Humans did not generate them.

They show that their GAIL algorithm is able to get about 70% of the expert (i.e.
TRPO) performance. That may not sound great, but it's better than the three
baselines: behavioral cloning and two apprenticeship learning variants. Remember,
IL papers are about trying to closely match (or in rare cases, exceed) the
expert, so we shouldn't expect new records being broken in terms of pure reward
(or cost). The last two baselines were in their ICML paper. **UPDATE**: it's
even better than 70% usually. The 70% figure is across *all dataset sizes*, some
of which are really small and therefore difficult to learn from.

By the way, they also test on something else: *dataset sizes*. This is good, we
can see which algorithms are sample-efficient by getting high performance (close
to TRPO) with few trajectories in the overall dataset. In fact, for the classic
control tasks, the smallest dataset has **one** trajectory?!? In other words, we
run TRPO on the algorithm. Then we execute it for one trajectory. And ... that's
our dataset that we feed into the GAIL algorithm, so during each iteration, when
it "samples" trajectories from the dataset, the minibatch size is one and is
always the same. Is this line of thinking correct? Maybe they had to reduce the
number of trajectories to ensure that behavioral cloning and the other baselines
were not good enough? It's fine to do this, because we want to know which
algorithms are sample-efficient so that means restricting the set of expert
policies from which the algorithms can sample.

PS: the last paragraph assumes the numbers here represent trajectories in the
entire dataset, not how many are sampled _per_iteration_, which is something
different. I'm not sure what values they used.

They say their algorithm is sample efficient, which I agree, though it's
interesting because they use a TRPO step and TRPO is supposed to be sample
*inefficient*. There seem to be two different "sample efficiency" metrics here,
one for the number of trajectories, and another for environment interaction. For
TRPO, how does the step work? Does it have to roll out its policy for many
trajectories?


## My Thoughts and Takeaways

I'm still confused about lots of this paper, but I'm getting there in terms of
understanding. I am trying to gauge the impact of this paper. Maybe I can read
existing code or other blog posts about this. I should definitely look [at this
code][2] right away!

For future work, it looks like we should *not* try and be more sample efficient
(i.e. with fewer trajectories in the database of expert trajectories) because it
would be hard to beat GAIL.

There are about 4-5 IRL papers that I want to read now. =) Whether I like it or
not, I will have to read those to probably understand this paper.

[1]:http://math.stackexchange.com/questions/223235/please-explain-the-intuition-behind-the-dual-problem-in-optimization
[2]:https://github.com/openai/imitation
