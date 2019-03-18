# Show, Attend, and Tell: Neural Image Caption Generation with Visual Attention

The short summary is that this is probably the extension of Karpathy's paper on
image caption generation, this time augmenting the model with attention. See
Justin Johnson's RNN lecture where he describes how the model works. The
extensions:

- Feed image through CNN, get a list of vectors (not just one). Each vector
  represents some spatial location in the scene.  (Not sure on the details of
  this, unfortunately.)

- Each output at time steps produces distribution over those vectors (i.e.,
  those spatial locations in the image). This is in addition to the distribution
  over words, which we sample from to get the words.

- Then that distribution over spatial locations is fed as input to the next time
  step in the RNN. Because it's a distribution, it has higher weight towards
  more important spatial regions for the captioning. Just think of this as a
  linear combination over features, where the weights are *learned* from the
  network. That's cool --- I often find it hard to internalize how these
  attention weights are magically learned.

- Actually I spoke too soon: the paper uses both soft and hard attention. Hard
  attention doesn't use the weights, it just picks one. The training is
  *stochastic* in this case, versus *deterministic* in soft attention, which the
  paper is clear about.

- Use LSTMs instead of vanilla RNNs.

Then due to the above algorithmic extensions, *interpretability* is possible by
visualizing the locations that were weighted highest at each time step. That's
very cool.

I would like to understand whether this paper was the one introducing the
hard/soft variants, and how much of the 'attention' based stuff people use
nowadays is from this paper, rather than earlier ones (though there are only a
few earlier ones, I think!).
