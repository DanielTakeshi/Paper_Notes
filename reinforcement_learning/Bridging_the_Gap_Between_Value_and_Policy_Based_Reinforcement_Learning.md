# Bridging the Gap Between Value and Policy Based Reinforcement Learning

A new algorithm: **Path Consistency Learning** (PCL).


## Main Ideas and Intuition

They say that:

> PCL iteratively fits both a parameterized policy and a state value function
> (implicitly Q-values), bridging the gap between value and policy based
> methods.

But I'm confused ... this is already what happens in **vanilla policy
gradients**, right? We have a parameterized policy along with the value function
(serving as the baseline, or the critic). What's new here?


## Mathematical Details

TODO


## Experiment Details

They test on a synthetic baseline and a set of six "algorithmic tasks" from
OpenAI gym. This will be interesting; I have not used any of those six tasks
before, preferring to use Atari or Mujoco stuff.


## Thoughts and Takeaways

Note that one of the authors gave a guest lecture on this in [the Berkeley
DeepRL course][1]. In addition, John Canny recommended that I read this paper.
Hence, this summary!

[1]:http://rll.berkeley.edu/deeprlcourse/
