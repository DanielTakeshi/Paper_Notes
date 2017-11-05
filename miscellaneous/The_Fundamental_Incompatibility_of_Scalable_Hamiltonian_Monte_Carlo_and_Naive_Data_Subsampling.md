# The Fundamental Incompatibility of Scalable Hamiltonian Monte Carlo and Naive Data Subsampling

Again, the motivating concept behind this paper is that with large data,
algorithms such as HMC are expensive. Thus, let's use less data (i.e.,
subsampling) each iteration, but unfortunately ...

> In this paper I demonstrate how data subsampling fundamentally compromises the
> scalability of Hamiltonian Monte Carlo.

Ouch!

This paper's conclusions are good to know. I can't say much for the theory as I
don't have the time to read it in depth. The experiments are with toy examples
so probably not worth that much for me to investigate. I was hoping to use this
paper to better understand HMC, but I think Neal's 2010 book chapter is still
the way to go for generic HMC understanding.

He argues in the conclusion:

> Unfortunately many of the problems at the frontiers of applied statistics are
> in the wide data regime, where data are sparse relative to model complexity.
> Here subsampling methods have little hope of success; we must focus our
> efforts not on modifying Hamiltonian Monte Carlo but rather on improving its
> implementation with, for example, better memory management and efficiently
> parallelized gradient calculations.

Hmm ... I think I'll actually be dealing with tall data. Bardenet et al, 2017
describe tall data as simply datasets with `n` samples where `n` is large.

See some discussion over at Andrew Gelman's blog.

http://andrewgelman.com/2016/12/09/fundamental-incompatibility-scalable-hamiltonian-monte-carlo-naive-data-subsampling/
