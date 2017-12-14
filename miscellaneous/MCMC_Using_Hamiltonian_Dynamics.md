# MCMC Using Hamiltonian Dynamics

**Update December 2017**: Check out [my MCMC code][1], where I implement the
figures from this writeup and get a *lot* of understanding.

This is a long paper (50 pages) and I won't be able to read it all. I'm just
hoping to understand enough of the Hamiltonian Dynamics part and to get
intuition. Remember that MCMC originally came out of trying to simulate
molecules, so there's some physics and chemistry background and terminology,
which is what John Canny seems to like.  HMC originates from a 1987 paper.
Hamiltonian/Hybrid Monte Carlo. Hamiltonian is more descriptive.


## Intuition

In MCMC, we have some probability distribution that we want to sample from. If
we are doing MCMC via HMC, then *we must define* a "Hamiltonian function" with
position and momentum terms. The position terms can be thought of the real
parameters \theta, whereas the momentum terms are auxiliary variables used to
help in computing trajectories.

The *trajectories* are determined not by random walks, but by Hamiltonian
dynamics, thus getting the best of both worlds: proposals that are distant
(because we explore the state space more and don't get trapped in local modes)
and highly likely (well, we always want this).

Method alternates between two steps:

- Update momentum variables in a "simple" way.
- Update position variables with a Metropolis-Hastings test.

**Very helpful intuition**, in terms of a puck sliding along a surface. We have:

- 2-D vector representing position q.
- 2-D vector representing momentum p.
- Thus, the "states" here are 4-D vectors.
- Potential energy function (a.k.a. stored energy), proportional to height of
  puck, a function of q.
- Kinetic energy function (a.k.a. energy in a "body" due to motion), equal to
  |p|^2/(2*(mass_of_puck)), i.e. a function of p. Inversely related with
  potential energy as puck moves along surfaces.

In probabilistic language:

> In nonphysical MCMC applications of Hamiltonian dynamics, the position will
> correspond to the variables of interest. The potential energy will be minus
> the log of the probability density for these variables. Momentum variables,
> one for each position variable, will be introduced artificially.

I.e. if we want to sample \theta, then \theta is the "position variable" or the
"variable of interest" and we have to introduce momentum variables, which Neal
says are usually Gaussians (whew!).

Note: for practical implementations, we must *discretize* the differential
equations.


## Math Notes

### Preliminaries and Basics

The "system" is defined by a Hamiltonian function, H(q,p), where q and p are
both d-dimensional vectors representing position and momentum, respectively (why
the same length?).

How is he deriving Equations 5.1 an 5.2 when we haven't even defined the
function? Or are these derivatives a *requirement* for a valid Hamiltonian
function? What's the context? I am pretty sure these are definitions we have to
assume but let's see if anything I read later clarifies it.

We define the *Hamiltonian function* as:

H(q,p) := U(q) + K(p)

and usually, U(q) = -log(p(\theta | ...)) and K(p) = (1/2) p^T M^{-1} p, where M
is a symmetric, positive definite "mass" matrix, often a multiple of the
identity. Equations 5.6 and 5.7 following that make sense.

PS: their simple example has sines and cosines in their law, and I remember that
Goodfellow's tutorial on GANs does almost the same thing! Goodfellow's tutorial
said:

> Differential equations of this form have sinusoids as their set of basis
> functions of solutions.

So just remember that when I see those style of differential equations, think
sinusoids! And we can solve "boundary points" to get the coefficients. Also, for
that example, maybe think of the posterior distribution as that clockwise ring,
so the samples are correctly sampling across that clockwise ring?

Four (probably non-exhaustive?) properties of the Hamiltonian for the purposes
of making "accurate" MCMC steps:

- **Reversibility**. We have T((q(t),p(t))) = ((q(t+s),p(t+s))) and the reverse
  is true by switching signs on the derivative equations. This makes sense.
  Also, I should probably start thinking of "states" as (q(t),p(t)) tuples.

- **Conservation of the Hamiltonian**. Yes, now I should definitely think of q
  and p as functions of t, so that we can take derivatives of H with respect to
  t, which then invokes the chain rule. Got it! Ok, what does it mean to
  "conserve the Hamiltonian?" That means, if the time changes, the
  H((q(t),p(t))) value stays the same! To prove this, we have to show that H is
  a constant, i.e. that dH/dt = 0. In fact, this invariance is key to showing
  that MCMC will sample from the "correct" distribution. I have to think about
  what this means for the probabilistic interpretation.

- **Volume Preservation**, aka Liouville's Theorem. Again, *what does this
  mean*? We have points (q(t),p(t)) which lie in some space with volume. Then
  applying the function T to that means the image has the same volume. Not quite
  helpful, but he proves it by showing that the determinant of the Jacobian
  (i.e. the Jacobian that defines the dynamics) has absolute value one, and I
  remember that the determinant is another way of describing volume.

- **Symplecticness**. This defines an extra condition on the Jacobian matrix of
  the dynamics, and which also implies volume preservation.

Three out of these four are preserved even when discretizing in computer
simulations. The one that isn't is the Hamiltonian conservation.


### More Advanced Material

What does discretization mean in our context? The main continuous variable we
have discussed is the time t, so we have to deal with small time increments
\epsilon. (Isn't this the same as other continuous functions? For instance, do
we need discretization to approximate a Gaussian density function? Why is there
a special focus on discretization? I'm not sure. TODO find out.)

Two main ways to do this:

- **Euler's Method**. This looks like it has something to do with the definition
  of the derivative. Is the poor performance of raw Euler's method (see Figure
  5.1(a)) analogous to compounding errors (i.e. *covariate shift*) in imitation
  learning? A modification is possible. [TODO understand technical details]

- **The Leapfrog Method**. This has the same flavor as Euler's method, but is
  better. We perform *half-steps*. [TODO understand technical details]

Yeah, I'm pretty sure that for Figure 5.1, we should think of the Hamiltonian
function as the distribution which we want MCMC to approximate. After all, a
circle is some distribution in some dimension space!  Note also the discussion
on *shear* transformations, which preserves volume.

Both of these methods are supposed to estimate p and q after some number of time
steps. I think that makes sense. Is this related to what I think of as MCMC
sampling \theta points? TODO find out! Also, how related are these updates to
the momentum SGD?

- **Section 5** is about variations of HMC, involving splitting of the
  Hamiltonian (not what I'd like to do) and others such as the Langevin Method
  and partial momentum refresh (interesting!).


## Takeaways

Biggest takeaway: if we're doing MCMC for "nontrivial" applications, we *have*
to use Hamiltonian Dynamics in some way. I'm not sure what the alternative is
(and don't say random walk proposals!). BTW it looks like the author has already
been using Hamiltonian Dynamics for neural network models. These probably
weren't deep networks, though; is there a way to scale it up?


[1]:https://github.com/DanielTakeshi/MCMC_and_Dynamics
