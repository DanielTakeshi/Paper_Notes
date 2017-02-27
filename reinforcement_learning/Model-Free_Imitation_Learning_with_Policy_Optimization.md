# Model-Free Imitation Learning with Policy Optimization

(The precursor to the G.A.I.L. paper.)

**TL;DR** Focus on the *imitation learning* problem: training an agent using
demonstrations provided by an expert. Instead of the bad alternatives of
behavioral cloning and raw IRL, use their method, which avoids learning a cost
function but nonetheless finds a policy (using apprenticeship learning) as good
as the expert and which scales better than existing algorithms. They provide two
model-free algorithms based on (1) policy gradients and (2) TRPO.

Note: IRL is bad because of the need for an internal RL loop. I didn't know
about this originally but now I think it's common knowledge based on how many
people have talked about it. Remember that for the future! 

**Question**: what does it mean when they say:

> [...] exploiting for learning signal a class of cost functions, which
> distinguish the expert policy from all others.

Why does this "class" constitute a "learning signal?" My answer: I think it is
because apprenticeship learning *assumes* that the true cost function belongs to
a certain class (e.g. linear), and they are using some form of apprenticeship
learning, hence they repeat that assumption.

I'm also wondering what makes these "model-free" algorithms. My answer: I think
it is because they never have to "model" or "define" the cost function.


## Background, Theory, and Algorithms

The notation is clear so far.

I'm still not sure how apprenticeship learning works if the restriction is that
we have to perform "as good as the expert." That could be very challenging,
right, even when considering the restriction of the cost function to be in a
class (of perhaps linear functions)? And didn't the G.A.I.L. paper "only" get
70% of the expert performance?

**NOTE**: Their "state-action visitation" \rho_\pi(s,a) is exactly the same as
the "occupancy measures" as stated in the G.A.I.L. paper. Though those are
*un-normalized* probability distributions, right? But I guess it doesn't matter
for computing expected costs because the costs are simply rescaled without
changing ordering if we normalized, right?

Apprenticeship learning observations: the formulation at the beginning of
Section 3 makes sense. It took a second read, but I didn't realize that
apprenticeship learning had to satisfy the cost of \pi being lower than the cost
of \pi_E for **all** c \in C. That seems like a tall order!

**KEY ASSUMPTION**: They assume only *linear* functions for C!! Their focus is
on the optimization procedure for apprenticeship learning, not to broaden C.
(For broadening C, consider their G.A.I.L. paper.) Their methods have a "certain
way" to solve the optimization problem in Equation 3, whereas prior work
(feature expectation matching and game theory) use other ways to solve Equation
3.

I'm not sure why this follows (specifically, how they can use state-action
visitation frequencies...):

> Syed et al. are therefore able to formulate a single linear program on
> state-action visitation frequencies that simultaneously encodes (3) [...]

but it seems important to them, because they are doing the same thing as the
above, except replacing state-action visitation with "parameterized stochastic
policies" (i.e. what I'm familiar with, policies which give probabilities to
resulting actions and which are parameterized with weights). I guess they are
also not using linear programming either ... also, it seems like it would make
more sense to optimize over policies anyway, rather than something indirect?
Perhaps optimizing over policies is more complicated?

**Section 4** describes their algorithm. As I suspected, they use some fixed
class of policies parameterized by valid \theta, so the optimization is
equivalently done over \theta. They take the gradient with respect to theta and
arrive at the expected answer in Equation 7. They argue for a two-step process:

