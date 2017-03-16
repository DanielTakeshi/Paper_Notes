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

- Update momentum variables (these are supposed to be "simple")
- Update position variables with a Metropolis-Hastings test.

**Very helpful intuition**, in terms of a puck sliding along a surface. We have:

- 2-D vector representing position.
- 2-D vector representing momentum.
- Thus, the "states" here are 4-D vectors.
- Potential energy function (a.k.a. stored energy), proportional to height of
  puck.
- Kinetic energy function (a.k.a. energy in a "body" due to motion), equal to
  |p|^2/2*(mass_of_puck). Inversely related with potential energy as puck moves
  along surfaces.

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

TODO


## Takeaways

Biggest takeaway: if we're doing MCMC for "nontrivial" applications, we *have*
to use Hamiltonian Dynamics in some way. I'm not sure what the alternative is
(and don't say random walk proposals!). BTW it looks like the author has already
been using Hamiltonian Dynamics for neural network models. These probably
weren't deep networks, though; is there a way to scale it up?
