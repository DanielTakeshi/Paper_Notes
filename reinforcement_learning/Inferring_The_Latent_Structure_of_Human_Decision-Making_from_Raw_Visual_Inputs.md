# Inferring the Latent Structure of Human Decision-Making from Raw Visual Inputs

This is in NIPS 2017; these notes are from the March 2017 arXiv version, which
might be out of date.

This builds upon GAIL by letting us infer latent structure. I.e., expert
policies are assumed to be first conditioned on some latent variable. This makes
sense in humans. Expert trajectories for humans will be conditioned on the
latent variable that represents the human, as some drive faster, more recklessly
than others, etc.

They propose InfoGAIL, which uses a similar objective as InfoGAN (hence the
naming). Thus their objective has changed to take into account mutual
information. But that isn't entirely their contribution, as otherwise it is not
substantial enough. Their contributions are:

- (As stated earlier) InfoGAIL can infer latent structure and distinguish
  between different modes of the expert.

- They can take in **raw pixels** as input; recall in the GAIL paper, the most
  complicated input was 376-dimensional, i.e. the humanoid.

- They demonstrate excellent, *interpretable* performance in autonomous driving
  domains.

- (They didn't list this, but I feel it's important enough.) Their improved
  optimizations they suggest, in **Section 4**. These make sense, and I'll
  definitely try to take a look at their code! Probably the easiest one to
  implement is to initialize our policy with behavioral cloning, and then to
  make use of transfer learning. Also, they use some form of memory but it's
  very fixed, they remember velocity and related stuff (see Section 5.2).

The experiments are all entirely car simulation (via TORCS). This is good but I
can see how this might be a drawback; GAIL is not featured very much except to
show that it cannot learn to distinguish between two types of expert
trajectories. And having it limited to this domain might make it hard to
determine InfoGAIL's generalization. **Update**: after reading their
experiments, I am unfortunately not entirely convinced since they seem to have
only run this a limited number of trials. Surprisingly, Table 2 doesn't even
report *how many trials they ran*; it just has the average. I assume the NIPS
2017 version will have more results.

Nonetheless, I enjoyed reading this paper. I need to (a) always use TRPO or at
least some KL-constrained step, (b) always initialize with behavioral cloning,
as there seems to be no reason not to, (c) figure out how to use some of their
reward augmentation tricks, and (d) do their other tricks.

Question: why do they assume their policies have fixed standard deviations.
Gaussian policies, I agree with, but having fixed standard deviation?
Intuitively I can see how this might not matter that much since getting the mean
right is almost the entire battle, but then why did we model the standard
deviation as a variable in CS 294-112?
