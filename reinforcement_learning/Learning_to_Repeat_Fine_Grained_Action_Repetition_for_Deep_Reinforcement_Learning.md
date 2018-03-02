# Learning to Repeat: Fine-Grained Action Repetition for Deep Reinforcement Learning

This extends their AAAI 2017 paper on learning to repeat with the key
extensions:

- No need for fixing two hyperparameter choices r1 and r2, avoids the
  computational blow up. Simple idea: just have two nets, one the original
  policy, and the other the repetition network which outputs a vector of size
  `|W|` and which we sample from the multinomial distribution to determine how
  long to repeat. This is how to handle discrete values.

- But how to train it? Simple: just use the same loss function. For instance,
  with DDPG, they literally concatenate the output vectors from the two networks
  and that's the output, and is trained via gradients provided by the critic in
  the same exact way as normal DDPG.

- They also add fine-grained learning to repeat in A3C and TRPO. It's not
  necessarily viewed as "concatenation" in this sense, I think, though one can
  use the intuition.

- Results are much more impressive than their AAAI paper. A3C is impressive on
  Atari games, TORCS seems impressive (a 10x (!!) improvement in performance, so
  I wish there was more about this in the main paper ...) and MuJoCo appears to
  be OK but not that great; it might have been better to stick MuJoCo in the
  Appendix.

The main questions I have at this point deal with the details in the network
architectures, so I won't obsess too much.

OpenReview link: https://openreview.net/forum?id=B1GOWV5eg
