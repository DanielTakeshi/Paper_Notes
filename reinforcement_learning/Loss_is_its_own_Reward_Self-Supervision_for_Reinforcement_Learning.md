# Loss is its own Reward: Self-Supervision for Reinforcement Learning

This paper is relevant for the following reasons:

- It tries to improve the sample efficiency problem in reinforcement learning, a
  very hot area of research.

- It tries to mitigate the problem of sparse rewards. They rightfully
  acknowledge that learning still happens with sparse rewards, but it may not be
  fast enough (related to the first point here).

- They modify the loss function (or rather, *augment it*) which a lot of people
  do nowadays. I know DeepMind did this, but I actually haven't read their paper
  yet. =(

Their solution involves **augmenting the loss function**, **pre-training** and
**joint supervision**. None of these sound novel, though, so let's see what they
have to say. Key quote:

> We argue that representation learning is a bottleneck in current approaches
> bound by reward. Our self-supervised auxiliary losses broaden the horizons of
> reinforcement learning agents to learn from all experience, whether rewarded
> or not.

My opinion: it would help if in Atari games, we didn't always *need* a reward to
learn.  However, when *I* play, I get a lot of "learning" done based on the
reward, so I'm a little nervous about this, even if I also think, conceptually,
their idea is intriguing. They're interested in *representation*, which in
English, means how we encode and communicate "good-ness" and "bad-ness" to an
agent. The example they use of a "decapitated agent" provides supporting
evidence for the importance of representation learning.

Another interesting quote when they list the stuff they benchmark against:

> Since self-supervised and unsupervised methods can make use of unannotated
> data, as auxiliary losses for RL they promise to mine more from the data
> already available to the policy.

**Main goal**: to show that taking reward-less steps in an environment do not
have to be as bad as is currently thought.

**Minor complaints**: I'm not fully aware of self-supervised learning so I got a
little lost following their explanation for the different stuff they're
testing/benchmarking (see Section 3). I also can't analyze Figure 3 very well. I
don't know why people have an aversion to increasing the `lw` parameter in
matplotlib.

I'd like to look at their data efficiency curves again when I have more time, as
this might be the way that people analyze "efficiency" in RL.

Takeaways:

- I need to understand self-supervision better, and how agents can annotate
  unlabeled data.

- I also want to know what work has been done in transfer learning and
  pre-training in the RL domain. They're prevalent in computer vision, but when
  did people start using them for RL? I'm curious.

- I need to see a precise definition and precise example of using an auxiliary
  loss, with clear mathematical equations.

Side note: this was originally an ICLR 2017 workshop paper. [See the reviews
here][1].  However, the paper seems like it's gone through lots of changes so I
wouldn't put too much stock into those reviews.

[1]:https://openreview.net/forum?id=S15PPJStl&noteId=S15PPJStl
