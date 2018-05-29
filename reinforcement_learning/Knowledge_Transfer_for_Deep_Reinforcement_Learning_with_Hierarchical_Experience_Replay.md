# Knowledge Transfer for Deep Reinforcement Learning with Hierarchical Experience Replay

Two main direct contributions over the policy distillation paper:

- A different multi-task architecture, with task-specific convolutional layers
  but shared fully connected layers.
- A hierarchical prioritized replay so that the student can obtain the most
  relevant teacher samples. (This is something that I thought of immediately
  after reading Policy Distillation.)

The first contribution seems like it would *increase* training time, but the
authors argue this should decrease it. Hmm ... well, in Policy Distillation,
with multiple games distilled into one agent, the only thing that was optimized
on a per-game basis was the final fully connected layer, and I think all this
means is to optimize *additional* layers for task-specific settings. Not too
much of a contribution. I'm confused about why the fully connected layers are
shared *after* the convolutional ones. Don't different games have different
action spaces? Or are they assuming they all share the same 18-D action space?

The hierarchical replay is more interesting, they split up the state space into
groups based on the value function as estimated by a teacher. Then they
prioritize within those based on the KL divergence gradient (since that's what
the student optimizes, *not* the Temporal Difference error). They argue the
first grouping among the value functions is necessary so that the student agent
will get a sufficiently uniform set of samples in the state space. Seems ...
plausible, not totally sure if the value function is what one should use but
it's applicable everywhere.

Indeed they seem to get some benefit for their replay on Breakout and Qbert, not
so much on games like Pong that are easy but that's expected. Results are a bit
limited, though.
