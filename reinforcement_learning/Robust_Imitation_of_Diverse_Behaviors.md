# Robust Imitation of Diverse Behaviors

Basic idea: for imitation learning, combine strengths of generative models
(GANs in the form of GAIL, VAEs, and autoregressive models) while addressing
individual weaknesses, so that the resulting imitated policies have lots of
*variety*.


## The Algorithm/Architecture

(Only briefly for now, I read the paper but no detailed notes yet... since I
need to review conditional GANs.)

- Use a bidirectional LSTM as the encoder for the VAE. Thus, the VAE takes in a
  *sequence*.

- Each timestep, the VAE maps to a policy and a state predictor (model
  predictor?).

- A *conditional* GAN is used for the "GAIL" step. GAIL is what actually
  generates the policy, but it is **conditioned/initialized** based on the VAE
  latent state. Recall from Stanford CS 231n Lecture 13, VAEs can produce
  useful feature representations via the latent variable!

## Experiments

- All are based on simulation via MuJoCo, but *not* the usual environments I'm
  aware of, because their simulations need to show *diversity* in training
  policies.

- I find it interesting that evaluation is based on the diversity of policies.
  In GAIL, as we know, it was easy to evaluate imitation quality based on game
  score. Experiments should definitely be chosen in such a way that they have
  interpretable metrics.
