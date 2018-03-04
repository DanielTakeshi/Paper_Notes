# Learning Complex Dexterous Manipulation with Deep Reinforcement Learning and Demonstrations

This paper shows that you can solve complicated, manipulation tasks (using a
five-fingered, dexterous robotic hand) using standard model-free RL methods, and
that the training time can be accelerated with demonstrations.

To accelerate with demos, (1) use BC on demos to init (on-)policy method, (2)
then do RL, but we should try and use the variation in demos to inform us
which dimensions are most important to explore. 

Note: they don't use the combined DDPG loss (since they're not using DDPG), the
Q-filter, or demos as done in (Nair et al, 2018). But they do have some flavor
of the combined DDPG loss. They're using **Natural Policy Gradient (NPG)** and
they augment the loss with an extra term that weighs the demo data. Yeah, it's
actually similar to Nair's DDPG loss, except it feels a bit more unstable to me,
because the authors here are relying on a heuristic:

> This is motivated by the premise that initially the actions suggested by the
> demonstrations are at least as good as the actions produced by the policy.
> However, towards the end when the policy is comparable in performance to the
> demonstrations, we do not wish to bias the gradient. Thus, we asymptotically
> decay the augmented objective.

The Q-filter seems better since we can actually explicitly measure `Q(s,a)`.

Experiments show that their method can indeed result in dexterous manipulation
of a simulated robotic hand.

That being said, there are a few weaknesses in this paper. The conclusion that
adding demonstrations can cut down training time in RL should be clear/obvious
and initializing with BC is standard practice if we have demos.  They also
suggest that this can be transferred to the real world, but there's no evidence
of real robotics experiments --- the paper's experiments are entirely simulated
robots, and their "human-like" qualitative evaluation might not be sufficient or
generalizable. The paper's main contribution is to advertise that model-free RL
can be used to solve complicated tasks, but it might not be entirely clear that
the jump from MuJoCo envs to the hand environments is significant enough to
warrant distinction, or if this is the right tool to solve the manipulation
task. Finally, the reward functions are heavily engineered to the task.

The lesson I've learned from read this paper, (Nair et al 2018), and other
similar work is that using demos to accelerate RL is important, but one
shouldn't just use the demos for initialization and ignore them afterwards
during the RL stage (or whatever happens next).
