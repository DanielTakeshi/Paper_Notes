# Graying the Black Box: Understanding DQNs

## Quick Thoughts and Introduction

This might be useful for visualization and understanding DQNs. It's also related
to **t-SNE**, which I only know due to reading Chris Olah's blog. =) [Here's the
link to that post (warning, heavy computation!)][1].

The paper's contribution seems to be additional t-SNE experiments as compared to
the Nature 2015 paper (yeah, that one used some visualizations!). The benchmarks
are on three Atari 2600 games, though they suggest that these methods extend to
other domains. I can see the limitations of this work right away (and the
reviewers online seem to agree). However, that doesn't mean this paper isn't
helpful, and it got in ICML. Their main finding appears to be that agents
identify hierarchical patterns. I think that makes sense, given the layering in
convolutional neural networks. More specifically, the state space decomposes
into several (or many?) clusters, which are lower-dimensional manifolds, and
policies are learned for each of those clusters.

They also make an interesting distinction between *inner* optimization and
*outer* optimization:

> While the optimization of the inner level is analytic to a high degree, the
> optimization of the outer level is far from being so.

The *outer* optimization level addresses the choice of neural network
architecture, learning algorithm, and so on. Indeed, this is less principled
because it's harder to "loop over."


## Figures and Visualizations

They do this mostly by (1) extracting activations from the last DQN layer and
(2) applying t-SNE. How similar is (1) to the way that others have visualized
deep networks (think filters in convolutional neural networks).

Rationale for t-SNE makes sense:

> [t-Sne] is known to be particularly good at creating a single map that reveals
> structure at many different scales, which is particularly important for
> high-dimensional data that lie on several different, but related,
> low-dimensional manifolds.


[1]:http://colah.github.io/posts/2014-10-Visualizing-MNIST/
