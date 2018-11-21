# Exploration by Random Network Distillation

See also: https://blog.openai.com/reinforcement-learning-with-prediction-based-rewards/

One of the key advertising features of the method/paper is that:

> In particular we establish state of the art performance on Montezuma’s
> Revenge, a game famously difficult for deep reinforcement learning methods. To
> the best of our knowledge, this is the first method that achieves better than
> average human performance on this game without using demonstrations or having
> access to the underlying state of the game, and occasionally completes the
> first level.

So, no demonstrations, or underlying states. (I think there were also some
papers that might have used language to help with Montezuma, but again, we're
only dealing with the raw pixels here, so I think this is a fair benchmark.)
But, this already piques my curiosity. :-)

The exploration bonus is based on predictive error. Can the agent, given current
observation and action, predict the next observation? The downside of this
method, of course, is agents are attracted to highly stochastic states, such as
TV static. If an agent gets there, it will stay there because it cannot predict
random TV static. (See the "noisy TV problem" in OpenAI's blog post above.) To
get around that, they use *prediction based on a fixed, randomly initialized
network*, so the answer to the prediction problem is deterministic, not
stochastic.

The other difficulty with predictive error is predicting raw pixels if those are
the observations, but that can be mitigated by predicting lower-dimensional
features from a neural network.

The exploration is built into a modified version of PPO. Note that one of their
other, secondary contributions, is to have separate value heads for extrinsic
rewards (episodic, from normal environment) and intrinsic rewards (from
exploration bonus). See Section 2.3 discussion, seems reasonable, particularly
due to how the extrinsic reward is "episodic" yet prediction reward is
"non-episodic." Though, they say in their ablations that the separate value
heads did not provide much benefit! Did they try the separate value heads for
their experiments and then retroactively do the ablations?

Qualitative results: lots of dancing around with skulls and dangerous creatures.
Heh, funny.

Overall, it's a very exciting result! The recent trend in RL is to use multiple
machines and parallelism for scalability (this paper uses 128 parallel envs),
and this method fits the bill for a suitable exploration method. Though, I
suppose this is not easy for teams outside of highly-funded DeepRL labs,
unfortunately. My main concern: how much of the performance on Montezuma's
Revenge depends on their parallelism rather than the actual RND? There's some of
that in Section 3.5 but would this work with just one parallel env? Also what
gets parallelized --- the env stepping, right? Anything else?
