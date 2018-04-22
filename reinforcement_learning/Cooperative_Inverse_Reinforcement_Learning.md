# Cooperative Inverse Reinforcement Learning

Really cool paper on the *value alignment* problem, basically how do robots
learn from a human, with problematic teaching, as in humans may not be optimal
in their demos, and humans are bad at explaining their value functions. Really
what we need is interactive robot-human teaching. 

Their formalism results in active teaching and active learning, where the
teacher has to find optimal training examples to teach the learner.

> The key assumption IRL makes is that the observed behavior is optimal in the
> sense that the observed trajectory maximizes the sum of rewards. We call this
> the demonstration-by-expert (DBE) assumption. One of our contributions is to
> prove that this may be suboptimal behavior in a CIRL game, as H may choose to
> accept less reward on a particular action in order to convey more information
> to R.

See also the BAIR Blog post on this.
