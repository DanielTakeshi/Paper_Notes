# Reinforcement Learning with Unsupervised Auxiliary Tasks 

This paper augments the normal reward in an environment with several other
training signals from the "sensorimotor stream" in RL environments. Their RL
agent, UNREAL, can learn separate policies for maximizing these (pseudo)-reward
functions, and ideally will be able to *control* the sensorimotor stream to suit
its objectives.

Key points:

- Augments A3C with auxiliary control and reward tasks. Uses a CNN+LSTM stem for
  the agent, which takes in a sequence of images from the environment. The CNN
  converts the images to some representation. Then, a sequence of these are fed
  into an LSTM, which produces some output at each time, which corresponds to
  the policy (and a second output which is the value function). This must be the
  case when John Schulman said that people often share parameters with the value
  function and the policy function.

- **Pixel Control**: changes in pixels often correspond to important events, so
  they train to maximally change the pixels. (I'm surprised this works without
  directly taking reward into account, but they have separate reward prediction
  components.)

- **Reward Prediction**: given the previous few states, try to predict the
  reward. If it's sparse, this is challenging, so they actually bias their
  distribution by emphasizing more of the high-reward states. I agree with them
  that it's a specialized version of prioritized experience replay.

- **Value Function Replay**: predicts the n-step return from current state. I
  think this augments the value function predictor which is normally part of
  policy gradient methods.

- The loss function is the typical A3C loss, **plus** the (weighted) losses of
  the control, reward, and VF replay losses, so there are four major terms to
  add up. 
  
  Then there are trained jointly, or can be trained separately using slightly
  different methods. I can **kind of** see how this works at a high level, but
  of course for details I'd have to try writing this up myself. Gulp, there's a
  lot of moving parts to this, and it's frankly amazing that this all works.
  Judging from [the OpenReview][2] it seems like people have similar questions.

- UNREAL outperforms state-of-the-art on Atari and does well in challenging
  Labyrinth tasks. (Is it still state-of-the-art on Atari, though?)

  - It scores ZERO points on Enduro?!? I'm surprised.

  - The Labyrinth results are impressive.

Thoughts:

- It reminds me of the paper Vladlen Koltun presented to us in class, since that
  one is also about cleverly modifying or changing the reward structure. It also
  takes advantage of the richness offered by the sensorimotor stream versus the
  sparseness of the value function.

- Wow, lots of moving parts. And their ablation studies show that it's needed.
  What else can we do to improve this that hopefully doesn't require
  increasingly complicated models?

- How does this compare to DeepMind's "Rainbow" model?



[2]:https://openreview.net/forum?id=SJ6yPD5xg&noteId=SJ6yPD5xg
