# Multi-task Deep Reinforcement Learning with PopArt

New algorithm: **PopArt**, which adaptively rescales the targets for the value
network to have zero mean and unit variance. Remember this! It will likely be a
popular algorithm that DeepMind pushes, like Rainbow. [Brief blog post here][1].
(PopArt is for multitask learning, whereas Rainbow is for combining various
algorithms together, but the agent is applied to tasks individually.)

Main advertisement / excitement from the paper:

> Excitingly, our method learned a single trained policy -- with a single set of
> weights -- that exceeds median human performance. To our knowledge, this was
> the first time a single agent surpassed human-level performance on this
> multi-task domain.

Aspects of the algorithm:

- Applies PopArt Normalization to IMPALA. For PopArt background, see the NIPS
  2016 paper "Learning Values Across Many Orders of Magnitude". That paper was
  applied to *value-based* methods, here they show it applied to *actor-critic*
  methods.

- The algorithm involves frequent normalization of terms, reminds me of batch
  normalization. Have separate normalization statistics for each task. If we're
  dealing with Atari, for instance, we'd have 57 different tasks (games) since
  that's how many are in DeepMind's suite.

- Return just a SINGLE policy that we should apply to all Atari games (and I
  suppose another agent for all the DeepMind lab stuff). We're not even allowed
  to condition on knowing the task beforehand, so no separate 'branches/heads'
  in the neural network.

Thoughts:

- Similar goals as the Ape-X DQfD paper regarding reward invariance, but the
  algorithms are a bit incompatible (see the Ape-X DQfD paper for details).

- Since it uses IMPALA, I'd like to revisit that paper and read others, and
  learn more about what aspects of algorithms (or general 'frameworks' --- A3C
  is probably better thought of as a framework) can be combined with each other.
  Also: IMPALA is open source, so I should try that if possible. Probably a nice
  blog post that summarizes what can and cannot be used with IMPALA would work?
  Don't forget to understand v-trace normalization.


[1]:https://deepmind.com/blog/preserving-outputs-precisely-while-adaptively-rescaling-targets/
