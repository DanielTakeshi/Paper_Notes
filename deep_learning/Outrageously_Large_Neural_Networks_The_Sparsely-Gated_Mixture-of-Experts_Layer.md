# Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer

## Thoughts and Takeaways

Wow, I'm excited about this one. ***Outrageously*** large?? Please. =) Their
main contribution is indeed the **Sparsely-Gated Mixture of Experts** layer. It
lets them perform **conditional computation**.  This means when a sample is
fed-forward through a network, only *parts* of the network are active at once.
(It reminds me of dropout; they actually don't cite that paper (!!) but instead
others that deal with recurrent nets, even though [those authors said][1] not to
cite them!!) This lets them better scale to larger model sizes since they can
use the similar architecture.

They say:

> In this work, we for the first time address all of the above challenges and
> finally realize the promise of conditional computation. We obtain greater than
> 1000x improvements in model capacity with only minor losses in computational
> efficiency and significantly advance the state-of-the-art results on public
> language modeling and translation data sets.

Their largest networks have **137,000,000,000** (137 billion!) parameters!

There's a *lot* of related work that I'd like to read. Wow, there is never
enough time. =( I wish i were John Canny.

Onto some specific stuff ...


## The Layer

The SGMoE layer contains multiple fully connected nets *inside* it. This doesn't
seem exciting, until they explain that their nets also have a **trainable gating
network** which chooses a (sparse!) set of experts to draw each time.

By the way, the key to making "new components" in neural networks is to figure
out how to compute gradients (for backpropagation). That's how we can get exotic
network components.


## Experiments

TODO


[1]:http://www.cs.toronto.edu/~ilya/pubs/
