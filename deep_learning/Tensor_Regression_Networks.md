# Tensor Regression Networks

**Main idea**: CNN architectures work with tensors, since they work with images
generally so the input is like `(N, 84, 84, 3)` if we're using RGB images.
However, as I know, eventually the convolutional layers must end and the output
should transform into a fully connected layer. This requires *flattening* the
current data so that it's of shape `(N, D)` for some `D`. This throws away
information (intuitively) so the authors have a **tensor regression layer**
which replaces this flattening part (+fully connected layers afterwards).
Clever! 

This has two benefits:

- Leveraging the tensor "structure" instead of throwing away information.
- Reducing the number of parameters. Indeed, recall that FC layers are probably
  the most inefficient network layers.

Instead of flattening to get the final layer output (i.e. predictions) they use
"low rank tensor decomposition." The "low rank" must be what captures the
"essence" of the tensor.

The main technical part is showing how this can happen while *also* making the
network differentiable. This is in a similar style as many papers, which
introduce network components. They have to be made differentiable (to pass the
peer review stage, of course). In particular, if we're doing a tensor
contraction to get lower dimensional representations (formally called a **Tensor
Contraction Layer**) this requires knowing the *projection factor* matrices
`V^(k)`. These are learned end-to-end with gradient backpropagation. The second
layer they use is a **Tensor Regression Layer**.

Thus, they've got TCL and TRL layers. Note that contraction can (and probably
should) happen before regression.

**Benchmarks**: they show that they can reduce the number of parameters in VGG
and ResNet and still obtain state of the art performance. Models were
implemented with the MXNet library, which might be better than TensorFlow for
this purpose. And, um, MXNet is affiliated with Amazon, like these authors.

I think deeply understanding this requires more knowledge of tensors than I
have, but I can see the rationale.
