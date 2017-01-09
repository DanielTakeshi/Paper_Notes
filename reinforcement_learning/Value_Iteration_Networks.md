# Value Iteration Networks

**General idea**. They introduce the *Value Iteration Network* (VIN), which they describe as "a fully differentiable neural network with a 'planning module' embedded within." They represent VINs with Convolutional Neural Networks (CNNs) trained via standard backpropagation. VINs can *learn to plan* and are useful for, among other things, reinforcement learning.

(I'm not sure why they had to add "fully differentiable" there, isn't that the default for neural networks?)


## Introductory Material

The most interesting part to me is when they said:

> However, most recent deep RL works [19, 16, 10] employed NN architectures that are very similar to the standard networks used in supervised learning tasks, which typically consist of CNNs for feature extraction, and fully connected layers that map the features to a probability distribution over actions. Such networks are inherently *reactive*, and in particular, lack explicit *planning computation*.

I agree, it certainly seems weird why we use similar CNNs. To mimic planning, we often use a sequence of frames as input, but that is still inherently reactive and doesn't scale well. They argue that this planning can be done with their VIN module, which can be plugged inside a normal neural network. This module is a representation of the classic value iteration network as a CNN and is (thankfully) model-free.

Key observation: suppose we have a policy which uses a CNN for Atari 2600 games. CNNs trained on two different games (assuming the have the same controls) will almost certainly not work. But let's make it simpler, how about grid world? They argue that if you re-randomize the grid-world setting (but still keep it "similar" enough) the CNNs in whatever standard DeepRL we use cannot generalize. Yes, I can believe this and the problem is challenging. Without VINs, we would have to resort to the following **bad** tactic to achieve our objective: increase the size of our neural network, then randomly generate a LOT of these Grid-World MDPs, covering "essentially" every possible configuration of obstacles, goal, and start states. *Then* we could generalize to an unseen, newly sampled domain. This would almost certainly be computationally intractable.

Notation/Formalism:

- They use standard MDPs with rewards, which are denoted using R(s,a) without the successor state.
- They describe standard Value Iteration as covered in CS 188.
- CNNs: nothing new here for me, but thanks for the review.
- RL and Imitation Learning: nothing new here, but thanks for the review. They don't consider the exact *learning algorithm* but instead the *policy*, i.e. the function \pi_\theta(a | \phi(s)). This is important, e.g. with DQN just knowing the neural network isn't enough, we also need to use experience replay, the target network, etc., but that's agnostic to the specific (neural network) representation of the policy.


## Algorithm

Something new: they use \bar{...} for a lot of things to represent a second MDP which is similar to the first but not exactly, e.g. another gridworld which still has the same problem structure? The idea is that the solution to the original MDP may utilize concepts gained from solutions to the other MDP. 

Note to self: associate "other" with the bar symbol, e.g., "other MDP".

I'm not sure why \bar{R} and \bar{P} only depend on \phi(s)? Don't you need the states and actions in the "other" MDP as well? It's not totally clear to me even after thinking about their running example (the gridworld setting(s)). OK, but anyway, I assume they have to *specify* the \bar{M}. Then we can find \bar{V}^\*, the optimal values for that other MDP. How can we use those to obtain the original policy \pi?

Two critical observations:

