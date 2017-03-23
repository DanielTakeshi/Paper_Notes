# Scalable Discrete Sampling as a Multi-Armed Bandit Problem

**Paper Contribution**: three (approximate) algorithms for sampling discrete
random variables *with lots of dependency*, i.e. think of a Bayesian network or
Markov Random Fields with lots of edges; the *lack* of edges implies conditional
independence. I'm simplifying things here, see Michael I. Jordan's note for more
details.

**Intuition**: how would *I* sample from a discrete distribution? Without
knowing anything else, I would need to compute the CDF. Then I would draw a
uniform random variable, and find the interval where that r.v. lies in, and that
corresponds to the *realization* (or more accurately, the realization *index*)
of the r.v. of interest.

(The alternative to this --- which I don't know about --- is a technique using
the *Gumbel* distribution.)

There might be some useful techniques in this paper that I can use in other
areas. After all, they say:

> This is to our knowledge the first attempt to address discrete sampling
> problem with a large number of dependencies and our work will likely
> contribute to a more complete library of scalable MCMC algorithms.


## Technical Details

TODO these notes are in progress...


## Experiment Details

TODO these notes are in progress...


## Thoughts and Takeaways

They reference a number of MCMC-related papers that I've read, and those which I
want to eventually read. However, I doubt that the MAB stuff will be useful for
me.
