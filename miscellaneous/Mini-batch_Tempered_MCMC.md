# Mini-batch Tempered MCMC

It considers the scalable MCMC setup that I'm intimately familiar with. Several
points from the paper:

- Most scalable methods are either (a) minibatch methods to approximate MH, or
  (b) use a minibatch of data to approximate the gradient. Unfortunately, our
  paper is not cited. :-( But our method would fall under group (a).

- They argue that the naive minibatch approach to estimating the MH ratio leads
  to the target posterior but with a temperature applied. I can *kiiind* of
  see this, but I'm worrying if this contradicts with the results from Section
  6.1 of Bardenet et al., 2017.

- They demonstrate experiments using that same mixture of Gaussians from Welling
  & Teh, 2011 (heh, did they read our paper?), and then Bayesian Logistic
  Regression, and then Bayesian Neural Networks. The BNNs are on MNIST
  classification, and I should probably read this over in depth.
