# Learning to Predict Where to Look in Interactive Environments Using Deep Recurrent Q-Learning

This is a recent arXiv paper (last updated February 2017) in the ICLR format,
though I don't think it was submitted there. However, the concepts look
interesting, which is why I'm skimming this.

They "propose" (more like "borrow", see later) a soft attention Deep Q-Network
to train an agent to play a game by having it focus on the relevant pixels. They
use human subjects (!!) to get ground truth "attention model labels," which they
can use to benchmark their algorithm with two prior works: *Itti-Kochs Saliency*
and *Graph-Based Visual Saliency* (I'm not familiar with these). They also show
that their agent/algorithm can still learn to play games (but it *should*, after
all!). 

I think their results are more towards predicting attention model regions rather
than getting the best game scores possible. They do not even report on game
score anywhere in the paper! The metrics for "predicting fixation locations" are
based on **Normalized Scanpath Saliency** and **Area Under the Curve**.

**Network architecture**: Figure 1 looks helpful, though not helpful enough.
I'm not sure what they mean by the attention model producing "vertical slices,"
are those just column vectors? Just think of these as the usual input to
attention models.

Question: is their attention mechanism only for visualization purposes, or is
there actually some savings done by this, i.e. are the original image inputs
weighted by the attention model somehow, so that the Q-Network can "ignore" the
non-attended regions? I think so because they say:

> In fact, learning to choose the optimal action based on the region selected
> from among provided candidates (vertical feature slices resulting from the
> last convolution layer of the CNN part of the model, see C_t in Section 3.1).

**Major oversight**: they cite the Deep Recurrent Q-Network, but not the Deep
*Attention* Recurrent Q-Network paper! The latter was in the NIPS 2015 Deep
Reinforcement Learning workshop. To make matters worse, their paper also uses
LSTMs (like the DARQN paper).
