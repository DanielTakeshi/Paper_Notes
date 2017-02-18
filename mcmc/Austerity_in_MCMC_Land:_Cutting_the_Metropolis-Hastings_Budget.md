# Austerity in MCMC Land: Cutting the Metropolis-Hastings Budget

Note: I have already discussed some of the material [in a blog
post](https://danieltakeshi.github.io/2016-06-19-some-recent-results-on-minibatch-markov-chain-monte-carlo-methods/).
This summary will be a bit short here as most of what I know is written
elsewhere and I'd rather not repeat myself too much.

General idea: this paper (like SGLD) is about using minibatches for Bayesian
MCMC. This time, we're not using "dynamics" but simply sequential hypothesis
testing. They start with a small minibatch size and test whether they should
accept/reject with high confidence (compared to the "correct" answer which we
would get from the full batch if we wanted to use all of the data). If they
can't say the answer with confidence, increase the minibatch size and try again.


## Introductory Material

The introduction is pretty clear. MCMC algorithms use an MH test (well, Gibbs
sampling skips it because the MH test always accepts), but that test is
expensive for big data Bayesian posterior inference, etc. An important passage:

> However, given a finite amount of computational time, it is not at all clear
> whether the strategy of retaining few unbiased samples and accepting an error
> dominated by variance is optimal. Perhaps, by decreasing the bias more slowly
> we could sample faster and thus reduce variance faster? In this paper we
> illustrate this effect by cutting the computational budget of the MH
> accept/reject step.

One confusion I often have is what we are *directly* talking about when we mean
bias and variance. The fact that MCMC tests are unbiased I think means if there
is a function of the posterior, e.g., E f(theta) (I'm putting this in
expectations, is that correct? Or should it not be there?), then when we sample
it, the function f based on the *sampled* \theta's, (1/n) * \sum_t f(\theta)
will have expected value equal to E f(\theta). Is that the correct line of
reasoning? I'm pretty sure that when we talk about "bias" we're talking about,
for example, what is being discussed in Section 2 of this paper (shortly after
Equation 1).

Also, this is a bit funny:

> Ironically, the reason MCMC methods are so slow is that they are designed to
> be unbiased.


## The Algorithm

They reformulate the test to make it equivalent but easier to manage with the
incrementally increasing minibatch size idea. (I've written the math elsewhere.)

What are the assumptions of their algorithm:

- It "will behave erratically if the CLT does not hold" they say. By that, I
  assume *the sampled minibatches themselves* (e.g. of 100-500 elements n out of
  the millions/billions N in the full dataset) must be Gaussian.

OK, that's great, but key question: **how do we choose m and epsilon**?
AKA the starting minibatch size and the tolerance threshold. Section 5.2 states:

> We now briefly describe how to choose the parameters of the algorithm:
> epsilon, the error of a single test and m, the mini-batch size. A very simple
> strategy we recommend is to choose m about 500 so that the Central Limit
> Theorem holds and keep epsilon as small as possible while maintaining a low
> average data usage.

Note that while they use m here, that's the starting minibatch size, and n is
what they use otherwise. They have dynamic programming there but I don't think
we figured out how that works. 

**UPDATE** never mind we have code. The purpose is to estimate the errors of the
algorithm. They want to estimate (and bound!) the *acceptance probability error*
because that will be linearly correlated with the total variation distance (see
Theorem 1).


## Some Theory

**Section 5.1** contains the main non-supplementary theory. This is supposed to
lead us to **Section 5.2** which, when *provided* with some error bound, tells
us how to be within that bound while using *as little data as possible*. This is
hard because using less data means the algorithm is more susceptible to errors.
Some important points:

- The epsilon is the error of a SINGLE test (error happens if the test picks
  something different than the full version). That is not the error of the FULL
  SEQUENCE; we need more analysis for this.  For the full error they use
  \mathcal{E} but what scale is it? Is it in [0,1]? It's not a single value,
  it's a sequence, is it normalized by the sequence length? I don't know. =(
  EDIT: See Equation 19, as well as 20. Indeed, \mathcal{E} is a sum over
  minibatches, and each component of the sum is a probability of making a
  mistake. This makes sense! =)
- Hmm ... here's *another* metric for error, total variation distance. This one
  explicitly measures the differences in posteriors, so that seems much better.
- With two assumptions, they show that the test statistic t follows a Gaussian
  random walk process. I'm not sure if these assumptions are reasonable or
  onerous, but these didn't seem to be what John thought where the strongest
  assumptions. BTW the t seems like it's a normalized Gaussian anyway in the
  limit ... which is actually one of the two assumptions here!
- They use a quantity \Delta = P_{a,e}-P_a. Don't they need absolute values?
  EDIT: never mind, they use |\Delta| later. Whew. We're not guaranteed that
  P_{a,e} >= P_a since our test may tell us to reject if we unluckily get lots
  of samples which tell us that our samples are bad. But this value for "error"
  makes sense, it's the difference between actual acceptance vs acceptance
  probability of their algorithm.

OH, *this* is what they were using dynamic programming for --- to compute the
estimated errors. They use this for **Figure 1**, which shows that for smaller
epsilon, the error is smaller (which makes sense). Also, for the *standardized
mean*, the closer it is to zero, the larger our error, which makes sense since
the current minibatch will "look very much like" the full dataset.

They defer most of this theory to **Appendix A** and then **Appendix B** and
then use **Theorem 1** to argue that the expected posterior of their method will
be close (or distance is bounded, I should say) to the actual posterior with
full data MCMC. I see, Appendix A has several assumptions, but they seem fine to
me?

Let's recap:

- \mathcal{E} = error of sequential test
- \Delta = error of acceptance probability
- Theorem 1 = error of stationary distribution

Unfortunately, I don't have intuition on the Total Variation Distance metric. =(

Again, how to choose m and epsilon?

- They say they choose m approx 500, which works well in practice and they use
  that for four experiments. So I don't think it is problematic for us to fix a
  value like m = 100. It might make the optimization problem for epsilon easier.
- But otherwise, we have to implement our own grid search algorithm??


## Experiments

Not much to say here as I never looked at their experiments in detail, because
the metric they use (risk) is not straightforward for me to use.

Personally, I often ran this algorithm but with \epsilon high (equal to one),
meaning that it only uses the first minibatch, and in fact I'd get good
performance, but I guess that is not the purpose or spirit of the algorithm. We
may be using the wrong metric. If their test always decides on the accept/reject
after the first minibatch, then it's just stochastic gradient descent, not MCMC,
right?


## My Thoughts and Takeaways

It's a simple algorithm and I think I understand it sufficiently well. Some of
the later material in the paper is a bit hard for me to follow but *the actual
algorithm* is OK with me. And I also have code for it! Let's test it out. I wish
I had statistics in my brain.
