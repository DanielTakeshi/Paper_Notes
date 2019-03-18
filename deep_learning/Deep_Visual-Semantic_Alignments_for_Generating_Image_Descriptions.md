# Deep Visual-Semantic Alignments for Generating Image Descriptions

Ah, this is the A. Karpathy & L. Fei-Fei paper which describes using recurrent
models for image captioning, and which forms the basis of the corresponding 231n
homework assignment on image captioning.

The basic idea is simple: start with an image, pass it through a CNN, and then
we get features. We can then run an RNN on this, as the initial hidden state (or
possibly some learned mapping of it). The initial *input* state is the `<START>`
token. Then run the RNN, and the output at each time step is a distribution of
scores over all the words. We sample that, and use the sampled word as the word
in the generated captioning.  We *also* use it as the next input state, which we
then feed through the RNN parameters to get the output. 

I don't think they benchmark against any other non-Deep Learning approaches.
This was probably the starting point for Deep Learning and image captioning
(this was CVPR 2015 so the work was done in 2014 probably ...).

Data: MSCOCO and others. Nowadays, MSCOCO is probably most commonly used for
future image captioning tasks.

What seems surprising is that they didn't even need to use LSTMs. It's vanilla
RNNs, well bidirectional RNNs.
