# Using Fast Weights to Attend to the Recent Past

This looks like a nice core paper for Deep Learning. It seems to propose a new
type of weight that grows faster than "ordinary weights", though doesn't ADAGrad
kind of do that already, because a larger learning rate means the weights change
more?

Their introduction implies that there haven't been many recurrent neural network
"architectures" other than the "normal" one and LSTMs (though I think GRUs
count?). But other than those three, I don't actually know any, and I think the
Keras implementation only has those three types. Will this turn out to be a
fourth type? **EDIT**: no, that's not what they mean. They mean RNNs, including
LSTMs (and presumably, GRUs) each have two levels of memory, one for the hidden
layer(s) and another for its weights (confusing, but OK). They are proposing
a *third* level. They want this to have (a) better storage, and (b) faster
weight updates.

## Experiments

There are four main experiments.

- **Associative Retrieval**, hmm, might be new to me.

- **MNIST Classification**, yeah that one *again*.

- **Facial Expression Recognition**, should be interesting.

- **Reinforcement Learning Agents with Memory**. This sounds the most
  interesting. This benchmark is somewhat limited, though. It describes the game
  of *Catch* and they deliberately make it partially observable. Note to self:
  this is the easiest way to make something partially observable! They're using
  the A3C paper, which I know *also* uses memory.


## Takeaways

They're using a lot of analogies from the brain. This might not be a good idea
but I like it.

Key question: what is the connection with this and **attention models**? They
mention that it's similar but I didn't quite catch the specifics.

Another note: to get this to work well, it might be worth knowing more about
their *layer normalization* work.
