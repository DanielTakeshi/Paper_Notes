# Deep Spatial Autoencoders for Visuomotor Learning


Main contributions:

- Proposing an autoencoder structure which takes in high dimensional RGB images
  from an environment, and encodes it into a lower-dimensional feature
  representation, where, *critically*, the features represent something like
  *coordinates of objects*, which is useful for a variety of reasons. (It can't
  be exactly, of course, because that's cheating and hand-tuning ... the goal is
  to *learn* how to pick the relevant spots in images.) The output of the
  autoencoder is a grayscale image of the scene.

  - The encoder starts off as a simple CNN. Then they use their spatial softmax
    layer from their JMLR paper. These are useful because, unlike in computer
    vision domains which usually require neural networks to classify things and
    thus be *invariant* to position, in robotics and reinforcement learning, the
    position matters.

  - Interestingly enough, the decoder is just fully connected. Heh, a lot
    simpler.

- I view the above as the main takeaway, but their *algorithm* actually has
  three stages:

  1. Optimize an initial controller.
  2. Use that to collect data to train their deep spatial autoencoder via
  *unsupervised* learning. Remember, autoencoders can be trained without labels.
  3. Vision-based controller trained with the help of the autoencoder.

  Steps (1) and (3) use the same RL algorithm, actually, but step (3) has the
  advantage of getting a better state representation from the autoencoder, along
  with a new (better) cost function.


Note: it is important to understand why this work is different from their JMLR
2016 paper, because they use the same "spatial softmax" (followed by an
expectation) from that paper. The main difference is that they do not assume
extra knowledge about the problem setup.
