# Robust Imitation of Diverse Behaviors

Basic idea: for imitation learning, combine strengths of generative models
(GANs in the form of GAIL, VAEs, and autoregressive models) while addressing
individual weaknesses, so that the resulting imitated policies have lots of
*variety*, in addition to being "expert-like".


## The Algorithm/Architecture

(Only briefly for now, I read the paper but no detailed notes yet... since I
need to review conditional GANs.)

- Use a bidirectional LSTM as the encoder for the VAE. Thus, the VAE takes in a
  **demonstration trajectory** and embeds that into a vector; so it is
  conditioned on that demonstration, which is not the case with GANs in general,
  because the Generator never has access to the true data. 
  
- Each timestep, their VAE maps to a policy and a state predictor, which is
  presumably modeling the dynamics of the system. Also, note that this is per
  time so the LSTM produces a hidden state vector ("latent" state in VAE-speak)
  at *each* time.

- Recall the standard VAE architecture:

  ```
  (input) -- encoder -- (latent_state) -- decoder -- (output)
  ```

  But here, there are TWO decoders, not one. Today, it pays to be creative with
  your neural network architectures.

- Interesting quote, partially explaining why they use VAEs:

  > Like GANs, autoregressive models produce sharp and at times realistic image
  > samples [39], but they tend to be slow to sample from and unlike VAEs do not
  > immediately provide a latent vector representation of the data. This is why we
  > used VAEs to learn representations of demonstration trajectories.

- A **conditional GAN** is used for the "GAIL" step. GAIL is what actually
  generates the policy, but it is **conditioned/initialized** based on the VAE
  latent state. Recall from Stanford CS 231n Lecture 13, VAEs can produce useful
  feature representations via the latent variable! This is the more confusing
  part of their architecture, but I think I'm getting it. We still have the
  expert trajectories, but these get *mapped* to z from the VAE, upon which we
  condition. The conditioning, they argue, will *remove* the multi-modal nature
  of the problem, so we can still optimize the GAIL using the standard reward
  metric. There are an arbitrary amount of reward functions as they're all
  conditioned on z.

  - Hmm ... they don't use the heuristic objective for the Generator?

  - It also seems like they "throw away" the decoding part of the VAE, because
    Algorithm 1 suggests that all you need is the encoder (to get z) and then
    the policy isn't the VAE decoder but actually the GAIL part. But that's not
    entirely true, they structure their GAIL policy so that it's a Gaussian
    conditioned on the VAE policy. This is interesting but I have no idea how
    they can justify this, other than saying "it works". And what about their
    autoregressive model?

- Unfortunately I don't think I have a complete-enough understanding of how they
  justify their objective mathematically, but it seems somewhat clear...


## Experiments

- All are based on simulation via MuJoCo, but *not* the usual environments I'm
  aware of, because their simulations need to show *diversity* in training
  policies. The experiments use: a 9 DoF robotic arm, a 9 DoF planar walker, and
  a 62 DoF complex humanoid (56-actuated joint angles, and a freely translating
  and rotating 3d root joint).

- Roughly speaking, GAIL policies are more robust than those of VAE policies,
  but GAIL policy *diversity* is less than that of VAE policy diversity. This is
  certainly interesting, but I am not sure why.

- I find it interesting that evaluation is based on the diversity of policies.
  In GAIL, as we know, it was easy to evaluate imitation quality based on game
  score. Experiments should definitely be chosen in such a way that they have
  interpretable metrics.
