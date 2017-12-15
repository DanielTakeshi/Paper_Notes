# A Conceptual Introduction to Hamiltonian Monte Carlo

**Update December 2017**: Check out [my MCMC code][1], where I implement the
figures from this writeup and get a *lot* of understanding.

This is an arXiv paper that seems similar to Radford Neal's influential 2011
reivew, though perhaps this one has a slight more rigor to it. The reason why
Betancourt wrote this is that the math on HMC is due to **differential
geometry**, which is too advanced for most of us. :-) Hence he builds this arXiv
paper (about the same length as Neal's review) using "basic" math. Which means
he will still gloss over a lot of details and frustrate me to no end, but I will
do my best.

Philosophy:

> This assumption [something about a function being uniform-like] implies that
> the variation in the integrand is dominated by the target density, and hence
> we should consider the neighborhood around the mode where the density is
> maximized. This intuition is consistent with the many statistical methods that
> utilize the mode, such as maximum likelihood estimators and Laplace
> approximations, although conflicts with our desire to avoid the specific
> details of the target density. Indeed, this intuition is fatally naive as it
> misses a critical detail.

Betancourt argues that there's an enormous misconception we have about *volume*
versus *density*. **Figure 1** has some great intuition on this, about what
happens to volume within versus outside a region when we go to higher
dimensions; the ratio differs *substantially*.

Lots of discussion over the **typical set**. It is intuitive. The
Metropolis-Hastings ensures that samples don't veer too far from it. The "guess
and check" behavior of random walk Metropolis is awful. Yes, I know. How do we
transition to HMC? We have to *glide along the typical set* using the gradient,
but the gradient itself *is not enough*:

> The gradient of the target probability density function encodes information
> about the geometry of the typical set, but not enough to guide us through the
> typical set by itself. Following along the gradient instead pulls us away from
> the typical set and towards the mode of the target density. In order to
> generate motion through the typical set we need to introduce additional
> structure that carefully twists the gradient into alignment with the typical
> set.

The "additional structure" that we need comes from differential geometry, but
that's too hard, so he makes an equivalence with planets and gravity. Huh,
that's weird, but cool:

> When we introduce exactly the right amount of momentum to the physical system,
> the equations describing the evolution of the satellite define a vector field
> aligned with the orbit. The subsequent evolution of the system will then trace
> out orbital trajectories.

We can make this analogy because exploring across a probabilistic system is
mathematically equivalent to exploring across some physical system of particles.

I understand and enjoy the discussion that derives HMC. Just one quick question:
what does it precisely mean when "the canonical density `\pi(q,p)` does not
depend on a particular choice of parameterization"?

Figure 20:

> Every Hamiltonian Markov transition is comprised of a random lift from the
> target parameter space onto phase space (light red), a deterministic
> Hamiltonian trajectory through phase space (dark red), and a projection back
> down to the target parameter space (light red).

Yes, indeed I know the dynamics are deterministic. Also, every Hamiltonian
*trajectory* is confined to a fixed energy level, at least in theory. What can
change the energy level, of course, is the momentum resampling!

I also like the phase space (q and p) versus target distribution (just q).

(For now, skipping over the part about how to derive an optimal kinetic energy
function ...)

How to implement HMC in practice? The main issue is how to construct Hamiltonian
trajectories themselves! For those we need *symplectic integrators*.

> Symplectic integrators are powerful because the numerical trajectories they
> generate exactly preserve phase space volume, just like the Hamiltonian
> trajectories they are approximating. This incompressibility limits how much
> the error in the numerical trajectory can deviate from the energy of the exact
> trajectory. Consequently, the numerical trajectories cannot drift away from
> the exact energy level set, instead oscillating near it even for long
> integration times (Figure 28).

Section 5.2: *finally*, an actual explanation for why flipping the sign of the
momentum makes things reversible!! Figure 30 makes sense because given the
dynamics of Hamiltonian trajectories, given the state at time L, we can't
continue forward with the dynamics for L more steps and get back to where we
started, because the values are continuous so technically we have measure 0
probability. To fix this, flip the sign of the momentum at L steps.

[1]:https://github.com/DanielTakeshi/MCMC_and_Dynamics
