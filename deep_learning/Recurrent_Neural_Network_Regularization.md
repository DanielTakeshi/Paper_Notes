# Recurrent Neural Network Regularization

This paper seems to be officially "arXiv 2014" since the authors literally ask
us to cite someone else's work. Interesting ... but the Tensorflow Tutorial for
RNNs appears to refer to this paper, so I'll be reading it anyway.


## Central Ideas

This is a natural language processing paper. This includes **language
modeling**, meaning that the model (i.e. the RNN) has to be able to determine
the probability of the next word in a sentence given the previous word. This is
the job of the LSTM. There are also speech recognition, machine translation, and
image caption generation experiments (wow!).

Their main contribution seems to be to show how to correctly apply dropout to
LSTMs. I guess it was tricky before? 


## Experiment Details

The dataset for language modeling comes from the Penn Treebank Corpus.
