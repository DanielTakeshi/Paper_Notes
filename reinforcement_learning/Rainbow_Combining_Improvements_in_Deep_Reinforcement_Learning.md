# Rainbow: Combining Improvements in Deep Reinforcement Learning

I knew a paper like this was about to come out of DeepMind, due to their
computational resources and familiarity with their algorithms. 

**Main idea of the paper**: succinctly described in the paragraph:

> In this paper we propose to study an agent that combines all the afore-
> mentioned ingredients. We show how these different ideas can be integrated,
> and that they are indeed largely complementary. In fact, their combination
> results in new state- of-the-art results on the benchmark suite of 57 Atari
> 2600 games from the Arcade Learning Environment (Bellemare et al. 2013), both
> in terms of data efficiency and of final performance.

The combinations they test:

- Double DQN
- Prioritized Replay
- Dueling Architectures
- A3C (or "multi-step" DQN)
- Distributional DQN
- Noisy DQN

Actually I didn't know there was a "Noisy DQN", I better check that out; seems
similar to OpenAI's parameter-space noise for exploration. Also, the A3C is
DQN-based, not the actor-critic, I assume, as it's multi-step DQN (or NDQN?).

So now this Rainbow is the new state of the art on Atari games. Good to know,
since I was wondering about that ...

Ablation studies remove one component each and test. That makes sense. Rather
surprisingly, from my perspective, it doesn't seem to hurt badly if you remove
the "Double DQN" part, but that may be because other components help to overact
the Q-learning bias which motivated Double DQN.

There are a few technicalities on how to combine all of these improvements, as
it's not as straightforward as the DQN -> DDQN extension.

It's also not really a hierarchical algorithm as there isn't a master policy
which is picking among options. It's a combination of prior techniques.

**Takeaway**: I read this mostly to see (a) what improvements are most
important, (b) how to do proper ablation studies, and (c) what are the limits
with DeepRL. :-) The other thing to note is whether certain improvements have
negative effects when combined with each other.

Last comment: Rainbow seems to choke on Breakout relative to some of the other
games. It's really hard to get consistent performance across all the games ...
