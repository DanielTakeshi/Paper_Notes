# Principled Option Learning in Markov Decision Processes


## Introductory Stuff

This uses the standard MDP formulation I'm familiar with, but with costs instead
of rewards (ugh). It will do for now. The paper is proposing how to learn
"options". These are individual policies by themselves that solve some subtask.

There are several tricky things about this paper:

- How can we formalize the MDP? It would be very helpful to have rules for when
  we can use options, and what we can assume from them. Also, how do prior
  options come into play?
    
- I don't understand why the "option" definition is one that says there exists a
  termination state. Shouldn't it just be a sub-policy that we follow with a
  higher-level controller?

- How can we extend this beyond the grid domain which looks very constrained?

- Fastest way to understand these update equations? Some of them are not clear
  to me and are taken straight from previous work.

There are several definitions of "options" stated in the paper. Let's combine
them as follows:

1. An option is a policy in a special domain where the agent can choose the
   halting action $a_{\rm term}$ that terminates the episode. [Section 2]
2. An option is a pre-learned routine behavior that can be invoked by a
   high-level controller to solve some subtask. [Section 3]
3. A good option should therefore be used by many subtasks and it should be
   similar to the optimal solution for each of these subtasks. [Section 3]

Don't forget, THETA = SUBTASKS!!! It's not for anything else! These simply have
a specific starting state (hard-coded into the code after being randomly
sampled) and a specific goal state.

There is a difference between *prior* options and *optimal* options. Use \pi_h
and \pi_\theta for these.

- Intrinsic cost: KL divergence from prior option and actual executed option.
- Extrinsic cost: discounted sum of costs due to actual executed option.

These add up to form 'free energy' but are usually competing objectives. I
understand the equations now in the paper (page 3) which are different ways of
mathematically expressing free energy.

There are some KL divergences in the paper, mostly looking like
E[sum_of_log_ratios] which is fine and matches the definition.

Remember that \rho in the paper refers to a trajectory of states/actions.

Also, there are no discount factors here! We're dealing with the finite horizon
case.


## The Algorithm

The problem: minimize the extrinsic cost under the constraint that the intrinsic
cost is not too high. These are competing objectives. On the bottom of page 3 we
have:

> This solution fails to generalize to unseen subtasks and is avoided by
> constraining the assignment in some way. In this paper we consider a hard
> constraint on the number of prior options.

I finally understand what they mean. The key is the assignment function 'q',
which is what allows the following to happen:

> At the beginning of each episode, the high-level controller observes the
> subtask $\theta$ and gives the low-level controller a hint $h$.

I can see why we need a constraint on $q$, and an easy way to do that is to
limit the prior options.

Let's say we start with two prior options indicated by \pi_h. (In the
experiment, they're actually randomly assigned at the start, but the good news
is that the algorithm repeatedly updates them, in fact it seems a lot similar to
E-M updates.) Then after seeing the SUBTASK, i.e. (start,end) state pair, the
"high-level controller" has to choose one of the two prior options to execute.

It can execute that prior option, but the results should be bad (at least in the
beginning) so it can also use an "optimal" policy, indicated by \pi_\theta. 


## Experiments

For the paper, the goal was to investigate the impact of two options on a
two-room domain with a narrow corridor. Subtasks are sampled so that the start
and end states are on opposite sides of the corridor. From definition 3 above,
the two learned options should be optimal for a variety of subtasks, and the
easiest (most straightforward) partitioning is when one option is for going from
left to right and the other is for the reverse direction.

I haven't been able to get the algorithm running cleanly yet, but I hope that
the algorithm was automatically able to determine the two different prior options,
i.e. it was not hard-coded that one prior goes left to right and the other one
in the reverse direction.


## How to Run on Different Domains

TODO I am not sure yet


## My Thoughts and Takeaways

If this were to be formulated as a clear problem statement, it would probably
be informally:

> Given (MDP information) output (best q, \pi_h, \pi_\theta)

The output of q and \pi_h make sense since they go hand in hand, given a subtask
q must give us the best prior to use. My guess as to why \pi_\theta is there is
that it tells us the true optimal policy to use for a subtask. Since we limit
the priors (i.e. same thing as limiting the options) we want to test to see that
our priors are close enough to the optimal policies, i.e. that they sufficiently
generalize and cover different areas of the policy space (e.g. L-R vs R-L
directions). For that, for a given subtask \theta, we'd compare \pi_\theta along
with the \pi_q(\theta) which is the prior our learned q will choose. Yeah, I
think that makes sense.
