# Prioritized Experience Replay

## My Initial Thoughts

This is one of the few papers where I think I get the idea of it even before
reading the paper, since it was *literally* mentioned as possible future work
extension by the original DQN papers from NIPS and NATURE. And, also, it's a
fairly obvious extension (though to be fair there's still numerous technical
details to cover, and implementing it can be challenging). In DQN, we use
experience replay for increased sample efficiency and to break correlations
among consecutive samples by sampling from a long history of samples.

But rather than sample at random from our dataset of the past, say, 10 million
samples, let's keep in the dataset only the *hardest* elements. By "elements"
here in the dataset, I mean the (s,a,r,s') data points. [As I covered in a
related blog post, these (s,a,r,s') samples are all we need to define a loss
function.](https://danieltakeshi.github.io/2016/12/01/going-deeper-into-reinforcement-learning-understanding-dqn/)
So the key idea is to pick the (s,a,r,s') samples that would be *most
beneficial* for training. Analogy: think of Support Vector Machines which only
care about the points closest to the margin in hyperspace. Those points are the
crucial values, and the others don't matter nearly as much.

Specifically, their contribution is:

> In particular, we propose to more frequently replay transitions with high
> expected learning progress, as measured by the magnitude of their
> temporal-difference (TD) error. This prioritization can lead to a loss of
> diversity, which we alleviate with stochastic prioritization, and introduce
> bias, which we correct with importance sampling.

The result is that:

> DQN with prioritized experience replay achieves a new state-of-the-art,
> outperforming DQN with uniform replay on 41 out of 49 games.

I believe this outperformed the Double DQN which was previously the state of the
art after the NATURE DQN paper. However, at this point I think A3C has surpassed
this, and then A3C was later surpassed by some other papers published and/or
released on November/December 2016. The field moves by fast!

## More Paper and Algorithmic Details

Intuitively, it makes sense to use temporal difference error as the metric by
which we judge the usefulness of a (s,a,r,s') sample. That's how we measure the
loss function, after all! It's a proxy for how much the agent will be able to
learn from that transition. Certainly, some samples will be *noisy* but I assume
that their (a) stochastic prioritization and (b) importance sampling corrections
will serve to prevent overfitting. (They present alternatives in Appendix A in
case the TD error is too noisy.)  EDIT: indeed, they mention overfitting in
**Section 3.3**, looks like I got it. =-) Section 3.3 is the stochastic
prioritization section.

They assume we have a fixed dataset and the main problem concerns *sampling*
from that dataset (i.e. they do not concern themselves with *storing* the
samples). That's fine, I assume we just keep the previous 10 million samples and
deal with those.

Motivating example: blind cliffwalk. Hmmm ... I think this makes sense.
Definitely there's a huge gap in performance, though this example probably
excessively stacks the deck in favor of the gap. It's OK though! They test with
(1) naive experience replay, (2) prioritized experience replay, and (3) an
oracle which can measure the exact impact on the loss function. (3) is best but
(2) is still much better than (1).

**Implementation**: use a *binary heap*. Yes, it's O(1) to get the max element
(i.e. the max TD error). See [CS 61B, Lecture 25 notes, from Jonathan
Shewchuk's class in 2014](https://people.eecs.berkeley.edu/~jrs/61b/lec/25.pdf),
where binary heaps are the data structure used to implement priority queues.

Wait ... they also describe later how to sample efficiently, clearly arguing
that the complexity cannot depend on N. They're not going to be sampling the max
element always, so I guess the binary heap is just for initial intuition. It's
helpful to me, at least.

**Stochastic Prioritization**. Their explanation and judgment make perfect sense
to me:

> To overcome these issues, we introduce a stochastic sampling method that
> interpolates between pure greedy prioritization and uniform random sampling.
> We ensure that the probability of being sampled is monotonic in a transition's
> priority, while guaranteeing a non-zero probability even for the
> lowest-priority transition.

I should think about if I can use interpolation in my own work.

**Annealing the Bias**. Ah, I get it. Vanilla DQN, since it uses stochastic
gradient updates, relies on its samples (for the gradient computation) coming
from the same distribution as the expectation. But this is biased with
prioritized experience replay. Hence the bias should be annealed using
*weighted importance sampling*. (I forgot the details on IS and weighted IS ...
I should review this later if it becomes important to me, pun not intended.)
There are some interesting conjectures about why weighted IS should work with
Q-learning convergence. See Section 3.4 for details, e.g. they argue (and I
agree) that the unbiased nature of samples is most important at the *end* of
Q-learning.

**Algorithm 1**: Double DQN with prioritized experience replay. Note the
*Double* here. I thought this paper didn't use Double DQN but "vanilla" DQN.
Wow, I guess I'm wrong. So this is certainly going to be an improvement over
"vanilla *Double* DQN". Yeah, so many "vanilla"s going around here ...

See Figure 3 for their results. It takes a bit of time to absorb since it's not
the raw scores. They have three main algorithms: vanilla DQN, vanilla Double
DQN, and their Double DQN with prioritized experience replay. Technically, they
have two variants of their algorithm: *proportional* and *rank-based*, which
differ based on the method of sampling from the priority queue (again, they're
not always taking the maximum element). They note that both variants had similar
performance in practice so the difference is not critical.

They also have *learning speed* results. They argue that learning is *twice as
fast* as compared to the baselines. Wow! There's a lot more, fortunately it's in
the Appendix and for now I'll skip them as there's no pressing need for me to
get these numbers.

**Section 6: Extensions**. Wow, there's a lot of interesting stuff there. But
I'm reminded of Robert Gordon's *The Rise and Fall of American Growth: The US
Standard of Living Since the Civil War* book; these extensions would improve DQN
but the original prioritized experience replay (as in, this paper!) is going to
nonetheless be the most important related improvement. In other words, there
will be diminishing returns. Ugh. But maybe think about how this can be extended
to other work. I mean, prioritizing things is pretty common!
 
## Final Thoughts

Wow, I'm impressed by this paper! =)  There's *so many* experiments and lots of
discussion.

I wish I could write something like this. =(
