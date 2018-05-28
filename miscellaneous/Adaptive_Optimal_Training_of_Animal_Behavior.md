# Adaptive Optimal Training of Animal Behavior

They model animal behavior, specifically rats, by using a logistic regression
model in a setup where the rat can choose left or right in response to a
stimulus. So the weights that model animal behavior is to construct a linear
model, which may (in fact, probably should) have a history component to it.

The weight changes are modeled with two ways: a random walk prior, and a
reinforcement learning signal.

The random walk uses ... well, a random walk. It's unclear to me why the authors
belabor over the math which I think is a direct application of Bayes' rule.

The reinforcement learning part is justified as:

> The fact that animals show improved performance, as training progresses,
> suggests that we need a non-random component in our model that accounts for
> learning. We will first introduce a simple model of weight change based on the
> ideas from reinforcement learning, then discuss how we can incorporate the
> learning model into our time-varying estimate method.

This makes sense at a high level and the integration with the time-varying
estimate model is as a "drift term" but I wish I understood this better.

They use policy gradients for the RL part (heh, I wonder what Ben Recht has to
say about this) and specifically the "RewardMax" model (isn't this trivial and
is what everyone else uses, maximizing reward??).

OK, now *after* all this stuff, we get to the "AlignMax" algorithm, which
actually brings us to our objective: figuring out the appropriate sequence of
stimuli to get the agent (rat) to some goal (some weight vector). Indeed they
use `w_goal - w_t` and multiply that with the gradient term.

I think I am confused by many aspects of this paper. They make a distinction
between what they do and IRL, since their work assumes non-stationary policies
while IRL assumes stationary (I think?). I wish I could run the code and see for
myself.

The reviews seem mixed, surprisingly, but I think reviewer 5 may have been
enough for the paper to get accepted to NIPS.

https://media.nips.cc/nipsbooks/nipspapers/paper_files/nips29/reviews/1053.html
