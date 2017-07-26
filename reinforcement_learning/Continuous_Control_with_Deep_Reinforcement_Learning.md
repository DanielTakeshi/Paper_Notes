# Continuous Control with Deep Reinforcement Learning

TL;DR: This is the **Deep Deterministic Policy Gradients** paper, and it's
probably one of my "The Nine Deep *Reinforcement* Learning Papers You Need to
Know About" if I may shamelessly borrow from Adit Deshpande's blog ... the other
thing to know (sorry) is that it is an extension of **Deterministic Policy
Gradients**.

Other high-level comments: The title of this paper indicates that we're doing
Deep RL using some neural networks, except that the *actions* here are
continuous valued, so they may be 7-dimensional vectors, each of which is
continuous and perhaps bounded in [-1,1]. However, this has surely been done
before so the main task now is to understand the *novelty* in this work versus
prior work. Yeah, it seemed like this was done with Deterministic Policy
Gradients, except they show how to massively scale it up with deep learning.
Indeed, they say:

> Our contribution here is to provide modifications to DPG, inspired by the
> success of DQN, which allow it to use neural network function approximators to
> learn in large state and action spaces online.

PS: this is an **actor-critic** method with the actor playing the policy
function and the critic being the Q(s,a) values learned via the Bellman error.
The policy is denoted as \mu instead of \pi, I suppose for extra emphasis on
having deterministic policies.

Their contributions can be thought of as extensions which are the following:

- Using an experience replay buffer, literally exactly the same way as in DQN.

- Using the target Q-network, except the **updates are soft** so intuitively the
  target Q-network's weights change slowly instead of jumping hard among target
  updates but then staying still for maybe 50k steps (at least, I think that was
  the interval I used for the CS 294-112 assignments). This seems to improve
  stability.

- Using **batch normalization** to handle the fact that in some domains,
  different components in the action vector represent different units.

- Note also that for **exploration**, they inject Gaussian noise into their
  actions. But that's something we *already* do with normal policy gradients!
  However, they don't use IID Gaussian noise but a sophisticated
  Ornstein-Uhlenbeck process to "suit the environment." Don't forget that for
  test-time evaluation, we should not apply this extra noise.

**My thoughts**: so ... this can be thought of as a hybrid between DQN and PG?
What exactly distinguishes the "deterministic" PG from "normal" PG? Also, this
paper doesn't really introduce any *new* concepts, but I think their method
really *works well*, maybe that's why this paper became popular?
