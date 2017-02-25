# Understanding_Deep_Learning_Requires_Rethinking_Generalization

**TL;DR: Deep neural networks easily fit random labels, and we need better ways
to analyze generalization error.**

Hmm ... I thought the first part (deep nets fitting random labels) was common
knowledge?

The first few pages are very readable. Thanks to the authors for their clarity!
But again the downside is that some of this seems "obvious," such as poor
generalization with random labels, etc. It looks like their key insight is that
we need to find ways to *analyze* this error, but traditional methods will not
suffice, I think because with fixed architecture and settings, the
generalization error can range from 0 to random chance. We want methods to
analyze generalization that can *distinguish* among these two models. From the
random labeling thing, we know this key insight: any generalization metric
independent of training data labels will not suffice for our purposes.


## Experiments

They argue that Rademacher complexity gives trivial upper bounds since the data
can be fit to match random noise.

Figure 1 shows that ... TODO


## Theoretical Construction

TODO


## Final Thoughts

This paper is on [OpenReview][1] where I can see lots of discussion. It appears
to be the most discussed paper submitted to ICLR 2017, and to have the highest
rated scores of any paper (9, 10, 10 from the three reviewers). However, on
February 22, there was a rather critical post on OpenReview, so I will be
interested in seeing what discussion comes out of that.

It's also interesting that the term *Rademacher complexity* comes here just as
I'm learning about it in STAT 210B this semester. In fact, before this semester,
I had never utilized Rademacher variables in a class (or research), ever.

Maybe I should email John Canny to see what he thinks of this paper?


[1]:deep_learning/Understanding_Deep_Learning_Requres_Rethinking_Generalization.md
