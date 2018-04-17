# CASSL: Curriculum Accelerated Self-Supervised Learning

This combines curriculum learning with self-supervision, the key application
being to do CL for a higher dimensional *action* space, so the CL is to order
the *action dimensions* so that we can explore the ones that are most
independent and interact the least with the other dimensions. This is DIFFERENT
from the usual paradigm of ordering the training instances from easiest to
hardest, as done in (Bengio et al., 2009) and similar papers.

The way they determine the most important action dimensions to explore is by
sensitivity analysis. Unfortunately I don't have too much background on that but
I'd just use the same off-the-shelf code they used. We can do this with human
labels (so humans decide the order of dimensions to learn) but obviously this is
very challenging, hence:

> Instead, we use global sensitivity analysis on a dataset of physical robotic
> grasping interactions to determine this ranking. The key intuition is to
> sequentially select the dimension that is the most independent and interacts
> the least with all other dimensions, hence is easier to learn.

They argue that this makes self-supervision less data-hungry.

Potential limitations? One is that perhaps there are better ways to do
curriculum learning. Another is they used controls but it's unclear how much of
a leap it is to go from 3D to 6D for self-supervision. There might also be
better ways to do sensitivity analysis (and optimize other metrics)?

I'm not sure how useful importance sampling would be for them ... also they're
using specific cutoffs. I wonder if there are better ways to do this. For
dimensions to explore, they do epsilon-greedy with eps=0.7, and for those
already learned, they do epsilon-greedy with eps=0.15.

Note, they say:

> To the best of our knowledge, this is the first work applications of
> curriculum learning on a physical robotic task.

However, I thought curriculum learning has been in robotics for a while? (And
physical robotics, I mean.) Hmm ... let me look up the literature.

Interesting paper!
