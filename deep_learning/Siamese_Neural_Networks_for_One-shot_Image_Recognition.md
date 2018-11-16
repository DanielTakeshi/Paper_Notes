# Siamese Neural Networks for One-shot Image Recognition

I finally got around to reading the Siamese NNs paper. Well, the real one is
from Jane Bromley's paper, but this is the major one that's post-Deep-Learning,
if I am understanding correctly.

The idea is straightforward. We can pass two images into the network. The images
are passed through identical convolutional streams (i.e., weights are tied).
Then, they are combined by not concatenation (as I would have naively thought)
but by computing the L1 distance between the vectors. (Well, not quite, there
are some alpha terms multiplied to it.) Then the sigmoid is applied. I think the
alpha terms can make the "L1 distance" negative so that the sigmoid's output can
operate over the full range of [0,1]. Thus, to train, all we need are pairs of
images, and the label (or supervisory signal) is whether the images are in the
same class or not.

Evaluation is done on the Omniglot dataset, which I _finally_ understand now.
There are 50 alphabets, and data are images of handwritten symbols. They apply a
considerable amount of affine transforms, so the training data is quite large.

How to apply this to test-time, for a novel image of unknown class? (But we do
assume that the training process saw one instance of an image from that same
class.)

To be clear: there are a set of 40 alphabet background set characters, and then
10 alphabet evaluation characters. The 40 can be used for lots of training, and
the other 10 are used to evaluate one-shot classification where we can only see
one sample from each class. (This protocol can be seen in Section 4.3, read it
carefully if I ever have to replicate this.)
