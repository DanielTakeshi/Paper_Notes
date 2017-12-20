# Reverse Curriculum Generation for Reinforcement Learning

**Main idea**: many tasks have sparse rewards, and in the limiting case, we have
a reward of 1 for reaching a goal configuration (e.g., for a robot end-effector)
and 0 otherwise. This paper proposes a solution based on automatically
generating a curriculum of starting states to reach the goal. In the beginning,
the starting states are sampled to be very close to the goal, so that even
random actions can result in some success. Then states are gradually sampled to
be farther away from the goal, hence the curriculum learning aspect.

Other points:

- The initial discussion on rewards reminds me of Hindsight Experience Replay,
  for learning from binary rewards. Reverse curriculum generation appears to be
  an alternative to HER. Could they be combined? HER was with DQN-based
  methods ... and this method uses a replay buffer to periodically save older
  "curriculum"s.

- How does this work mathematically? Use policy gradients, but need to provide a
  good signal, so they enforce a success rate of between 10 and 90 percent, so
  that gradients don't vanish. They can vanish if the baseline equals the reward
  (so the task is too easy) or if the baseline is zero, which means there is no
  point in having a baseline (task is too hard to solve anyway).

- It's not possible to enforce the 10-90 percent constraint exactly (that's what
  I thought) so they use a sophisticated sampling procedure, using *Brownian
  motion*.

- Experimental results are on mazes or on simulated robots, using MuJoCo. The
  code itself is based on `rllab`. Their policy networks are fully connected,
  two hidden layers, 64 units each, and they train with TRPO.
