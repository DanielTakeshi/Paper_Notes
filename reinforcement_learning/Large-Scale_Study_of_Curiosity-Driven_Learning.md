# Large-Scale Study of Curiosity-Driven Learning

See also: https://blog.openai.com/reinforcement-learning-with-prediction-based-rewards/

They list several factors that cause prediction errors, and design RND (see
other paper) to mitigate two of those. The other, the prediction errors (or
novelty), is *useful and desirable*.

This paper presents the "noisy TV problem" as hinted at in the blog post. The
goal is, of course, to predict the next state -- well, features of it -- given
current state and action. It is a bit eerily human-like. :-)

They benchmark:

- 54 standard environments: Atari, etc. With no rewards!! (They bring a good
  point, often extrinsic, explicit rewards correspond well with curiosity.)
- The algorithm from (Pathak et al, 2017), i.e., curiosity-driven exploration,
  where intrinsic reward is based on estimation error of forward dynamics.
- Different feature spaces for the states, meaning `phi(s)` is what we predict,
  not `s` directly.
  
W.r.t. embeddings, they test: (a) identity, for a poor baseline, (b) random
features, works surprisingly well, (c) VAEs, and (d) inverse dynamics features.
It's not clear which is best:

> Since all the features involve some trade-off of desirable properties, it
> becomes an empirical question as to how effective each of them is across
> environments.

though, intuitively, I don't think raw pixels should work well. They report that
random features work surprisingly well, slightly below learned inverse dynamics
features. Pixels and VAEs don't work well.

I like how the paper reports on practical considerations for implementation, and
also about issues with "end-of-life" cases with episodic environments.

I suspect this paper will be widely cited as a reference for what works and
doesn't work in curiosity.
