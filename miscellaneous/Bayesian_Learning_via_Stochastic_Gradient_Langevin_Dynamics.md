# Bayesian Learning via Stochastic Gradient Langevin Dynamics

Note: I have already discussed some of the material [in a blog post](https://danieltakeshi.github.io/2016-06-19-some-recent-results-on-minibatch-markov-chain-monte-carlo-methods/).

General idea: this paper is about the problem of Bayesian MCMC methods, which have grown out of favor compared to optimization methods since it's harder to apply minibatch techniques with MCMC methods (which require the full posterior for "exactness," which technically means detailed balance in our context). They use updates that are similar to stochastic gradient descent, but which inject the correct amount of noise in them so that the resulting set of samples "moves around enough" to approximate a *distribution* rather than a single point estimate.


## Introductory Material

The key passage:

> One class of methods "left-behind" by the recent advances in large scale machine learning are the Bayesian methods. This has partially to do with the negative results in Bayesian online parameter estimation (Andrieu et al., 1999), but also the fact that each iteration of typical Markov chain Monte Carlo (MCMC) algorithms requires computations over the whole dataset.

You know, the "left behind" stuff are like Trump voters, and the "optimization methods" which can easily use stochastic gradient descent are like the "current" people.

OK, this is the *real* key passage:

> Our method combines Robbins-Monro type algorithms which stochastically optimize a likelihood, with Langevin dynamics which injects noise into the parameter updates in such a way that the trajectory of the parameters will converge to the full posterior distribution rather than just the maximum a posteriori mode.

There's also discussion of this algorithm being a "two-phase" algorithm, the first being like SGD and the second being like Bayesian MCMC.


## The Algorithm

Their key idea is to use Equation 4, which combines the best aspects of Equations 2 and 3.

I didn't quite get the entirety of the argument in Section 3 about why the \theta_t samples collectively approach samples from the posterior distribution as t -> infinity. The other way to frame their argument here is that they're showing Equation 4 approaches Equation 3, where Equation 3 is the "exact" one that they wish to simulate. Also, using Equation 3 means there's no need for rejection tests since the (Langevin) dynamics will accurately approximate the "system," i.e. the posterior.

For their analysis, they use Equation 6, which indeed is a zero-mean random variable, whose variance depends on the minibatch in question. The mean of the minibatch is \sum_{i=1}^N \nabla \log p(x_i | \theta) but that gets subtracted away, hence zero mean.

OK, I understand Equation 7, yes as t -> infinity, it approaches Langevin dynamics exactness since \epsilon_t^2 is dominated by \epsilon_t without the square. And Langevin dynamics (IF it's exact) means we can ignore the expensive MH test.

OK, that's good, we know it approaches Langevin dynamics. But the *next* step is to show that the sequence of samples now converges to the desired posterior. (Why are these claims not necessarily true together? I *think* it's because the previous claim assumes large t, but we start with t=1 and must actually *get* to that point, perhaps somewhere earlier before t is large, we lose the ability to arrive there?) They show this part via showing that a subsequence converges, arguing that the total injected noise between two points in the subsequence is O(sqrt(\epsilon_0)), hmm ... well if we have zero mean Gaussians with variance \epsilon_0 the square has expectation \epsilon_0 so the norm (which applies the square root) will mean sqrt(\epsilon_0), I think. Then this noise dominates the extra noise from the stochastic gradient part. (I'm still not sure at a high level how this works, I suppose it's supposed to be intuitive ...)

They claim:

> In the initial phase the stochastic gradient noise will dominate and the algorithm will imitate an efficient stochastic gradient ascent algorithm. In the later phase the injected noise will dominate, so the algorithm will imitate a Langevin dynamics MH algorithm, and the algorithm will transition smoothly between the two.

Then Section 4 is dedicated to providing the transition point, when we should start collecting samples only *after* the algorith has entered its posterior sampling phase, i.e. when it "becomes Langevin dynamics."


## Experiments

I have some extensive experience trying to get their first experiment to work. SGLD works (i.e. I verified their results), and so does our method. I'd like to modify our code for that experiment to make it more robust, though, but I never find the time. =(

I haven't looked at the second one in much detail.

I did not have time to digest the ICA experiments and it is unlikely that I ever will.


## My Thoughts and Takeaways

For the most part, this paper is clear to me (except the ICA part but that's not my concern). I will try to think about how I can use similar techniques. For our MCMC paper (on arXiv as of this writing) we didn't directly use SGLD but I think there's a way to combine this technique with ours.
