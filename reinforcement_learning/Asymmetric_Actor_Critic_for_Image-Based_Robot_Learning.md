# Asymmetric Actor Critic for Image-Based Robot Learning

(This blog post isn't entirely about the paper, but it's close enough.)
https://openai.com/blog/generalizing-from-simulation/

I like this paper. Suppose we are running DDPG (an actor-critic algorithm) in
the context of some image-based RL task. Using images means we can more easily
apply this in real world settings with robot cameras. But, the actor and critic
in the standard DDPG definition would need to take the high dimensional images
as input. What about instead *passing the low dimensional state information to
the critic*? After all, **the critic is not going to be used after training ---
we just need the actor itself, which is trained on images**. And having the
critic trained on lower dimensional state information *can accelerate the
training process*.

There are other aspects of the paper:

- Adding another loss to the actor so that it has to predict the lower
  dimensional state information as part of a "bottleneck" layer.

- Lots of domain randomization on the images to transfer policies to the real
  world.

- Using DDPG with Hindsight Experience Replay and goal-based RL, so both actors
  and critics get a goal in addition to the current state or observation
  (actor=observation, critic=state). This is not a contribution of this paper
  per se but it's an important part of how the algorithm works.

But the main idea is, again, to pass lower dimensional state information into
the critic.

Their experiments are convincing, though I wonder if a better baseline would be
DDPG with demonstrations, like a lot of concurrent and subsequent work has
done. Well, they can't test everything, I guess.

Though, this method presupposes that we have a low dimensional state
representation. In these tasks, they do, but other tasks are not so easily
condensed into low dimensional information, and arguably maybe images actually
have a better "inductive bias" compared to the true states.
