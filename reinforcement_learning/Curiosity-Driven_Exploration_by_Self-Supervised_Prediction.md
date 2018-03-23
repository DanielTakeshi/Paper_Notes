# Curiosity-Drive Exploration by Self-Supervised Prediction

https://danieltakeshi.github.io/2018/03/23/self-supervision-part-1/

I am interested in this paper because it deals with reward types that are sparse
and/or missing. Despite missing/sparse rewards, their proposed agent can still
explore.

**Project webpage**: [link](https://pathak22.github.io/noreward-rl/)

I like the classification of exploration into the categories of (1) encouraging
the agent to explore novel states or (2) encouraging the agent to perform
actions minimizing uncertainty about the environment. Yes, VIME definitely falls
in the second category. 

This paper belongs to the second category and their insight is transforming the
original feature space (i.e .images) into a lower-dimensional representation,
such that the new feature space represents **only information relevant to the
action from the agent**. I.e. the feature space ignores environmental changes
that aren't affected by the agent's actions.  If this were Breakout for
instance, one example would be the changes in environment that happen when the
ball is in the back and hitting everything without the agent moving at all,
though this might be hard to detect. They don't use Atari here, BTW, they use
*VizDoom* and *Super Mario Bros*.

How do they develop this lower-dimensional feature space? By training the agent
using "self-supervision", the agent is given (s_{t}, s_{t+1}) and must predict
the action the agent took. It's interesting when you think about it ...

See **Figure 2** for the high-level description. It's a great overview.

**Reward formulation**: there is some extrinsic reward (usually zero, though!)
and an intrinsic reward from curiosity, and the total reward the policy needs to
maximize is the sum of those two components. When they run A3C, it's really
running in a new environment, this one with lots of rewards, most of which are
"artificially introduced" but in a clever way.

**Practical implementation tip**: remember that this is an *exploration* policy,
not a full-blown RL algorithm. Their exploration needs to be integrated with an
existing algorithm for getting a "real" policy. They use A3C as their base
algorithm.

**Final thought**: given this work, plus others such as VIME, if we're doing
random exploration in state of the art Deep RL, then something is wrong!
