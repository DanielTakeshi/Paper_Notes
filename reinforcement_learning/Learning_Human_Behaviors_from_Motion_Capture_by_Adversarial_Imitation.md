# Learning Human Behaviors from Motion Capture by Adversarial Imitation 

This seems to be a follow-up paper to "Robust Imitation of Diverse Behaviors",
which will appear at NIPS 2017. This one ("Learning Human Behaviors ...") is an
arXiv preprint, so I wonder where it will eventually appear. They argue in the
beginning that:

> Yet current methods for humanoid control fall short of our desires. Methods
> that rely on pure reinforcement learning (RL) objectives tend to produce
> insufficiently humanlike and overly stereotyped movement behaviors.

This is intuitive but not yet "scientific".  I assume their video abstract will
explain a lot. They argue that as an alternative, we can produce human-like
behaviors in agents via imitation learning from human data using *motion
capture*. Indeed, imitation learning is probably better than RL if we've got a
complicated task but lots of demonstrations, since that's how a human would
learn.

They present a method to determine **low-level** controls from motion capture
(this is where the imitation and the GAIL stuff happen), then they train a
**high-level** controller *on* these low-level controls via reinforcement
learning, so it reminds me of hierarchical reinforcement learning.

They say that using the `-log(1-D)` reward for the Generator is better than the
log(D) value. But I don't see any numbers?!? Also, they perform M Discriminator
updates for every single Generator update... I just don't know if these tricks
generalize. Otherwise, they use the same GAIL algorithm.

Experiments:

- They argue that the Discriminator doesn't need the actions. But I don't see
  any results with the MuJoCo environments from the GAIL paper (maybe the hopper
  but I'm not sure ...). It would be better to show that, in my opinion.

- They additionally show that imitation learning can work even with partial
  observability, but that was in part because the data they have is noisy (I
  think).

- I think the motion capture part is cool but it makes it really hard to see how
  to generalize this, etc.
