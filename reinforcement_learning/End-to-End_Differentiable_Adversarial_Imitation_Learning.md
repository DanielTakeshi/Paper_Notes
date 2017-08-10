# End-to-End Differentiable Adversarial Imitation Learning

*Quick note*: this paper also seems to be under an older title "Model-based
Adversarial Imitation Learning".

This paper cleverly extends GAIL so that it goes from model-free to model-based
(MGAIL).  Remember what that *means*. Model-free means we don't know dynamics,
etc., and for our purposes, what matters is that with stochastic policies, we
don't know the exact gradients and have to resort to gradient estimates based on
rollouts.  This is basically what VPGs do. (If confused, see Pieter Abbeel's
excellent slides from his MILA Deep Learning and Reinforcement Learning Summer
School 2017 talk. The model-based stuff uses pathwise derivatives and BPTT.)

The authors instead cleverly design "model-based GAIL" so that we know the exact
derivatives. They state:

> The challenge of this approach is that it requires learning a differentiable
> approximation to the environment's dynamics.

Indeed, since it's usually best not to assume we know the exact dynamics.

The main technical/math contribution is to utilize information from the
*Jacobian* of the Discriminator to derive an exact gradient. Note that this
requires knowledge of a forward model (i.e. dynamics). See Sections 3.3 through
3.5 for details, and note their use of a GRU layer.

The experiments were conducted using the same environments as the GAIL paper
used, except they're not using Reacher. This is a bit of a disappointment, as
Reacher was probably the environment that gave GAIL the *most* trouble. It would
have been nice to see if MGAIL would have imitated it better. Note that for
network design:

> The discriminator and policy neural networks are built from two hidden layers
> with Relu non-linearity and are trained using the ADAM optimizer.

This makes sense.

Wait ... their GAIL results are a copy/paste version of the old one. I wonder
how important it is to get the code bases to align. In addition, are their MGAIL
results really that much better than GAIL's?

**Side comment**: it's somewhat relieving that this paper, building upon the
GAIL one, *doesn't* go through dense math but skips straight to the design and
results (admittedly, after some borderline excessive background material). As we
know, the GAIL paper labored through lots of math before getting to the key
idea. Also, I really want to write a blog post about the reparameterization
trick. That's been coming up a *lot* lately ...
