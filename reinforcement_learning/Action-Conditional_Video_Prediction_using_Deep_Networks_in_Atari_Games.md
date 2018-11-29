# Action-Conditional Video Prediction using Deep Networks in Atari Games

**Main idea**: learn function `f: x(1),x(2),...,x(t),a(t) --> x(t+1)` to predict
the next frame given sequence of previous frames and the action. Thus, it's
'action-conditional'. And the domain of `x(t)` is from the Atari benchmark.

The paper is easy to read and understand. Here are a few points I found
interesting:

- They use a feed-forward and recurrent architecture. The former stacks the
  frames, the latter uses one frame per time. Makes sense. In both cases, they
  do some convolutions to the input, "insert" the actions (in a "multiplicative"
  manner) and then do "upsampling" to get the output back to size (3,210,160).

- Curriculum learning: the network is actually trained with K-step predictions,
  with the prediction of a time step acting as input to the next time step. This
  allows for longer-horizon training.

- Evaluate qualitatively by looking at video, and quantitatively with pixel-wise
  MSE.

- Importantly, we need to *use* the predicted frames in control; otherwise why
  do we both with prediction? (1) Replace the emulator's frames with predicted
  frames for use by DQN, and (2) use the predictions to improve DQN exploration.

  1. Pre-train DQN, then DQN after that uses predicted frames only. But, the
  score decreases (unsurprisingly). I'm a little confused about why this is
  useful...

  2. Exploration: 

  > Thus, we propose an informed exploration strategy that follows the
  > `ε`-greedy policy, but chooses exploratory actions that lead to a frame that
  > has been visited least often (in the last `d` time steps), rather than
  > random actions.  Implementing this strategy requires a predictive model
  > because the next frame for each possible action has to be considered.

  Similarity in frames is measured via a Gaussian kernel.

- They test with 5 games, because it was mid-2015.

It's probably one of the standard references if we're talking about 'video
prediction' in the context of reinforcement learning. Note that it was published
in late 2015, which is before a lot of DeepRL research.
