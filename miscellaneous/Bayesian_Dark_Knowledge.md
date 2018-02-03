# Bayesian Dark Knowledge

TL;DR: proposes “distilled SGLD” where SGLD is “packed into one neural net”  for
computational savings.

Their most complicated experiment is with MNIST classification, and they show
that SGLD is superior with SGD, and for once they say that they run 10 trials
(not just one) though unfortunately they don’t describe hyperparameter tuning in
detail.  Well, at least this is better than related work.

Their distilled SGLD is slightly worse wrt accuracy (than SGLD, maybe SGD not
sure) but their point was to just sacrifice a small amount of accuracy
and get LOTS of computational and memory savings. Uses thinning and burn-in.
Unfortunately, no code seems to be available from them BUT it’s on MXNet. 

I don’t know the stopping condition and I assume they averaged the predictions
(ensembles) for SGLD at least, and perhaps the other methods. It would really be
helpful to have all this information clearly laid out in an appendix.
