# Bridging the Gap Between Value and Policy Based Reinforcement Learning

This paper proposes a new algorithm, **Path Consistency Learning** (PCL), and
proves several theoretical properties, such as that it "minimizes
inconsistency," which fits well with its name!

Quick refresher: *Value-based* means we have to come up with a value function
V(s), or Q(s,a) I suppose. This probably means DQN. *Policy-based* means
updating the policy directly, as in policy gradients. Right?


## Main Ideas and Intuition

They develop a new optimality criteria for entropy-regularized rewards. See
Equation 1 for details. See also Equations 2 and 3 for examples of what it means
to be "temporally consistent," which is another way of saying "the policy is
optimal." They key is that they are proposing an *alternative* "consistency"
formulation!

They say that:

> PCL iteratively fits both a parameterized policy and a state value function
> (implicitly Q-values), bridging the gap between value and policy based
> methods.

But I'm confused ... this is already what happens in **vanilla policy
gradients**, right? We have a parameterized policy along with the value function
(serving as the baseline, or the critic). What's new here? Or maybe that value
function is not considered "the heart" of policy gradient algorithms? I'm not
sure.


## Mathematical Details

TODO


## Experiment Details

They test on a synthetic baseline and a set of six "algorithmic tasks" from
OpenAI gym. This will be interesting; I have not used any of those six tasks
before, preferring to use Atari or Mujoco stuff.


## Thoughts and Takeaways

This paper is interesting, albeit a bit hard to understand at first. Here's my
intuition: think of the normal Bellman update. When we run standard MDPs in
introductory undergrad AI courses, we have this notion of *Bellman optimality*
(or *Bellman consistency* might be another way to describe it) which describes
what happens when we're at an optimal policy. But here, they argue for a *new*
(more complicated) consistency criterion called *temporal softmax consistency*,
which when achieved, shows that our current policy is optimal in some sense.
Here, optimality is measured based on "entropy-regularized" reward. By the way,
there should be some way to map from their version to the Bellman criteria, I
assume. **EDIT**: heh, they read my mind. See Equations 2, 3, and 4.

Their contribution is *not* entropy regularization, FYI, but they *do*
introduce *discounted* entropy regularization. This seems odd, why wouldn't the
original paper have done that? Looks like some more reading for me ...

Note that one of the authors gave a guest lecture on this in [the Berkeley
DeepRL course][1]. In addition, John Canny recommended that I read this paper.
Hence, this summary!

[1]:http://rll.berkeley.edu/deeprlcourse/
