# Benchmarking Deep Reinforcement Learning for Continuous Control

This is going to be a fairly short summary compared to my others, as there isn't
much technical detail. Most of the contribution is due to an [rllab repository on
GitHub](https://github.com/openai/rllab) which I'll try to use now.

*Remark*: be sure to read this on a PDF, where Table 1 can be rotated for easier
reading.


## Main Ideas

The key challenge they observe:

> [...] it has been difficult to quantify progress in the domain of continuous
> control due to the lack of a commonly adopted benchmark.

By "continuous control" we mean tasks with actions that are continuous. With the
Atar 2600 games, the actions could be LEFT, RIGHT, etc., but these are NOT
continuous actions, which would be like controlling a swimmer and with actions
encoded as a 3-D vector indicating the forces we're applying in certain
directions.

In fact, the paper cites other references saying that naive Q-learning on
continuous control (where we discretize the action space) is intractable due to
the curse of dimensionality.

This work presents a GitHub repository which provides a suite of 31 continuous
control benchmarks for deep reinforcement learning so that we can more
accurately measure progress in the field. There should be more today, since it's
been a year.

The benchmarks are divided into four categories:

- Basic 
- Locomotion
- Partially observable
- Hierarchical

They use *physics simulators*. Yeah, that's about as good as we can do,
probably.


## Results

TNPG and TRPO do a very good job, apparently.

It seems like the majority of TRPO algorithms I see reported in experimental
papers use rllab's implementation, so I will do the same. As of this writing,
however, there is no DQN implemented there.

Their conclusion is that:

> Results show that among the implemented algorithms, TNPG, TRPO, and DDPG are
> effective methods for training deep neural network policies. Still, the poor
> performance on the proposed hierarchical tasks calls for new algorithms to be
> developed.

This must be why people are working so hard on algorithms for hierarchical
tasks.


## My Thoughts and Takeaways

This is an unusual paper for ICML and I'm surprised it got in. That's not to say
it's bad; it's mostly because academic reviewers tend to be conservative. By
that, I mean conservative in academic ideas, not Donald Trump-style
conservative. Hmmm ... maybe there are a few closeted ones? =)
