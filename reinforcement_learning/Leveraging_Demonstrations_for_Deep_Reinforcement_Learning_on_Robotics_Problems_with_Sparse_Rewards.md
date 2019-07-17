# Leveraging Demonstrations for Deep Reinforcement Learning on Robotics Problems with Sparse Rewards

Very similar to DQfD, in that it combines an RL algorithm with demonstrations.
This time, DDPG is used with demos, whereas DQfD combines DQN with demos. The
experiments involve four simulations, then a real robot inserting a clip into
an object. Generally, the tasks are about "insertion" of some sort.

As far as the technical contribution, note the similarities with this one and
the later works by (Nair et al., 2018) and (Matas et al., 2018). All use DDPG
with demos for sparse reward tasks. This paper specifically proposes the
extensions:

- Demonstrations, from humans.

- Prioritized replay to control sampling the demo or agent samples. (But Matas
  said it was not possible to tune the priorities the way DeepMind did it, for
  Matas' task.)

- 1-step and n-step returns.

- Learn multiple times per env step (this means increasing the frequency of
  minibatch updates relative to environment steps, and unfortunately this has
  to be highly tuned)

- L2 regularization (though not sure how major this is ...).

But it does *not* have the (a) behavior cloning loss, (b) "Q-filter" added to
the BC loss, or (c) resetting to demonstrations from (Nair et al., 2018). It
also doesn't have the asymmetric actor-critic or TD3, since those were
presented later. In contrast, (Matas et al., 2018) tried those. *It's unclear
to me if this current paper uses a pre-training stage where it pre-trains on
all the demonstrator states* --- I might just be missing it somewhere?

I think this paper isn't going to be updated further since the (Nair et al.,
2018) seems to have done what this one wanted to do?
