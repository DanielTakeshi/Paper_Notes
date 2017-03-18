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
convolutional neural networks.


## Figures and Visualizations

TODO


[1]:http://colah.github.io/posts/2014-10-Visualizing-MNIST/
