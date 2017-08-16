# Tumor Localization using Automated Palpation with Gaussian Process Adaptive Sampling

This is about using palpation to figure out where an embedded tumor is located.
The tumor is modeled using stiffer tissue.

They benchmark with three algorithms, one which randomly explores, one which
uses UCB, and one which intuitively tries to explore exactly at the boundary.
Thus, it effectively directly optimizes to find the tumor boundary. Evaluation
of the proposed methods is based on symmetric difference between the predicted
tumor boundary and the actual tumor boundary.

This paper is interesting but focused on a very narrow niche, and there's only
one relatively small set of real, physical experiments (they have simulations
but even those are relatively limited).  And it's kind of obvious that the
approach that directly finds the tumor boundary will do better than the others.
It was originally submitted to ICRA 2016, then rejected, then resubmitted and
accepted to CASE 2016. I can see why it's slightly below the ICRA threshold.
