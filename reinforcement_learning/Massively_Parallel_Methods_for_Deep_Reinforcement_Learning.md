# Massively Parallel Methods for Deep Reinforcement Learning

This is the rare workshop paper that looks useful. I think most of this stuff is
subsumed by the A3C paper, because the parallel stuff was incorporated into
there, and the "massive distributed stuff" was eliminated and shown to be
improved in the A3C paper.

However, this has the critical "human starts" metric that will be important for
future benchmarks.

Evaluation:

I think this is how DeepMind normally evaluates:

> We used two types of evaluations. The first follows the protocol established
> by DQN. Each trained agent was evaluated on 30 episodes of the game it was
> trained on. A random number of frames were skipped by repeatedly taking the
> null or do nothing action before giving control to the agent in order to
> ensure variation in the initial conditions. The agents were allowed to play
> until the end of the game or up to 18000 frames (5 minutes), whichever came
> first, and the scores were averaged over all 30 episodes. We refer to this
> evaluation procedure as null op starts.

So, this involves training the agents, and then doing 30 held-out evaluation
trajectories. OK, this is not the same as OpenAI's metric ... but I assume if
DeepMind were concerned about *accelerating learning* then they would evaluate
based on some average (over a window) of training episode rewards.

For the human starts metric, it's similar except there are 100 starting points,
and the trained policy engages in those 100 episodes.

Basically, I think any paper that has only tables or reports final numbers uses
held-out evaluation rollouts, whereas those that involve full learning curves
need training data episodes.
