# RL^2: Fast Reinforcement Learning via Slow Reinforcement Learning

General idea: they encode an RL algorithm inside the weights of a Recurrent Neural Network (RNN). This is designed to be the "fast" part, since there's prior knowledge, and the "slow" part is when they have to train the weights. The problem they tackle is the need to use a large number of samples and trials for general RL algorithms, particularly with DQNs, and PGs; they wish to use priors. The name RL^2 comes from "using reinforcement learning ... to leran a reinforcement learning algorithm"!


## Introductory Stuff

This seems similar to what Roy Fox's papers deal with, the need to encode priors. However, here, the priors are embedded inside the RNNs. It's part of the larger category of *Bayesian Reinforcement Learning*. (Note: Roy Fox's G-learning is not a Bayesian algorithm.) They argue:

> Rather than hand-designing domain-specific reinforcement learning algorithms, we take a different approach in this paper: we view the learning process of the agent itself as an objective, which can be optimized using standard reinforcement learning algorithms. The objective is averaged across all possible MDPs according to a specific distribution, which reflects the prior that we would like to distill into the agent.

This is interesting. So rather than have a single learning algorithm (gradient descent?) optimize one set of weights, are they doing a search "one level up" to search across learning algorithms? I think that's what they're doing, judging from this statement later:

> We now describe our formulation, which casts learning an RL algorithm as a reinforcement learning problem, and hence the name RL^2.


## Algorithm

Terminology:

- Episode: a start-to-finish sequence of interaction with an MDP.
- Trial: a series of episodes of interaction with a fixed MDP; # of episodes for a fixed MDP is set at n. 
- Inner Problem: the act of seeing one MDP and running n episodes on it. It could be seeing one POMDP but they choose MDP for simplicity.

The description is interesting. Rather than have a fixed MDP, they have *a distribution over MDPs* and are sampling from it (it's easy to do this with toy MDP cases). Then each "trial" consists of a sampled MDP and a set of episodes on it. Wow, I'm liking this idea. The objective is to maximize the expected discounted reward over the set of episodes for one trial (not an episode).

The RNN inputs are (next_state, action, reward, termination_flag). They use \phi to embed it in some way; I'll need to look at the code for that. They use Gated Recurrent Units (GRUs). When am I going to know how these work?

TRPO is used to optimize the policy. Now, this is the policy for the inner problem? TODO clarify....


## Experiments

Two sets of experiments:

- MABs and Tabular (to test if the algorithm is close to being optimal)
- Vision-based (to test scalability)

For the MABs they can test with many algorithms, including some of which are known to be theoretically optimal.

For the vision-based test, they use a maze.


## My Thoughts and Takeaways

I will read the code for this later. I'm mostly wondering about the "using RL to learn a RL algorithm", I need to understand precisely what that means.

The paper has lots of details on the experiments, which is nice. The experiments themselves, though, use some fairly basic settings.
