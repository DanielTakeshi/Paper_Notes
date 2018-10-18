# Deep Reinforcement Learning from Human Preferences

https://blog.openai.com/deep-reinforcement-learning-from-human-preferences/
https://deepmind.com/blog/learning-through-human-feedback/

I am thinking about this paper in the context of imitation learning. For many
tasks (e.g., in robotics) we really care about human preferences, and we use
some other stuff (e.g., a reward) as a *proxy*. But what if the reward is not
necessarily reflective of "quality"?

I understand the high-level overview:

- Have a standard policy updated via standard reinforcement learning
- BUT the rewards are not "true" environment rewards, but _predicted_ from
  another deep neural network, the reward predictor.
- The reward predictor is updated via supervised learning, based on human
  preferences among two different trajectories (which were generated via the
  current policy!)

Sounds good! They were able to get this to scale to complicated environments and
with deep neural network policies.

OK, I think I might need some clarification on this:

> One subtlety is that the reward function r may be non-stationary, which leads
> us to prefer methods which are robust to changes in the reward function. This
> led us to focus on policy gradient methods, which have been applied
> successfully for such problems (Ho and Ermon, 2016).

But whatever. They use A2C for Atari games and TRPO for MuJoCo. (I wonder if
they used OpenAI baselines?)

Wait, they say the clips are only 1-2 seconds long? That's impressively short.
(And to be clear, part of the paper's contributions is precisely that we do
_not_ need substantial human intervention. Otherwise it would be too
burdensome.)

The human judgments are stored in a database of triples, each of which is
`(sigma1, sigma2, mu)`, and where `mu` is in the set `{[1,0], [0,1], [0.5,
0.5]}`, so it represents a very simple probability mass function.

I see, they show how to fit the reward function based on assuming the
probability of preferring one trajectory over the other depends on the
exponential of the rewards. See Equation 1 and the associated discussion for
details. It reminds me of "Skill Rating for Generative Models" (arXiv, 2018),
yet I'm surprised the Google-affiliated authors of that paper didn't cite this
one!! Anyway, you can fit the reward function network towards `mu` via cross
entropy minimization. Normally in classification (e.g., of images) our labels
are one-hot, so `[1,0]` or `[0,1]` in the case of two classes. Here, we have the
additional possibility of fitting to `[0.5, 0.5]`.

Another thing: how to decide what to query to the human? They use a simple
approximation: using an ensemble of predictors, we provide the query with
highest variance in reward estimation. It would be better, as they say, to
figure out if there's a way to know what query would be most informative in
information theoretic terms.

Looking at the Atari results, the A3C (more like A2C) they use gets essentially
zero points! That might be like what I am experiencing with baselines, showing
that they (probably) used that repository. They attribute it due to the
difficulty of surpassing another car with random exploration.

Hmm ... some of the results may suggest human preferences are good (especially
in Enduro) but in other cases it's not as useful (Breakout). True, their goal is
not to out-perform RL (or even the synthetic query baseline) but instead to get
*nearly* as good. That makes their task substantially easier.

They have a good point, feedback needs to be provided throughout training, and
not just in the beginning (due to the different state distributions), but that
is kind of obvious, isn't it?
