# Model-Free Imitation Learning with Policy Optimization

(The precursor to the G.A.I.L. paper.)

**TL;DR** Focus on the *imitation learning* problem: training an agent using
demonstrations provided by an expert. Instead of the bad alternatives of
behavioral cloning and raw IRL, use their method, which avoids learning a cost
function but nonetheless finds a policy (using apprenticeship learning) as good
as the expert and which scales better than existing algorithms.

Note: IRL is bad because of the need for an internal RL loop. I didn't know
about this originally but now I think it's common knowledge based on how many
people have talked about it. Remember that for the future! 


## Theory and Algorithms

Notation is fairly clear; the setting (apprenticeship learning) also clear
though to a lesser extent for me. I'm still not sure how apprenticeship learning
works if the restriction is that we have to perform "as good as the expert."
That could be very challenging, right, even when considering the restriction of
the cost function to be in a class (of perhaps linear functions)? And didn't the
G.A.I.L. paper "only" get 70% of the expert performance?


## Experiments

TODO


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


[1]:Model-Free_Imitation_Learning_with_Policy_Optimization.md
