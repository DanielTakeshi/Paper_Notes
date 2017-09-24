# Inferring the Latent Structure of Human Decision-Making from Raw Visual Inputs

This paper is in NIPS 2017. These notes are from the March 2017 arXiv version,
which might be out of date.

This paper is another one which builds upon GAIL. The main thing here is that we
can infer latent structure. This means that expert policies are assumed to be
first conditioned on some latent variable. This makes sense in humans. Expert
trajectories for humans will be conditioned on the latent variable that
represents the human, as some drive faster, more recklessly than others, etc.

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
  implement is to initialize our policy with behavioral cloning. Yeah, there
  seems to be no real downside to that.

The experiments are all entirely car simulation (via TORCS). This is good but I
can see how this might be a drawback; GAIL is not featured very much except to
show that it cannot learn to distinguish between two types of expert
trajectories.

I enjoyed reading this paper. I need to (a) always use TRPO or at least some
KL-constrained step, (b) always initialize with behavioral cloning, as there
seems to be no reason not to, (c) figure out how to use some of their reward
augmentation tricks, and (d) do their other tricks. But I have no idea how to
implement WGANs and how to *correctly* utilize a replay buffer, at least in the
context of GAIL and imitation learning.
