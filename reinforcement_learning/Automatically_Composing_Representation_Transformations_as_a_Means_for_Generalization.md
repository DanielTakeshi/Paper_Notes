# Automatically Composing Representation Transformations as a Means for Generalization

I like the high-level discussion. Reads like a good set of notes or a blog post!

> How can we build a learner that can capture the essence of what makes a hard
> problem more complex than a simple one, break the hard problem along
> characteristic lines into smaller problems it knows how to solve, and
> sequentially solve the smaller problems until the larger one is solved?

The experiments are done on *multilingual arithmetic problems*. Huh.

They recast it as a "new" type of MDP, and form a **Compositional Recursive
Learner**. I can see why it's different from the 'standard' MDP and RL
formulations, especially because they argue that (a) transition dynamics are
nonstationary, and (b) the state space can change in size. Still, their policies
are trained with PPO and the Adam optimizer, so at least that isn't changing, as
in we are *still* using PPO-like algorithms.  :-) 

The question I have, and which I do not know enough to answer, is whether this
is the right tool to be solving these recursive structures (or, if it *is* the
right tool, is the problem simply too easy?).

They also argue that their paper is different from the "learning to learn"
algorithms by Schmidhuber, Finn (but they don't cite MAML?!?), etc., because
they are *not* trying to learn new things faster, but trying to *learn to
reason*, if that makes sense.
