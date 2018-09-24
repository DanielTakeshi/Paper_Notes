# Self-Imitation Learning

This works precisely because it's like exploiting states with high TD? So,
actions are much better than expected as judged by the value function critic.
And "replaying" those actions requires that we save these in a special replay
buffer (so again, replay buffers make an appearance).  Since it uses value
functions, you can apply it to any actor-critic architecture, and indeed that's
one of the paper's contribution.

This is "on top of" an actor-critic architecture. So again, continues trend of
new IL/RL algorithm that are really piggy-backed onto existing, "vanilla" ones
which serve as baselines in the experiments.

Interesting, the algorithm involves:

- Running trajectories, saving output 
- Then updating experience replay to use full Monte Carlo return instead of the
  single r_t (why?)
- Then do normal A2C update, this is on-policy.
- Then do the off-policy self-imitation learning after this, focusing on those
  where return is greater than value function.
- No importance sampling needed (why?)

Really, is it that simple?
