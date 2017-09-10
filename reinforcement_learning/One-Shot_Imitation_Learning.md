# One-Shot Imitation Learning

Note: I read the version from March ... waiting to see if there's a newer
version.

## Quick Thoughts/Observations

This is a bold paper title. Recall that imitation learning (not reinforcement
learning!) is the problem of trying to mimic some expert demonstration. However,
this usually requires multiple runs of the expert. The "one-shot" part must mean
we only need *one* expert trial.  The authors are clear on their overall goal:

> [...] ideally, robots should be able to learn from very few demonstrations of
> any given task, and instantly generalize to new situations of the same task,
> without requiring task-specific engineering.

and

> In this paper, we are interested in policies that are not specific to one
> task, but rather can be told (through a single demonstration) what the current
> new task is, and be successful at this new task.

Their contribution is a meta-learning framework (i.e. learning to learn) that
can achieve this goal. They use soft attention for generalization; for more
information about this, read [this Distill "article"][1]. See also their Figure
1, which is very helpful.

Some of this is slightly misleading. The reason is that they seem to require
more demonstrations to first *get* a reasonable policy that can generalize (call
it a "baseline" or "generic policy"). Once they have that, *then* they can use
just *one* demonstration on a new (but closely related) task and the policy
should generalize. Intuitively, it feels like some form of pre-training. It's
also, as they say, a form of multi-task learning or transfer learning, where
learning for one task should generalize to another. Not surprisingly, they cite
the *Third Person Imitation Learning* paper, among others. However, these prior
works are not good enough for learning using *one* demonstration on whatever new
task is being considered. That's their contribution.

> Our learned policy takes as input: (i) the current observation, and (ii) one
> demonstration that successfully solves a different instance of the same task
> (this demonstration is fixed for the duration of the episode). The policy
> outputs the current controls.

See also [this blog post][2].

They mention later in the paper that their **architecture for block-stacking**
is another contribution because they think this will be useful for future tasks.

- The architecture has three parts: the *demonstration* network, the *context*
  network, and the *manipulation* network. 

  - **Demonstration**: this takes the entire demonstration as input and outputs
    some embedding. This demonstration can be long, so they need to subsample
    for timesteps. I've faced this issue (over whether to subsample or not) with
    1000-length trajectories in MuJoCo inputted to an LSTM layer.

  - **Context**: this takes the embedding from the demonstration, *and* the
    current state, and produces a context embedding. I get it, the demonstration
    embedding is going to be fixed, so the input at time t is the concatenated
    vector (d, x_t).

  - **Manipulation**: given that embedding from the context network, figure out
    the action to take. This is the simplest one.

  PS: this description is so much clearer now that I have more intuition on
  how nets can be combined, e.g. as in VAEs. And it *does* make sense to split
  the computation into smaller, more easily-understood network components. This
  is trained entirely end-to-end, right?
  
- They also use a novel **neighborhood attention operation** for the first two
  of these three networks.  They don't directly rely on prior work because of
  issues with varying-sized input *and* output (classical attention models deal
  with *fixed output*). The intuition is that we need one attention mechanism
  for *each* block here. Unfortunately, it's still quite confusing to me.

For the easier, particle-reaching task:

- To be clear, the input is **both** a full demonstration and the current state,
  at each time step. At time step t, the input is (D, x_t). So D must remain
  constant throughout. And they can use, e.g., an LSTM so that D turns into
  some fixed vector d, which is then concatenated with x_t, so (d, x_t) is the
  ultimate vector fed into the fully connected parts. On the other hand, perhaps
  there's a way to get d_t, to get the D -> LSTM -> output to **vary** among
  time steps. I'm not sure. They increasingly vary the architecture to make it
  more specialized to this task, so there is definitely going to be a tradeoff
  here.

Now, how are these trained, anyway? Imitation learning, I suppose? This has to
be done offline, right??


## Experiments

Their experiments are on **particle reaching** and **block stacking**. Both of
these are *families* of tasks, so it's possible to define an infinite number of
tasks which fall into one of the two categories. For particle reaching, the
robot isn't even a real robot; it's a simulated 2-D coordinate (!!) which can
move around in its "robot space." This is as simple as one can get, because to
understand the task, we need to see one demonstration. Their block stacking is
with a real robot **(I think, it might actually be pure simulation of a real
robot, I don't know)** and tasks are specified with a string, which is
convenient to describe. The blocks each have their own letter, FYI, which is
what makes this work.

For training the original, "baseline" policy they use imitation learning
algorithms (this is not the one-shot part). I see, they also augment the expert
trajectories with noise. A basic, but intuitively appealing idea for
generalization.

I'm not sure why they need to condition the policy on a randomly chosen
demonstration, but it seems important for their paper so I will look at this
again.

I didn't get to finish reading their network description, but I understand parts
of it. Their context network is what helps the robot to focus on only the
relevant blocks at a time. The manipulation network gets the context embedding
(output from the context network) and determines the actions.

Figure 10 shows that performance is equivalent among these tasks. These tasks
are all the same up to permutation! I.e. stacking four blocks in any order.
I think Figure 10 involves 100 demonstrations *for each task*, so this is 24*100
total demonstrations!


[1]:http://distill.pub/2016/augmented-rnns/
[2]:https://blog.openai.com/robots-that-learn/
