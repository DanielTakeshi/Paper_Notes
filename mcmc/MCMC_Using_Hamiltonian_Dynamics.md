# MCMC Using Hamiltonian Dynamics

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

For practical implementations, we must *discretize* the differential equations.
What does this mean? TODO


## Math Notes

The "system" is defined by a Hamiltonian function, H(q,p), where q and p are
both d-dimensional vectors representing position and momentum, respectively (why
the same length?).

How is he deriving Equations 5.1 an 5.2 when we haven't even defined the
function? Or are these derivatives a *requirement* for a valid Hamiltonian
function? What's the context? TODO find out!

Now it gets clearer. We define the function as:

H(q,p) := U(q) + K(p)

and usually, U(q) = -log(p(\theta | ...)) and K(p) = (1/2) p^T M^{-1} p, where M
is a symmetric, positive definite "mass" matrix, often a multiple of the
identity.


## Takeaways

Biggest takeaway: if we're doing MCMC for "nontrivial" applications, we *have*
to use Hamiltonian Dynamics in some way. I'm not sure what the alternative is
(and don't say random walk proposals!). BTW it looks like the author has already
been using Hamiltonian Dynamics for neural network models. These probably
weren't deep networks, though; is there a way to scale it up?
