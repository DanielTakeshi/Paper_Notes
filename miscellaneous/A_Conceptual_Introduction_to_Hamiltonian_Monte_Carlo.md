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

Lots of discussion over something known as the "typical set." I skimmed it for
now, but I'll read it later. 

The "guess and check" behavior of random walk Metropolis is awful. Yes, I know.

OK ... he's making an equivalence with planets and gravity. I know I'm in the
right place.


[1]:https://github.com/DanielTakeshi/MCMC_and_Dynamics
