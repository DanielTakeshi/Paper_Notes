# Large-Scale Distributed Bayesian Matrix Factorization using Stochastic Gradient MCMC

I would like to understand this paper with respect to the matrix factorization
case. I do the same with BIDMach and its SMF.scala code, though I don't really
use "anything Bayesian," i.e. I tend to set the priors to be uniform and ignore
them. I also use John Canny's specialized algorithm (which is not quite the same
as the textbook description of Alternating Least Squares).

TODO difference between this and (minibatch) Alternating Least Squares? They
make the distinction between ALS and SGD, and it looks like they're focusing on
the SGD half, but improving it by adding Bayesian stuff (hence the "MCMC" part)
to get Bayesian advantages.


## Technical Details

TODO these notes are in progress ...


## Experiment Details

They test using the Netflix dataset and the Yahoo music dataset. I know the
Netflix data so it would be interesting to see how their performance compares to
that of SMF in BIDMach.

Maybe I should be using the Yahoo dataset?


## Thoughts and Takeaways

TODO these notes are in progress ...
