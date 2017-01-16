# Probabilistic Inference for Determining Options in Reinforcement Learning

## Paper Summary and Contents

Note: I'm reading the version that appeared in an ICML workshop in 2015. I think this paper later got accepted into the journal named *Machine Learning* (different from *Journal of Machine Learning Research*) for the September 2016 edition so this may get outdated, but I think the general idea is the same.

Their focus is on tasks involving *sequential decisions* and they argue that the *option learning* framework will be useful, because the agent can learn different subpolicies (these are the options!) for specific tasks. The next task for the agent is to *combine* these options by determining starting and termination stages. They claim that existing option learning tactics must specify the options/subpolicies manually, but they will instead infer it from data. This is again a common theme I've been seeing in papers nowadays. =) Their setting also assumes a **Semi Markov Decision Process** (SMDP) which is like an MDP except the agent can take "macro-actions" which are like picking one action that will happen many time steps, or more generally, macro-actions correspond to simple policies, i.e. *options*. Oddly enough, SMDPs are used to "simplify the problem" in their own words, but SMDPs actually introduce more complexity by introducing extra actions. Or do they get rid of the individual action (unless those happen to be a policy which only performs one action)?

OK, *here's* how they define **options**:

> An option consists of a subpolicy, an activation policy and a termination probability.

I still conflate option=subpolicy, but since options are run for several steps until termination and only one option is run at a time, there must be some way to introduce the beginning/termination probabilities in an agent which is executing these.

**Contribution**: they develop an algorithm which uses Expectation-Maximization to determine options for both discrete and continuous settings (this is unlike most option learning work which uses only discrete cases). This lets them learn options from data with minimal prior knowledge. E-M should invoke thoughts of "graphical models" and fortunately there's one in Figure 1 representing an HMM (so Baum-Welch woud indeed be more appropriate than E-M).

Minor extra note: they use Q-learning for discrete case, policy search methods for the continous case. No deep learning here. =(

I see, Section 2 is for **imitation learning** and Section 3 is extending it to **reinforcement learning** where it has to explore instead of being given a dataset. Now wait a second, Section 3 uses an "exponential transformation of the advantage function." I'm not sure I follow what's going on here. They even have a temperature parameter \eta! This brings back bad memories. =(

Their update isn't like the Q-learning update, it's more of a probabilistic (soft? Like Roy Fox's paper?) update rule which involves KL-divegences. Hmmm ... somehow this turns into a weighted maximum likelihood estimate of the policy. 

No idea about what the HiREPs algorithm does. =(

**Experiments**:

- Figure 2 shows their discrete state (and action, btw) task with two variants of grid world (one with two rooms, another with traversing through a hall).
- This is the "underpowered swingup" problem. Unfortunately, I don't know the details. If this were a year later I'd demand OpenAI gym CartPole or something slightly more advanced.


## My Thoughts and Takeaways

It's another hierarchical RL paper, so a very common theme. Think of the agent as following a high-level controller which chooses a sub-policy, exceutes it for a while until it decides not to, then picks another sub-policy, etc. [TODO: Think of how to simulate this in my head; think of what probabilistic inferences are needed.]

They don't need to explain the details of the E-M algorithm, it's well-known by now.

I'm impressed by the way they form the update policy. I'm surprised that I haven't seen this form before as it seems to make some sense. Am I forgetting something? Or is it because I've focused more on discrete action space scenarios (e.g. Atari 2600, GridWorld (which also has discrete *state* spaces), etc.).

The bad news is the continued reliance on GridWorld tasks. But this seems like a workshop paper so it's OK.
