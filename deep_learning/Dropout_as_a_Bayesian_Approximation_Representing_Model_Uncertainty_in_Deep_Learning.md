# Dropout as a Bayesian Approximation: Representing Model Uncertainty in Deep Learning

A popular paper!

> We develop a new theoretical framework casting dropout training in deep neural
> networks (NNs) as approximate Bayesian inference in deep Gaussian processes. 

The figures Yarin Gal uses a lot (and which also appear in his blog post and his
PhD thesis) show uncertainty estimates are for 1-D toy examples. 

For MNIST, here’s what’s cool: they continuously rotate an image (see Fig. 4)
and plot (“scatter”) 100 different runs — good! — of the last-layer predictions.
Fig. 4 is a bit hard to process but I think I get it.  They do not optimize for
classification performance but instead demonstrate the model’s uncertainty. (For
optimizing performance, they use UCI data and RMSE & log likelihood.) They have
RL using Andrej Karpathy’s JavaScript code and replace ε-greedy with Thompson
sampling for exploration, but the metric is still average reward.

The paper says looking at a DNN’s prediction probability vector is not the same
thing as “confidence” or “certainty” in a Bayesian sense. That's important!
