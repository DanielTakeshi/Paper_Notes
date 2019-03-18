# Effective Approaches to Attention-based Neural Machine Translation

This is one of the most impactful attention-based papers. It's for machine
translation (i.e., one language to another...) so a different task than Xu's
attention-based paper for image captioning. The most similar paper is probably
*Neural Machine Translation by Jointly Learning to Align and Translate*
(Bahdanau et al., ICLR 2015). Also, remember that this was before the
transformers paper was published. :-)

The general idea:

- We are follow the encoder-decoder framework for neural machine translation.
  
  - The source sentence goes through the encoder and produces a bunch of hidden
    states at each time stpe.
    
  - The decoder takes something as input, and produces the resulting sentence.
    It starts with `<START>` as the token, and uses a generative model to
    produce the sentence, similar to how Andrej's image captioning paper works.
    (I think that's how this works.)
    
- I think historically, people used to just pass the last hidden state from the
  encoder and pass that as input. But, the obvious downside is that we have to
  stuff all the input sentence's information into that hidden state.  Why not
  just use all of the hidden state outputs? That's what this paper does. (Other
  solutions involve stuff like reading the input in reverse, which is what Ilya
  Sutskever's paper does.)

- At each time step, the hidden states in the *decoder* will take as input the
  prior decoder hidden state, but also another vector (Chris Manning calls it
  the "context vector") which is the attention-weighted linear combination of
  all the hidden state output vectors from the encoder. Thus, the context vector
  is like the summary of the "memory" from the encoder, but now we don't have to
  use all the hidden states but just one vector (b/c it's a linear combination
  of them). The attention weights come from a *score function* which can be a
  simple matrix multiply, like a one-layer feed forward neural network.
