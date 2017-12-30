# Deep Q-learning from Demonstrations

(Note: this paper was published under an earlier title "Learning from
Demonstrations for Real World Reinforcement Learning".)

This is highly relevant to papers such as Ashvin Nair's "Overcoming Exploration"
since we use human demonstrations to boost/improve standard reinforcement
learning algorithms, so it combines IL and RL.  It also makes sense that there
are situations when we have access to demonstration data but do not have
simulators. That's where an algorithm like this shines.

Refer to their algorithm as **DQfD**. How does it work?

- Combine temporal difference updates with supervised learning (via
  classification, as this is for discrete actions) on the demonstrator actions.
  Yes, this makes sense, and implies that DQfD can outperform the demonstrator,
  which by now seems to be a requirement for new IL algorithms.

  - But note that for the very beginning, we *only* train on the demonstrator
    data. This training, BTW, also uses TD losses. Thus, the algorithm does NOT
    "just" work by initializing a policy with behavior cloning. That would be a
    good baseline, though.
  - The pre-training uses **four** losses, not just one.
  - Looks like this was originally proposed (in some form) in "Boosted Bellman
    Residual Minimization Handling Expert Demonstrations", a 2014 ECML paper.

- Important contribution: use a prioritized replay buffer to *automatically*
  discover a good balance of demonstration data vs collected data via execution
  of the policy. I was wondering about that, but *this seems to overstate their
  contribution*. It looks like they have normal prioritized experience replay,
  except (a) the expert data is never over-written, and (b) there is a small
  constant which boost the expert data frequency. That's it. But I was hoping
  there would be something better than just that small constant ... 
 
- They *also* show that DQfD outperforms the following three algorithms:

  - Human Experience Replay. Hey, I know this one! (I tried it and it's not that
    good.)
  - Replay Buffer Spiking. Another one where the replay buffer is mixed with
    demonstration data.
  - Accelerated DQN with Expert Trajectories. I should ask Aravind about this.

- The amount of demonstration data is not that much, in fact it comes from just
  a few minutes of gameplay (per game)!

Very nice. I see why the algorithm works, and I think I could implement this.
Man, DeepMind's definitely the gold standard for deep RL/IL.
