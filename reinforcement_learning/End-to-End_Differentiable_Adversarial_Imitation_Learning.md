# End-to-End Differentiable Adversarial Imitation Learning

*Quick note*: this paper also seems to be under an older title "Model-based
Adversarial Imitation Learning".

This paper cleverly extends GAIL so that it goes from model-free to model-based
(MGAIL).  Remember what that *means*. Model-free means we don't know the
dynamics (i.e. the values `p(s'|s,a)`). For our purposes, what matters is that
with stochastic policies, we don't know the exact gradients and have to resort
to REINFORCE-based gradient estimates based on rollouts.  This is what we do in
VPG, aka the REINFORCE algorithm. (If confused, see Pieter Abbeel's excellent
slides from his MILA Deep Learning and Reinforcement Learning Summer School 2017
talk. The model-based stuff uses path-wise derivatives and BPTT.) What's the
benefit of stochastic policies? It helps us explore more. And it lets us get log
probabilities that we need for gradient estimates (but does this count as a
benefit?).

The authors instead cleverly design "model-based GAIL" so that we know the exact
derivatives. They state:

> [...] with REINFORCE, G asks D whether the pattern it generates are authentic
> or not. D in return provides a brief Yes/No answer. On the other hand, using
> the exact gradient, G gets access to the internal decision making logic of D.
> Thus it is better able to understand the changes needed to fool D.

and then, when describing their mode-based approach, they say that:

> The challenge of this approach is that it requires learning a differentiable
> approximation to the environment's dynamics.

It is very unrealistic to assume we know the exact environment dynamics. Hence
why we need to approximate it, with neural networks obviously.

I understand how they got Equation 8 and its derivation, but it's not totally
clear to me how useful this perspective is, or why it inspired them to use the
Jacobian.

The main technical/math contribution is to utilize information from the
*Jacobian* of the Discriminator to derive an exact gradient. This requires
knowledge of a forward model (i.e. dynamics). See Sections 3.3 through 3.5 for
details, and note their use of a GRU layer. Before that, in Section 3.2, they
use the reparameterization and Gumbel-Softmax tricks to keep gradients flowing.
I need to blog about these ASAP. I think the advantage of model-based approaches
is that we know the forward model so the backpropagation can take prior states
into account, which is not the case with stochastic policies.

They compare two approaches to the forward model. See Figure 4. Unfortunately,
there is no explanation for why their advanced forward model is better beyond
empirical results.

**Experiments**: using the same environments as the GAIL paper, except they're
not using Reacher. This is disappointing, as Reacher gave GAIL the *most*
trouble and it would be nice to see how MGAIL does on that.  Note that for
network design:

> The discriminator and policy neural networks are built from two hidden layers
> with Relu non-linearity and are trained using the ADAM optimizer.

This makes sense. But why ReLU instead of tanh as done earlier?

**Observation**: their GAIL results are literally copied and pasted from the
GAIL paper, so they never bothered to re-implement it or run the original code?
I wonder how important it is to get the code bases to align. In addition, are
their MGAIL results really that much better than GAIL's? GAIL already did a
great job in the environments tested.

Code: `https://github.com/itaicaspi/mgail`

**Side comment**: it's somewhat relieving that this paper, building upon the
GAIL one, *doesn't* go through dense math but skips straight to the design and
results (admittedly, after some borderline excessive background material).  The
GAIL paper labored through lots of math before getting to the key idea. Also, I
really want to write a blog post about the reparameterization trick. That's been
coming up a *lot* lately ...