1. Find the optimal cost function. (But wait, isn't that complicated IRL?!?
EDIT: hmm ... they have a way of finding it quickly, it's just a normalized
version of some vector, and it's enough to find the parameterized version w.)
2. Improve policy given this cost function. (I assume by using gradient-based
methods.)

Yes, my thoughts are right for this second method, they use standard policy
gradients in **Section 4.1** and then TRPO in **Section 4.2** (wow, that
sub-section is long, partly because a third of it is purely a review of TRPO).
It's important to have that section because --- as I know from others --- the
gradient estimates in Section 4.1 have *high variance*.  Equations 9 and 10 make
sense because they basically convert the idealized formula using \eta into
empirical expectations. We cannot get true expectations in practice, right? 

I don't quite follow Equations 11 and 12?  I'm also confused about how L is the
same as \eta up to first order at \theta_0?

The part about how to transform TRPO into their apprenticeship learning setting
is technical. They're trying to find that something "majorizes" their objective
function and are borrowing some math from TRPO to do so (I really need to read
the TRPO paper and *all* the proofs, but that would take ages). They define a
new function, M which upper bounds it.

They they use the same empirical argument from TRPO to use average KL
divergence, etc. OH, and then they do *importance sampling* after this. Wow, a
lot of stuff to do to compute an objective. I wish I understood why "the
identity" holds (just before Equation 23).

**Section 4.3** describes how to compute the cost function, which is similar for
both C_linear and C_convex, the two linear cost classes they consider for the
paper. But why are we assuming feature expectations here? I'm confused. I can
understand why we are assuming that cost functions can be parameterized by
vectors "w" (and thus finding optimal "w" is the same thing as finding the
optimal cost function) but why are we assuming feature expectations again?

Wait, I think I get it, we assume feature expectations because, while it's true
those came from prior work, that's the stuff they are *keeping*. Prior work
solved this by doing slow, incremental IRL but they are doing stuff in Sections
4.1 and 4.2 here (while *keeping* the cost function class, hence while keeping
the feature expectations vector!). Remember that they said they're not focusing
on modifying the cost class here.

Part of it is that they discuss their contributions in Sections 4.1 and 4.2 and
then "go back" to 4.3. I'd suggest moving the description back, except the
solution for "w" in the TRPO-like case relies on their TRPO description.


## Experiments

Unlike the G.A.I.L. paper, the expert trajectories were generated from a variety
of algorithms (not just TRPO).

I *think* when they said:

> In all of the continuous environments, we used policies constructed according
> to Schulman et al. (2015): the policies have Gaussian action distributions,
> with mean given by a multi-layer perceptron taking observations as input, and
> standard deviations given by an extra set of parameters.

that means the policies *they* are trying to derive are fully-connected neural
networks. Unfortunately, I can't find any architecture discussion.

Also, what are tabular Boltzmann policies?

So, in full, here are their four sets of experiments:

- **Grid-Worlds.** The optimal policy comes from value iteration.

- **Moving in a Plane.** This is from a Sergey Levine paper in 2012. The optimal
  policy is due to an algorithm from that paper which also uses model dynamics,
  which *they are not using*.

- **Andrej Karpathy's Demo.** The agent is swimming. See the demo, it's really
  cool! (I need to learn JavaScript someday.) The optimal policies were
  generated by, I think, running some "normal" reinforcement learning which
  knows the cost function.

- **Highway Driving.** This is also from that same Sergey Levine paper. The
  optimal policies were generated the same way as in the other experiment (why
  didn't they list the two Levine-related experiments in back-to-back
  subsections?).

Remember here that evaluation is based on how close the learner is to the
expert, or if the learner is *better*. In Figure 3 for instance (corresponding
to Andrej Karpathy's demo) the metric is based on "excess cost" so the
assumption is that the learned imitating agent will have performance bounded by
the expert performance. This also appears in Figure 2, FYI.

PS: It looks like four is a good set of experiments to have for an ICML paper.


## My Thoughts and Takeaways

There are lots of connections with the [G.A.I.L. paper][1]. They are focusing on
the same problem (imitation learning) with tackling the same issue (that
generating the cost function is expensive and probably not even worth it).

Some interesting intuition I got: in IRL, the assumption of expert optimality
serves *as a prior* on the potential space of policies for our trained agent.

**More intuition**: how do we know if a cost function "fits" an expert policy?
(Or more accurately, expert *demonstrations*?) Well, we can run normal
reinforcement learning on that cost function, so let's run policy iteration
(LOL) and figure out the optimal policy we get. Then compare that policy with
the expert demonstrations? If simple policy iteration gave us something that was
a lot better, then why would the expert trajectories be like that? The expert
trajectories should be more like that policy we just got!

I really need to discuss this paper with someone.


[1]:Model-Free_Imitation_Learning_with_Policy_Optimization.md
