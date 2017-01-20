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
Hmmm ...

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
  sense that there's more variance, which is intuitive.
- **Central Limit Theorem stuff**: TODO
- **New contribution**: TODO
- **Confidence samplers**: TODO

Re-read this once I have more energy!

Section 7, an improved confidence sampler. TODO
