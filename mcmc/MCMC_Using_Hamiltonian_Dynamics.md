# MCMC Using Hamiltonian Dynamics

This is a long paper (50 pages) and I won't be able to read it all. I'm just
hoping to understand enough of the Hamiltonian Dynamics part and to get
intuition. Remember that MCMC originally came out of trying to simulate
molecules, so there's some physics and chemistry background and terminology,
which is what John Canny seems to like.

HMC originates from a 1987 paper. Hamiltonian/Hybrid Monte Carlo. Hamiltonian is
more descriptive.


## Intuition

In MCMC, we have some probability distribution that we want to sample from. If
we are doing MCMC via HMC, then we must define a "Hamiltonian function" with
position and momentum terms. The position terms can be thought of the real
parameters \theta, whereas the momentum terms are ... something else.

The *trajectories* are determined not by random walks, but by Hamiltonian
dynamics, thus getting the best of both worlds: proposals that are distant
(because we explore the state space more and don't get trapped in local modes)
and highly likely (well, we always want this).
