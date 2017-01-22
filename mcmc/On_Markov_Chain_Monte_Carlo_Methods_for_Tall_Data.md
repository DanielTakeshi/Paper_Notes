# On Markov Chain Monte Carlo Methods for Tall Data

My goal in reading this paper is to better understand the relationship between
their contribution and John Canny's idea of our subsampling method. This is a
long paper which is a cross between a survey and some new contributions. There's
an iPython notebook that come with it, fortunately. Also, some of this is about
the "divide and conquer" way of MCMC on large data, but that's not relevant to
me, I'm only concerned about the subsampling approach(es). They even mention
later that, in limited cases, they can "break the O(n) barrier" which I
interpret to mean that in general situations, their method (and also that of
Korattikara 2014?) must use O(n) data points to get the correct posterior.

Section 2.2, the MH algorithm, yes Figure 1 is 100% clear, though with one
possible exception, they use the un-normalized posterior, but we really use the
un-normalized *log* posterior, i.e. in our code we use logs everywhere. I think
it's OK to do the same thing, if we know the log distribution of the posterior
we can obtain the true posterior by exponentiating everything.

PS: Why on earth is the posterior denoted as \pi in Equation 3?!? The \pi is
often used as the **PRIOR**.

Running examples (100,000 points for each case):

- X_i from N(0,1)
- X_i from log N(0,1), the log-normal distribution, i.e. the log of this is
  normally distributed. Or, alternatively, given Y_i points which are normally
  distributed, if we exponentiated these, we'd get a log-normal distribution
  (hence, all variables are positive!).

I see, so our **data points** x_i are all univariate. Our **parameters** are
2-D, (\mu, \sigma), and *this* is what we will be sampling, so during the
figures, the \mu is the x-axis, the \sigma is the y-axis. They use a flat prior,
but for some reason they use \log \sigma \propto 1, instead of \sigma \propto 1?
**Update**: I see in the code that they are sampling \log \sigma values. Those
are roughly zero mean since \sigma = 1 is the true value. There must be some
advantage to sampling the \log \sigma rather than \sigma^2 or \sigma directly.

Hmmm ... Figure 2 is OK. I was confused why the mean is concentrated so much at
0.005 and not 0.0, but from the notebook, when they generated 100k points, those
points had mean 0.005. Maybe that's why. **AND** I was wondering, why does 2(a)
have a tight distribution of the mean around 0.005, it doesn't put much weight
on 0.0? The answer is that the variance is *not* from that graph, the variance
itself is actually from the y-axis. So don't get confused, the histograms in the
plots (very nice, BTW, I want to try that ...) are only about how close we can
estimate the \mu and \sigma, the **actual data** will have variance as estimated
by \sigma, not by how wide the histograms are for the mean. OK. Note that Figure
2 uses *all of the data*, i.e. it's not minibatch.

Section 3 is for **Divide and Conquer Methods** and I am skipping it. Section 4
is for **Exact, Pseudo-Marginal MH** and I am also skipping it (well, I skimmed
these sections). They don't seem to be as good as expected. Section 5 includes
SGLD, which I reviewed here. There are figures here, though a bit out of order
in the jupyter notebook.

**Section 6, Approximate Subsampling Approaches**, is what I really wanted to
know. These use a subset of the data to approximate the MCMC test (specifically,
the MH portion). The paper presents several options:

- **Naive subsampling**: run the algorithm as usual, but instead, just use a
  subset of the data points. The target distribution is nontrivial to determine,
  however, and certainly isn't the original one. It's probably "wider" in the
  sense that there's more variance, which is intuitive. I have not gone through
  the details; I will do so if it is beneficial and I can find the time.

- **Central Limit Theorem stuff**: they provide two main categories, a
  *pseudo-marginal* approach and a *t-test* approach. I still don't get the
  pseudo-marginal approach. But I understand the t-test approach. Both of these
  methods rely on the CLT assumption, *specifically*, that the *average
  subsampled log likelihood ratio* (i.e. that \Lambda_t^\* term) is normally
  distributed. Intuitively, that makes sense because that term is the mean of a
  bunch of IID variables \log p(x_i | theta). However, that's assuming that the
  "p" distribution is "well-behaved". I guess if p is long-tailed, the \log p
  values may vary considerably, as Bardenet show in their 2014 paper.

  The "problem," is that we make this assumption as well. See Section 2 of our
  paper.

  By the way, I'm now thinking that if we re-do the experiments, we should try
  and use the same Normal and log-Normal distributions from this paper. It would
  mean we have more known benchmarks and in the log-Normal case we'd show that
  our stuff works even with no CLT assumptions.

- **New contribution**: the goal here is to try and obtain subsampling
  algorithms with guarantees of "correctness," but *under weaker assumptions*
  than Normality (of the \Lambda_t^\* terms, i.e. the mean of the log likelihood
  ratios). 

  The normal MCMC test has noise with the u \sim Unif[0,1]. The naive way of
  subsampled MCMC is to do the same as normal MCMC, but with a subset of data
  points, which ONLY affects the \Lambda_t^\* term. That term ALSO introduces
  noise, so they propose only using that as the noise, and simply forgetting
  about the u part (set it equal to one to make the log u disappear). Now, what
  I don't understand is how we're able to just assume we can set u=1. Why does
  this work??
  
  Again, I get the idea that the \Lambda_t^\* term provides noise, but why are
  we allowed to remove u? All right, let me just continue, I can't get bogged
  down. To simplify, we can assume flat prior and symmetric proposal, so the
  test is just \Lambda_t^*(...) > 0. That's more intuitive because if it's
  greater than 0, that really tells us the \log p() terms are HIGHER for our new
  sample, hence it BETTER EXPLAINS the distribution. That's intuitive.
  
  They do some analysis by looking at what happens with the Berry-Esseen
  inequality to try and "relate" the CDFs of the (non-Gaussian!) \Lambda_t^\*
  term along with a real Gaussian.  If the differences are close enough, then
  they're Gaussian (but then again, why not use Austerity MH then??). (This is
  the purpose of Berry-Esseen in general, I think, to compare CDFs w/Gaussian
  CDFs.) Their conclusion? The CDF of \Lambda_t^\* is very similar to a function
  which corresponds to the sigmoid function!  In addition, the target is not \pi
  but a tempered version of it, \pi^\beta.

  They actually say their approach is impractical! I'm not sure I follow
  completely but their argument is basically saying in order for temperature to
  be 1, they have to use all n samples, i.e. t must be n. Yeah, I was worried,
  any time you need temperature to be 1 it's hard.

- **Confidence samplers**: this is their 2014 paper which I've already read
  carefully, I hope. It uses all data points to come up with concentration
  bounds, which is unfortunate.

Section 7, an improved confidence sampler. I didn't read this because it looks
like an extension of their confidence sampler work and doesn't address the issue
of heavy computation for C_{\theta,\theta'}.
