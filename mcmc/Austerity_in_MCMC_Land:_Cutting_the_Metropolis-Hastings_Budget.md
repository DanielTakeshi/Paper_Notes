# Austerity in MCMC Land: Cutting the Metropolis-Hastings Budget

Note: I have already discussed some of the material [in a blog post](https://danieltakeshi.github.io/2016-06-19-some-recent-results-on-minibatch-markov-chain-monte-carlo-methods/). This summary will be a bit short here as most of what I know is written elsewhere and I'd rather not repeat myself too much.

General idea: this paper (like SGLD) is about using minibatches for Bayesian MCMC. This time, we're not using "dynamics" but simply sequential hypothesis testing. They start with a small minibatch size and test whether they should accept/reject with high confidence (compared to the "correct" answer which we would get from the full batch if we wanted to use all of the data). If they can't say the answer with confidence, increase the minibatch size and try again.


## Introductory Material

The introduction is pretty clear. MCMC algorithms use an MH test (well, Gibbs sampling skips it because the MH test always accepts), but that test is expensive for big data Bayesian posterior inference, etc. An important passage:

> However, given a finite amount of computational time, it is not at all clear whether the strategy of retaining few unbiased samples and accepting an error dominated by variance is optimal. Perhaps, by decreasing the bias more slowly we could sample faster and thus reduce variance faster? In this paper we illustrate this effect by cutting the computational budget of the MH accept/reject step.

One confusion I often have is what we are *directly* talking about when we mean bias and variance. The fact that MCMC tests are unbiased I think means if there is a function of the posterior, e.g., E f(theta) (I'm putting this in expectations, is that correct? Or should it not be there?), then when we sample it, the function f based on the *sampled* \theta's, (1/n) * \sum_t f(\theta) will have expected value equal to E f(\theta). Is that the correct line of reasoning? I'm pretty sure that when we talk about "bias" we're talking about, for example, what is being discussed in Section 2 of this paper (shortly after Equation 1).

Also, this is a bit funny:

> Ironically, the reason MCMC methods are so slow is that they are designed to be unbiased.


## The Algorithm

They reformulate the test to make it equivalent but easier to manage with the incrementally increasing minibatch size idea. (I've written the math elsewhere.)

They have dynamic programming there but I don't think we figured out how that works.


## Experiments

Not much to say here. I often ran this algorithm but with \epsilon high (equal to one), meaning that it only uses the first minibatch, and in fact I'd get good performance, but I guess that is not the purpose or spirit of the algorithm. If their test always decides on the accept/reject after the first minibatch, then it's just stochastic gradient descent, not MCMC, right?


## My Thoughts and Takeaways

It's a simple algorithm and I think I understand it sufficiently well. Some of the later material in the paper is a bit hard for me to follow but *the actual algorithm* is OK with me. And I also have code for it! Let's test it out. I wish I had statistics in my brain.
