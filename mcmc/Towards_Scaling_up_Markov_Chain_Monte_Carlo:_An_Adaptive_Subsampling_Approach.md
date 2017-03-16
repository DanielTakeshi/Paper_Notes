# Towards Scaling up Markov Chain Monte Carlo: An Adaptive Subsampling Approach

This is an ICML 2014 paper that is very similar to the "Cutting the MH Budget"
paper by Korattikara et al ([some notes here][1]), since they are concerned with
the same problem of minibatch MCMC on large datasets. They use the same problem
assumption as that paper. I think the advantage of this paper is stronger theory
(and potentially better control over how much error the algorithm has). The
downside is that it's not really a minibatch MCMC because their constants for
the concentration bound have to be computed each iteration by all samples, which
defeats the purpose. There are a few special cases (e.g. logistic regression)
but I've found through personal experimentation that even in those cases, the
analytic constant is too high to be of practical usage. The sample code (I'm not
sure for the journal paper or for this one) also computes the constant by going
through all examples.

Another thing I'm thinking is that we really need "real" experiments rather than
toy logistic regression stuff. MNIST is good for a start but I want to move on
to other stuff. They use the Covtype dataset, which has 400000 training points,
but only use the first two attributes?!? So the data is embedded in a (400000x2)
matrix.

Sorry for the short set of notes; I have lots of annotated comments on the paper
offline.

[1]:https://github.com/DanielTakeshi/Paper_Notes/blob/master/mcmc/Austerity_in_MCMC_Land:_Cutting_the_Metropolis-Hastings_Budget.md
