# A Complete Recipe for Stochastic Gradient MCMC

## Quick Thoughts

This is also in the theme of "stochastic gradient" (i.e. taking subsets of the
data to compute gradients, easy to scale) and "MCMC" stuff (a method to
approximate some distribution via sampling, and typically associated with
Bayesian-ism, but can be inefficient). MCMC nowadays require *dynamics* to
efficiently and accurately sample from the posterior, but this means computing a
gradient that's prohibitively expensive with large datasets, which is why we
need working stochastic gradient versions.

In this paper, the authors show some recipe or "unifying framework" to map MCMC
samplers to their formulation. (I can see the benefits of this kind of work,
since it helps when algorithms are "special cases" of general statements.) Then
they propose a new algorithm, **Stochastic Gradient *Riemann* Hamiltonian Monte
Carlo (SGRHMC)** which is supposed to get the benefits of Riemann HMC with the
*scalability* of stochastic gradient methods.

There is overlap with the authors of this paper and the SGHMC paper (from ICML
2014) so the similarity here is not surprising. And I have to remember, any time
we talk about "benefits" of HMC, it's almost always due to *efficient
exploration*, as compared to random walks.


## Intuition

The following passage about related work makes sense:

> In the plethora of past MCMC methods that explicitly leverage continuous
> dynamics -— including HMC, Riemann manifold HMC, and the stochastic gradient
> methods -— the focus has been on showing that the intricate dynamics leave the
> target posterior distribution invariant. Innovating in this arena requires
> constructing novel dynamics and simultaneously ensuring that the target
> distribution is the stationary distribution.

What they are trying to do is *unify* all of these into a single framework so
that all (really, all?!?) of these methods *and* future variants can be cast
into this.

Their framework is to define a system with two matrices, D(z) (the diffusion
matrix) and Q(z) (the curl matrix), with z = (\theta, r), similar to what
Radford Neal describes in his tutorial, the parameters of interest followed by
auxiliary variables.

To cast algorithms into this framework, one therefore needs to define D and Q.
In addition, future algorithms may be devised by creatively setting D and Q.
Interesting ...

Their **experiments** involve synthetic datasets and sampling from an LDA model
on a Wikipedia dataset.  Figure 2 plots performance measured in KL-Divergence
from the posterior. Yes, that's a good idea when you know the target, as they do
with simulated data. However, **no error bars are given** ... did they run these
only once? The simulated data should be cheap enough to run many times. For the
LDA, they measure using **perplexity**.


## Technical Material

High-level overviews of the theorems:

- **Theorem 1** shows that, if Equation 3 holds, then the samples form a
  stationary distribution.

- **Theorem 2** shows that, for any continuous Markov process with a desired
  stationary distribution, there exists some D and Q in their formulation which
  matches the sampling distribution.

Theorem 2 covers more algorithms than Theorem 1, and the relationship is a
superset.

I'm skipping the proofs.

They begin their new algorithm, SGRHMC, by noting that previous work, when cast
into their framework, does *not* sufficiently fill in the space D x Q of
possible D and Q matrices. Clever! (I also liked how they showed that they
couldn't cast naive SGHMC into their framework, but their *actual* SGHMC *can*
be cast into it ... showing that their framework can catch errors.)


## Concluding Thoughts

I wasn't sure what to expect out of this paper since it's not totally clear what
a "complete recipe" means, and it's hard for me to tell how large a class of
algorithms this recipe contains. However, it seems to be impressive and covers a
wide class of algorithms.
