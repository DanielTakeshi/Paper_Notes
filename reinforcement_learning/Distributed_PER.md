# Distributed Prioritized Experience Replay

AKA, the "**Ape-X**" paper.

This is an empirical paper that falls under the theme of 'distributed' and/or
'parallelism' to improve reinforcement learning, which is getting to be all the
rage. But, I will have to see how distinct this work is from A3C which already
does parallelism --- except it *doesn't* use an experience replay. And what
about ACER which already uses experience replay? We need to have a taxonomy of
algorithms somewhere.

After reading a bit, I think the difference between this and A3C wrt data
generation is that here, data generation is done in a distributed fashion across
different CPUs, whereas A3C uses a single CPU but with different threads
(*within* that CPU, presumably).  How much of a difference does this make wrt
latency and other factors?

The paper has some nice clarification of shared experiences vs shared gradients,
and the advantages of the former --- that they become 'outdated' much more
slowly. [And they answer a question I originally had about why we initialize
priorities to be the maximum priority possible in normal PER, see the OpenAI
baselines code for details...]

The algorithm is relatively easy to understand and they provide nice intuition
in Figure 1 and Algorithms 1-2 for the actor and learner, respectively.
Unsurprisingly, they use *hundreds* of CPUs for the actors, but surprisingly,
only a single GPU learner.

How to extend to DQN and DDPG:

- **DQN**: Use the standard n-step returns with bootstrapped estimates for the
  targets (and truncated if episode ends in fewer than n steps, obviously). For
  actors, just use epsilon-greedy policies for each, potentially with different
  epsilon values.

- **DDPG**: See appendix D, looks pretty straightforward. They got rid of the
  annoying Ornstein-Uhlenbeck process in favor of a Gaussian. :-)

And note, in future work they *also* extend DQfD (learning from demonstrations)
with this work. Lots of stuff going on here! As you can see, the nice thing
about developing 'frameworks' is that they are broadly applicable. But, it
raises the question over how people can train these things without the deep
pockets of Google DeepMind.

PS: this algorithm is actually *better* than Rainbow ... which is amazing,
especially since Rainbow came out a few months earlier. So, this is the new
state of the art on Atari, at least at the time of publication.  I think it went
DQN -> DDQN -> Prioritized DQN -> Dueling or A3C? -> Dueling+Prioritized? ->
Rainbow -> Distributed PER? Is that the order of 'state of the art' on Atari? Am
I missing anything?
