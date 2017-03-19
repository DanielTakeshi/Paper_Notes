# Graying the Black Box: Understanding DQNs

## Quick Thoughts and Introduction

This might be useful for visualization and understanding DQNs. It's also related
to **t-SNE**, which I only know due to reading Chris Olah's blog. =) [Here's the
link to that post (warning, heavy computation!)][1].

The paper's contribution seems to be additional t-SNE experiments as compared to
the Nature 2015 paper (yeah, that one used some visualizations!). The benchmarks
are on three Atari 2600 games, though they suggest that these methods extend to
other domains. I can see the limitations of this work right away (and the
[reviewers seemed to agree][2]). However, that doesn't mean this paper isn't
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

Figure 1 looks good; they provide a graphical user interface. Seems like this
would be a hybrid of UIST and ICML. I particularly like the *gradient image*,
though naturally the t-SNE image is the real star.

I think they are trying to color the t-SNE image in **Section 4.2** with
hand-designed features, because otherwise how could we tell the clusters? (I
remember with MNIST, one can color the images by their class, so I assume this
is something similar.) 

Also, what are saliency maps (**Section 4.3**)? I think I saw these before but I
need to refresh my memory.

Maybe t-SNE requires these features as input, instead of the raw pixels?


## Analysis of Games

- **Breakout**: The hierarchical policy is (to simplify a bit) in two stages:
  first, carve a tunnel, then, keep the ball above the bricks. They demonstrate
  these clusters with t-SNE. Looks cool! Also, **Section 5.5** addresses
  something that's been long bugging me: what happens when the first stage of
  the game is cleared, and the board starts anew but with the same setting,
  except the score is 450 (or whatever the max score is for that first setting
  in Breakout). How does the agent do in that *second* setting? The pixels are
  exactly the same, except for the score counter. If we cropped the score
  counter, shouldn't the agent perform flawlessly?

- **Seaquest**: This is harder for DQN than humans (at least, at the time this
  paper was published). The paper talks about specific scenarios in the game.
  Like the Breakout game, different *options* are identified.

- **Pacman**: Interesting, I haven't seen this discussed in detail. Two t-SNE
  plots are provided.

The tradeoff with this paper is that it focuses very specifically on the actions
and policies that are unique to these three games. The techniques of t-SNE and
manual feature extraction (ugh) should certainly generalize.


[1]:http://colah.github.io/posts/2014-10-Visualizing-MNIST/
[2]:http://icml.cc/2016/reviews/880.txt
