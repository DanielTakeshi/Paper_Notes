# Preconditioned Stochastic Gradient Langevin Dynamics for Deep Neural Networks

They extend SGLD by, well, "preconditioning".

As in many Bayesian-related papers, they evaluate test error on MNIST,
presumably with model averaging since they describe averaging the predictive
distribution. They use burn-in and thinning, and compare against optimization-based
algorithms. 

However, results seem so close, and I don't know if they only ran for one random
seed (seems like it) and they didn't explain the stopping criteria. Also, I'm
not sure if optimization-based algorithms are run for equal length or were tuned
enough for a fair comparison.

Fortunately, there's code online. Unfortunately, it's in MATLAB, which is
surprising since the paper was likely written in 2015. Oh well, I can try it
out.
