# Observe and Look Further: Achieving Consistent Performance on Atari

Two main reasons why I wanted to read this paper:

- It extends the DQfD paper, and I am very interested in ways to bootstrap and
  accelerate RL, and the combination of RL and other stuff like IL.

- Consistent performance: as we know, many (er, all) deep RL algorithms have
  high variance.  I've been frustrated by this. It would be great to see how
  consistent we get. Though, I want to know what 'consistent' means here ...
  consistent enough like the consistency of air travel safety? I hope ...

**What's the main contribution?**

- A new Bellman update.

    - They insert a new function `h` and then apply it to the bellman operator,
      with the same function inverted (`h^-1`) for the Q-value inside the
      update. It's unclear to me how they understood that this would be helpful.
    - They claim this will help deal with different reward scales. So, they do
      not need to clip into [-1,1]. That's one major change.

- The goal is to address these other things:

    - *processing diverse reward distributions*: new Bellman update (see
      previous point).
    - *reasoning over long time horizons*: increase discount factor from 0.99 to
      0.999 but counter potential instability by adding a temporal consistency
      loss.
    - *exploring efficiently*: This is where they use DQfD, but with three major
      changes: don't have separate pretraining phase, use fixed 75-25 split
      agent-expert data (actually that makes it easier to implement) and
      imitation/large-margin loss only on best expert demo.
      
The algorithm is **Ape-X DQfD** because it uses ideas from the Ape-X paper,
titled "Distributed Prioritized Experience Replay" (ICLR 2018). Yeah, more
distributed systems!! I am seeing a clear trend.

**What do the experiments show and suggest?**

- It looks better than prior methods!
- Thankfully, they don't just put a bunch of plots, there are tables that
  compare the number of games 'won' by an algorithm vs another algorithm.
- Still, hard to digest these results in the context of all the other algorithms
  DeepMind has done. I'll need to write a blog post organizing all the papers
  and algorithms.