- The vector of "other" optimal values V\*(s) encodes all the information we need to know about the other MDP's optimal plan. Yes, because to find a policy, we --- in the discrete tabular case --- maximize over (the "other") Q\*(s,a) values, which can be written as (the "other") V^\* values. 
- Local connectivity structure means the other P(s'|s,a) is low or zero for most (s,a) pairs, hence it's related to attention models. Right, so the optimal solution only depends on a subset of the other V^\* values. I see, an example of \psi(s) could be the \bar{V}^\* values for a subset of neighboring states.

**Update**: After another read of the paper, I think I'm understanding Figure 2 better. We are given \phi(s) in the *original* MDP (so I guess the fact that only \phi(s) is input reflects that these are the only states we care about for the purposes of the "other" MDP). Then the other MDP performs Value Iteration. Then it will output a *subset* of these, in accordance with the connectivity structure of the other MDP. *Finally*, this information is provided to a *reactionary* policy which performs on the *original* MDP. Whew! And the Value Iteration process is *performed* via the VIN module, which we see in the other part of Figure 2. So it seems like they have to combine reactionary with planning; we can't completely ignore reactionary stuff.

Hardest part: backpropagating through the planning algorithm. They claim that the f_P and f_R functions are easier to design for backpropagation purposes, so let's focus on the planning algorithm. They claim:

> Our main observation is that each iteration of VI may be seen as passing the previous value function V_n and reward function R through a convolution layer and max-pooling layer.

Remember how the value iteration algorithm is formulated as V(s) = max_a (...)? I guess the max_a acts as the final max-pooling, and the convolution is the inner sum with the sum over the other values, weighted by the transition probabilities (hey, these could be the weights of the convolutional layer, right?). But no non-linearities, right? I don't think there would be. To better understand this, does it make sense to try and implement value iteration with just this CNN itself? Not sure how we'd get the weights but maybe I could fix and manually assign them to the CNN, and we can see if it works as expected?

Yes, looking at the paper again, the max-pooling is definitely the max_a part. That's easy, what about the rest? I *still* feel like I'm missing something. The overall steps are not "clicking." =( But it's probably hard to think about this. How does the stacking work, where the reward image is stacked with the previous \bar{V}?

I'm even more confused about:

> The VI block is simply a NN architecture that has the capability of performing an approximate VI computation.

Why is it *approximate* and not exact?


## Experiments

Four experiments: *Grid-World Domain*, *Mars Rover Naviation*, *Continuous Control*, and *WebNav challenge*. Key questions: can VINs do planning, and are VINs better than the reactive policies that are commonly seen?

(1) Grid-World, they generated the expert data from the optimal policy generated by exact VI, which is only feasible since the setting is simple. Their goal is not to actually get the optimal policy for a specific grid-world but to **plan** for it, i.e. how to generalize to new grid-worlds. I think the data they generated is from a new, held-out grid-world. I see, they use **prediction loss** and **success rate** as metrics. The former, I think, measures performance on (image, action) pairings from the ground-truth from exact VI. The latter is more important since it measures the percentage rate of running a trajectory without hitting obstacles (though this doesn't indicate how far off it's from the optimal trajectory, for that they use **trajectory difference**). By the way, the f_R is itself a CNN! So this is a *second* CNN ... this creates the grid which is *input* for the VIN. Wow. One of their competing methods is a CNN based on DQN which outputs a probability over actions. This should be easy to train since we can do exact VI on this, then generate a bunch of data and input it to the network. They present a gutsy conclusion:

> Rather, VIN policies, by their VI structure, focus prediction errors on less important parts of the trajectory, while reactive policies do not make this distinction, and learn the easily predictable parts of the trajectory yet fail on the complete task.

(2) Mars Rover, this is the logical next step, they take a *natural* image (from Mars!), and then convert it to a grid-world, and do the same thing they did earlier. Cool!

(3) Continuous Control, with continuous states *and* continuous actions, so cannot use naive VI. Instead, they discretize the setting into coarse grids. They use Guided Policy Search. When am I going to know how this works? Again, they seem to use a *distribution* of settings (MDPs?), as they say: "40 different test instances from the same distribution."

(4) WebNav challenge, given a query, the agent has to navigate through links to get to the goal webpage. Very interesting, it's definitely a lot different from the previous three experiments, all of which involved 2-dimensional navigation.


## My Thoughts and Takeaways

I'm not sure on the *exact* input/output of all this. How does this all *work* in practice? I know how DQN works end-to-end after obsessing over it for so long. I hope to do the same here. Also, I'm still confused about how the VI block works. I know it simulates an approximate form of value iteration (why not exact??) but I need to do the math and see if that's true. Or maybe I should check out the supplementary material? It can be hard to think about how all of these CNNs come together and are *trained* together in one go.

This seems similar to the RL^2 paper I reviewed earlier (on this github). I remember that paper *sampled MDPs from a domain*. That's similar to what's going on here with the gridworld case being sampled. Maybe the RL^2 paper got the idea from this one?

There's also a lot of connection to attention models. When am I going to become an expert in those?

Also, since there's K levels of recursion in the VIN, does that mean we use RNNs? I think it's CNNs.

The paper's idea is certainly interesting. Others must have been impressed, as this was the NIPS 2016 best paper award.
