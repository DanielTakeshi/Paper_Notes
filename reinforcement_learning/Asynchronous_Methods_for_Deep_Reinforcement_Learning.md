# Asynchronous Methods for Deep Reinforcement Learning

General idea: 

Remember that, though the best known aspect of this paper is the **A3C
algorithm**, this paper is really about *four* algorithms, all of which are
asynchronous variants of other algorithms. I view asynchronous as meaning
"parallel" and they argue that by parallelizing learning, they "stabilize"
training. This is easiest to understand in the context of DQN and experience
replay. Asynchronous learning means we *no longer* need to use experience
replay, and thus we can use on-policy methods.


## Introductory Material

Most of this is familiar to me. I have a question, though. When doing n-step
Q-learning, how are we supposed to know the rewards at subsequent time steps?
Because they say r_t + gamma*r_{t+1} + ..., and that implies we know the rewards
after the current (s,a) pair. But if this is online updating, how is this true?
Are updates simply delayed until we get to n steps *after* the current (s,a)
pair?


## The Algorithm

They modify four standard RL algorithms:

- **One-step Q-learning**. This one is easiest to understand. Each thread does
  epsilon-greedy Q-learning (in fact, to maximize diversity, threads can sample
  epsilon!). Each thread accumulates its gradient update to a SHARED target
  network (remember, the separate target network is important for DQN) and
  periodically sends the update using Hogwild! updates. By the way, are we
  assuming that there is also a separate, shared Q-network? In DQN we need two
  networks; **I assume both are shared**? Yeah, because both \theta and
  \theta^{-} are "shared" in Algorithm 1.

- **One-step SARSA**. Same as the Q-learning except with a different target,
  makes sense.

- **N-step Q-learning**. OK, this explains my question about how n-step
  Q-learning works. At least I'm thinking about the right things! See this:
  
  > Each n-step update uses the longest possible n-step return resulting in a
  > one-step update for the last state, a two-step update for the second last
  > state, and so on for a total of up to t_max updates.
  
  I'm not following why they need "thread-specific parameters" here, though.
  This isn't in the one-step Q-learning pseudocode.

- **Actor-critic**. Unfortunately I probably have the least knowledge on how
  this works, and this is their best algorithm! It seems like a policy gradient
  based method since the gradient is updating the parameters of the policy
  directly, and they're using advantage functions. I still don't know what
  **actor-critic** means. It looks like they're also putting in some n-step
  learning in the pseudocode. Why?? It looks just like vanilla policy gradients?
  And I still don't know what they mean by thread-specific parameters.

They apply two major changes to all of them.

- **Asynchronous updates**. What does this mean? This means running multiple
  instances of the environment *in parallel*, but using *threads on a single
  CPU*, not multiple machines. To make parameter updates, do Hogwild! updates,
  which they already know works well. I know Hogwild! Basically, each thread has
  gradient updates and that's like the minibatch version. With parallelization,
  we have to be careful about threads, deadlocks, etc., but Hogwild! showed that
  you can still make gradient updates in parallel without worry, since in the
  end things still converge.

- **No experience replay**. (Actually, this is the same category as in
  asynchronous updates, so they didn't need to split the discussion in two.
  Basically, asynchronous updates provide these two benefits here, running in
  threads and avoiding the need for experience replay.)


## Experiments

I appreciate their exploration of momentum SGD and RMSProp. They had to do some
more exploration of the latter due to the parallel setting. I get it, I'll just
trust their result and won't investigate further.


## My Thoughts and Takeaways

At the time it was published (ICML 2016, though it was on arXiv for a few months
earlier), I believe it had the best results on the Atari 2600 games. It's a bit
tough to say because that requires going through the whole set of games and
eyeballing the numbers but I trust the paper's claims. And don't forget, they
don't *just* benchmark on Atari games. They benchmark on Mujoco, as usual, but
also on a new 3D maze task; there's been a lot of future work which also uses
that maze task (Labyrinth).

This is one of my favorite papers. I find it hard to read research papers, but
this one was actually not that bad for me, especially the 8-page
non-supplementary material content. I can understand and follow the n-step
methods and SARSA. It also doesn't seem hard to replicate a simpler version of
asynchronous methods for DQN which we created for the CS 294-129 class at
Berkeley. It's not truly parallel but it simulates that by running different
game environments sequentially (*if* it were easy to write parallel code we
would just do that). The harder part might be A3C itself, though.

I like the emphasis on using a CPU instead of a GPU and training in half the
time, since it makes their algorithm more accessible.

Great work!! I want to know how to implement these someday.
