# Value Iteration Networks

**General idea**. They introduce the *Value Iteration Network* (VIN), which they describe as "a fully differentiable neural network with a 'planning module' embedded within." They represent VINs with Convolutional Neural Networks (CNNs) trained via standard backpropagation. VINs can *learn to plan* and are useful for, among other things, reinforcement learning.

(I'm not sure why they had to add "fully differentiable" there, isn't that the default for neural networks?)


## Introductory Material

The most interesting part to me is when they said:

> However, most recent deep RL works [19, 16, 10] employed NN architectures that are very similar to the standard networks used in supervised learning tasks, which typically consist of CNNs for feature extraction, and fully connected layers that map the features to a probability distribution over actions. Such networks are inherently *reactive*, and in particular, lack explicit *planning computation*.

I agree, it certainly seems weird why we use similar CNNs. To mimic planning, we often use a sequence of frames as input, but that is still inherently reactive and doesn't scale well. They argue that this planning can be done with their VIN module, which can be plugged inside a normal neural network. This module is a representation of the classic value iteration network as a CNN and is (thankfully) model-free.

Key observation: suppose we have a policy which uses a CNN for Atari 2600 games. CNNs trained on two different games (assuming the have the same controls) will almost certainly not work. But let's make it simpler, how about grid world? They argue that if you re-randomize the grid-world setting (but still keep it "similar" enough) the CNNs in whatever standard DeepRL we use cannot generalize. Yes, I can believe this and the problem is challenging.

Notation/Formalism:

- They use standard MDPs with rewards, which are denoted using R(s,a) without the successor state.
- They describe standard Value Iteration covered in CS 188.
- CNNs: nothing new here for me, but thanks for the review.
- RL and Imitation Learning: nothing new here, but thanks for the review. They don't consider the exact *learning algorithm* but instead the *policy*, i.e. the function \pi_\theta(a | \phi(s)). This is important, e.g. with DQN just knowing the neural network isn't enough, we also need to use experience replay, the target network, etc., but that's agnostic to the specific (neural network) representation of the policy.


## Algorithm

Something new: they use \bar{...} for a lot of things to represent a second MDP which is similar to the first but not exactly, e.g. another gridworld which still has the same problem structure? The idea is that the solution to the original MDP may utilize concepts gained from solutions to the other MDP. 

Note to self: associate "other" with the bar symbol, e.g., "other MDP".

I'm not sure why \bar{R} and \bar{P} only depend on \phi(s)? Don't you need the states and actions in the "other" MDP as well?

Two critical observations:

- The vector of optimal values V\*(s) encodes all the information we need to know about the other MDP's optimal plan. (TODO explain more)
- Local connectivity structure means P(s'|s,a) is low or zero for most (s,a) pairs, hence it's related to attention models. (TODO explain more)

Hardest part: backpropagating through the planning algorithm.

TODO


## Experiments

Three experiments: *Grid-World Domain*, *Mars Rover Naviation*, *Continuous Control*, and *WebNav challenge*.

TODO


## My Thoughts and Takeaways

I'm not sure on the *exact* input/output of all this. How does this all *work* in practice? I know how DQN works end-to-end after obsessing over it for so long. I hope to do the same here.

This seems similar to the RL^2 paper I reviewed earlier (on this github). I remember that paper *sampled MDPs from a domain*. That's similar to what's going on here with the gridworld case being sampled. Maybe the RL^2 paper got the idea from this one?

There's also a lot of connection to attention models. When am I going to become an expert in those?
