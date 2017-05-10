# Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift

This paper introduces *Batch Normalization*, a technique that by now has grown
wildly popular and is, I think, a standard feature of any state of the art
network (though I'm not totally sure, have to check on that). I won't spend too
much time writing up notes because I coded it in the Stanford CS 231n
assignments; that's the best way to learn about batch normalization. I'm never
sure I get what the *dimensions* of the inputs are in this paper. It's easier to
understand in code. In CS 231n, batch normalization is applied *before* each
ReLU.

However, here are the highlights on *what* batch normalization means and why we
like it:

- Batch normalization tries to ensure that the input distribution to each layer
  (technically, each nonlinearity) is the same. That is, each scalar-value
  feature should be standardized to mean 0 and variance 1. Why is this useful?
  Because if not, deep networks are designed so that small changes early have
  large changes later, hence input distributions may differ substantially from
  minibatch to minibatch, and the weights have to "adjust" to that (or suffer
  from vanishing gradients). The authors call this **Internal Covariate Shift**.

- Batch normalization adds extra *scale* and *shifting* parameters to allow the
  identity transformation to be represented. This means batch normalization does
  not "constrain" the network. Anything that the non-batch normalized network
  can represent, so can the batch normalized version. To be clear, think about
  the 1-D case with a sigmoid. If our minibatch of data x1,...,xn (all
  scalar-valued) are normalized, then the sigmoid layer will often represent
  stuff in its "linear" regime and not in its extremes (and sometimes those
  extremes are needed!). Here's what CS 231n says:

  > It is possible that this normalization strategy could reduce the
  > representational power of the network, since it may sometimes be optimal for
  > certain layers to have features that are not zero-mean or unit variance. To
  > this end, the batch normalization layer includes learnable shift and scale
  > parameters for each feature dimension.

- It's better than previous methods (e.g. whitening each layer) because it's
  relatively cheap (no need to compute O(n^2) matrices) and it works seamlessly
  with backpropagation. That's the key; everything must be differentiable, and
  backpropagation must *know* that it is computing gradients of features that
  are *defined* to be normalized.

- During training, minibatch statistics (i.e. mean and variance) are estimated
  via a moving average, but during inference (i.e. test-time application) it's
  best to have the statistics be the same, so compute it from the whole training
  data. This makes sense, we need determinism (modulo the training data).
  Alternatively, after training, we have our minibatch statistics. Thus, we
  could just use those final estimates and fix them.

The paper argues convincingly with better results. On MNIST, their LeNet version
with BN outperforms the default, and they improve upon the best ImageNet
results (from He et al). They also show that sigmoids can be used, and that
other regularization tactics such as DropOut are not necessary. There are other
techniques one can use to gain more out of batch normalization, such as using
higher learning rates.

TODO: I really need to go through the derivatives again to make sure I
understand them. This helps me to know why everything is trainable. Then I'll
check [this][1] and [this][2].

[1]:https://kratzert.github.io/2016/02/12/understanding-the-gradient-flow-through-the-batch-normalization-layer.html
[2]:https://kevinzakka.github.io/2016/09/14/batch_normalization/
