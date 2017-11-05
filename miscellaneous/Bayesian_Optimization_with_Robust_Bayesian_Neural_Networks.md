# Bayesian Optimization with Robust Bayesian Neural Networks

This paper is about the problem of hyperparameter optimization, which they argue
is normally solved by Bayesian optimization based on Gaussian processes. They
extend this BO procedure to use neural networks based on stochastic gradient
HMC. (I am unfortunately not familiar with the literature on hyperparameter
optimization, besides Bengio's paper on random search over grid search.)

Are they using a neural network to optimize the hyperparameters of *another*
neural network? No, I don't think so ... I need to read the appendices in more
detail, if this paper is going to be relevant for me. 

Can Bayesian optimization be used for the actual network parameters, instead of
hyperparameters?

What I like about this paper is how they actually use DDPG and can benchmark on
deep RL tasks. Perhaps there is some nontrivial advantage from the Bayesian
perspective applied to deep RL.

See some discussion over at Andrew Gelman's blog.

http://andrewgelman.com/2016/12/09/fundamental-incompatibility-scalable-hamiltonian-monte-carlo-naive-data-subsampling/

And they've got documented code!!

http://automl.github.io/RoBO/
