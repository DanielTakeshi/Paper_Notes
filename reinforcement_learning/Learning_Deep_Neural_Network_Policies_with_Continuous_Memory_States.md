# Learning Deep Neural Network Policies with Continuous Memory States

## Algorithmic Contribution

Not totally clear on this, but it seems like they're trying to avoid
backpropagation? They don't use LSTMs or GRUs.  They must do backpropagation to
train the neural network *policy*, but this might be distinct from the *memory
states*, I think.


## Experimental Results

One takeaway is to figure out the best way to encode partial observability to
make the problem solvable but also interesting.


## Thoughts and Takeaways

I was interested in reading this paper because it's about partial observability
and RNNs in reinforcement learning.  This paper was in ICRA 2016 but it's really
almost a pure Deep RL paper (which might not yet be expected at ICRA). I think
it also uses pure simulation, which might be interesting as well.

I also need to learn **Guided Policy Search**. I know it splits up the policy
search into alternating stages:

- **Trajectory Optimization**: produces training data for supervised learning.
- **Supervised Learning**: train a nonlinear (aka neural net) policy.

But how does it *really* work? I need intuition!!

The main contribution of this paper, now that I have read it, is an extension of
the previous well-known GPS paper so that it can train policies with memory.
