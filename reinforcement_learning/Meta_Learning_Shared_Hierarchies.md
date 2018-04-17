# Meta Learning Shared Hierarchies

Update: re-read the paper, makes more sense now.

Quick highlights:

- Develop a new algorithm, MLSH, which develops a master policy that switches
  between several lower-level policies ("primitives"). Each primitive executes
  for some steps, then the master picks (usually) a different policy. And so on.
  It's easiest to think of these (the master and the `K` primitives) as separate
  neural networks, though there are obviously many ways to share parameters. Be
  creative!

- Advantage: the master policy is dealing with time horizons that are `1/N`
  times the full length trajectory `N`, i.e., its "learning problem" is much
  shorter, which makes everything easier.

- Their novelty is that the master policy keeps getting reset for each task from
  their distribution, and that they argue that a good hierarchy is one with good
  primitives, which means those enable fast fine-tuning for a master policy.

- Algorithm resets the master (`\theta`) then warms-up `\theta`, then jointly
  trains `\theta` along with primitives `\psi`. 

- The primitives are held fixed during test-time. Thus, it seems like `\theta`
  trained via normal RL during test-time? UPDATE: had some older stuff here, but
  a bit irrelevant. It's clear now and I think the theta is just fine-tuned
  normally, because the skills have to be fixed.

- Primitives are (ideally) either directional changes, like up, down, left, and
  right, or they could be walk vs crawl. I say "ideally" because these are
  learned automatically, so there's no guarantee that they turn out to be what
  we want!

- They say:

  > While the prior work on metalearning optimizes to learn as much as possible
  > in a small number of gradient updates, MLSH (our method) optimizes to learn
  > quickly over a large number of policy gradient updates in the RL setting -—-
  > a regime not yet explored by prior work.

The major problem/limitation with this seems to be that we assume a task can be
drawn from a distribution `T \sim Prob_T` and that these sampled tasks are
related and can share motor primitives. (Their evaluation metric is how much
reward an agent can get in the first `T` time steps in a new environment.)

UPDATE: well, actually, this isn't the main weakness since many settings (e.g.,
mazes) can be built this way and it's hard even for humans if the task
distribution is very "wide." A weakness could be the hand-crafted
hyperparameters and that there aren't that many skills tested (at most three, I
think).

I'm still having trouble seeing how this differs from the options framework.
Maybe the options framework doesn't assume we sample tasks from a distribution?
But the options framework still has the same notion of a master policy selecting
a sub-policy to execute for some time steps, before moving on to the next one.

Think: why does this work, and could I implement this? UPDATE: yes it's clear
now.
