# Dynamic Action Repetition for Deep Reinforcement Learning

This paper addresses the issue of the fixed hyperparameter k which indicates the
frame skip rate. It turns out to be an important hyperparameter, as I know.
Higher k means we repeat the same action longer, lower k means we can change
faster.

- They do this by having two hyperparameters, r1 and r2. The final layer of the
  DQN network (or A3C actor network, though they weren't as specific on that)
  has double the number of output nodes, so there are really two vectors
  indicating the Q-values for the actions. One of the vectors corresponds to
  rate r1, and the other is rate r2.

  This was a bit disappointing, since I was hoping we would *learn* r1 and r2,
  rather than just put in a second hyperparameter .... They set r1=4 and r2=20.

- The good news is that they experiment with both ARR (action repetition rate) 4
  and 20 for normal DQN with double the nodes in the last layer. Unfortunately
  their tables are missing some important benchmarks, e.g., it's unclear why 
  they didn't do ARR of 20 for all games tested.

- Unfortunately, there's only four games tested (well, kind of understandable
  given limited computational resources...), and little or no discussion of how
  many random seeds were run, now many trials they ran, etc. I have no idea
  about the statistical significance of these results.

However, it seemed to have met the bar for AAAI.

I'm also thinking about the connection between this and the options framework.
They're doing almost the same thing, after all, the ARR is telling us when we
should stop an option. So think of the ARR telling us to do an action for k time
steps, but we could also have it tell us to execute the sub-policy k times.
