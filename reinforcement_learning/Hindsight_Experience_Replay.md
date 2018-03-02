# Hindsight Experience Replay

See: https://danieltakeshi.github.io/2018/02/28/sample-efficient-rl/

**What problem are they trying to solve?**

Issues with normal/default model-free (deep)RL:

- Sample efficiency is bad.
- Problems with learning from sparse rewards (e.g., binary reward indicating
  task completion).
- It's possible to engineer and change reward functions, but this is not always
  desirable, possible, or efficient.

Their solution is **Hindsight Experience Replay**:

- Key idea: replay episodes with a different goal.
- Constraints: need multiple goals in an environment.
- Modularity: it can be combined with off-policy RL algorithms, so it doesn't
  replace them but can augment them.


**How does it work**

Idea: say we have a trajectory that went from states `s1` to `sT` but did not
achieve the goal `g` anywhere. Then we can put this trajectory in the replay
buffer **with `sT` as the goal, not `g`** (though we also put in `g` as well),
so that half the time, we have some reward to learn from. 

- The *policy* and *value functions* which are set such that they take in as
  input `(s,g)` pairs, a state *and* the goal.

- Simplest version: add to replay buffer the goal achieved in the final state of
  the episode (in addition to the original goal which wasn't achieved). This
  works because the dynamics aren't affected when changing the goal, agreed, and
  I also see how the set of goals added to replay can be customized.  *Fine
  print*: they assume they've got a function `m` which handles the mapping from
  state to goal, see Section 3.2 for examples.

See Algorithm 1 for details. Surprisingly, a simple but great idea, which is the
best kind of research! Does it make sense that it would work? I think so, due to
the denser set of rewards, though in some sense it's surprising that effectively
"random" goals would be good (random goals because those goals are sampled at
the end of the set of random trajectories at the beginning due to a random
policy). **I wonder how much of a constraint their "mapping" is**, i.e., that
thing which maps the ending state to a goal in the set of goals (in settings not
seen in the paper).


**Experiments**

Motivating example: having a state space consisting of an n-dimensional binary
vector, and action i flips the bit in the i-th component. The state and target
are randomly initialized. Yes, I can see how standard RL algorithms fail here.
The standard solution here is reward shaping, but the authors don't want to do
that as reward shaping is not always feasible.

All their (non-"trivial") experiments are with robotic arms, either simulations
or (after simulation) real-world experiments. They use a Fetch 7 DoF arm,
standard for OpenAI, with no rotation needed for their tasks (this makes the
problem easier). Tasks: pushing, sliding, pick-and-place.

- DDPG with and without HER: wow, all three tasks, the HER versions are clearly
  superior to raw DDPG. They also did DQN but basically that does as well as
  DDPG, awful.

- HER with single-goal setup: it still works, but it's faster if there are
  multiple goals.

- Shaped reward functions: rather surprisingly, it doesn't work for their chosen
  shaped function. They'd probably need it to be very complex, another benefit
  for HER. Heh, they read my mind:

  > Of course for every problem there exists a reward which makes it easy (Ng et
  > al., 1999) but designing such shaped rewards requires a lot of domain
  > knowledge and may in some cases not be much easier than directly scripting
  > the policy.

- Sampling goals for HER: one key takeaway is that you don't want *too* many
  "artificial" goals (i.e., goals that were not the original goal) coming in the
  replay.

- Physical robot: [see video][1].

A nice, thorough set of experiments.

[1]:https://www.youtube.com/watch?v=Dz_HuzgMxzo
