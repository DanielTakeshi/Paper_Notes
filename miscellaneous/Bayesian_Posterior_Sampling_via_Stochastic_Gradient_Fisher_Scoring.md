# Bayesian Posterior Sampling via Stochastic Gradient Fisher Scoring

Main contribution wrt prior work: extends SGLD by arguing that due to small step
sizes, SGLD doesn't mix well (I guess that’s reasonable, as indeed the step size
has to go down for the theory to work and to avoid costly MH tests). 

Interesting: they say they tried increasing step sizes for SGLD and inserting MH
tests to correct for higher discretization error, but “initial attempts using
only a minibatch instead of the whole dataset were unsuccessful.” Maybe our work
could have helped.

Experiments involved: (1) logistic regression, (2) 3-layer f-f nets w/30 units
in each w/sigmoids, and (3) RBMs. Remember, this was 2012.

The results from LR have interesting 2-D marginal distributions and isocontours
(**ground truth from HMC**). I'm not clear on this, but are the marginals only
2D because they are doing binary classification of MNIST digits, with 7s and 9s
only?

They also plot inverse autocorrelation time per unit based on mean and
covariance of the samples.  Again, HMC is benchmarked as ground truth.

For the networks, they do the typical benchmarking with SGD. I'm not sure if I
can trust these results, since I don't know if the baseline of SGD has been
sufficiently tuned. I assume they did model averaging? In any case I really need
to investigate this in more detail.
