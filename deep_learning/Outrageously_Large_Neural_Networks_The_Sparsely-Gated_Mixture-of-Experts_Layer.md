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

Their largest networks have **137,000,000,000** (137 billion!) parameters! The
sheer size alone of these models warrants interest in this paper, methinks.

There's a *lot* of related work that I'd like to read. Wow, there is never
enough time. =( I wish i were John Canny. Anyway, make sure to understand all
the reasons why conditional computation has not yet been able to scale. They
make sense to me, though I don't have much experience dealing with measuring
network bandwidth; I'll just have to trust them. I really like how 600,000
images is considered a "small dataset" nowadays.

In the introduction, I have a question: what precisely is meant by "top-level"
mixture of experts? Are these models where we have one hierarchy level? That is,
if we make a choice, we pick one of the N experts that we have, and that's it?
No further hierarchy? I think so but I'm not sure ... this is also related to
their discussion later on two-level hierarchical MoEs. However, they focus on
"ordinary MoEs" in this paper. Thus, we use one level, right?

Onto some specific stuff ...


## The Layer

The SGMoE layer contains multiple fully connected nets *inside* it. This doesn't
seem exciting, until they explain that their nets also have a **trainable gating
network** which chooses a (sparse!) set of experts to draw each time. As
expected, each expert has to take the same-sized input and create same-sized
output. For simplicity, they focus on the experts all having the same
architecture. I can imagine LOTS of variants of this going forward.

Digression: the key to making "new components" in neural networks is to figure
out how to compute gradients (for backpropagation). That's how we can get exotic
network components.

It is critical to understand how **gating** works. How does one compute G(x),
the vector which applies weights to experts? E.g. if G(x) = [0 0 1 0 2], then
only the third and fifth experts have to do computation, and we weight the fifth
expert twice as much. Got it?  Early work (Jordan & Jacobs 1994) simply used a
trainable weight matrix to produce G(x), which will NOT produce sparsity. This
work (1) **adds noise**, and (2) **adds a "KeepTopK" operation**. In that order.
The KeepTopK should be backpropagated in an analogous way as ReLUs.

How to address challenges?

- **Batch Sizes**: I agree that it is necessary to keep batch sizes large, up to
  a point. They use distributed systems. I don't quite understand their
  descriptions for convoultions and recurrent nets, unfortunately. =( I really
  need to get more distributed systems programming experience.

- **Network Bandwidth**: I think I understand this one better. The bottleneck is
  communication and data transfer, NOT the backpropagation within each expert
  (since GPUs do that quickly). Thus, to increase computational efficiency, just
  add more parameters to the experts!

- **Balancing Expert Utilization**: Oh I see, the naive way of using MoEs will
  result in a few experts dominating all the time. To address this imbalance,
  they introduce new loss functions.

I think I need to re-read the paper, and delve into the Appendix, to understand
these fully. Their explanations don't seem too complicated.


## Experiments

Here are the experiments:

- **Language Modeling**: they test with a benchmark that has one billion words,
  and a second benchmark with 100 billion words (from the Google News corpus).

- **Machine Translation**: benchmarked on WMT'14 En->Fr and En->De corpora. I
  don't know too much about these datasets to comment, unfortunately.

To give an idea of the number of experts inside each SGMoE layer, they use
numbers ranging from 32 to 131072.

[1]:http://www.cs.toronto.edu/~ilya/pubs/
