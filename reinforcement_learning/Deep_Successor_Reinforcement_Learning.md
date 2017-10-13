# Deep Successor Reinforcement Learning

This introduces the DSR framework, so hopefully there will be additional uses
for it other than the ones shown in the experiments.

**Technical General Idea** 

They extend "Successor Representations" (SR) into the *deep* variety (DSR). 
Benefits of this are (a) to increase sensitivity to distal reward changes, which
must help out with credit assignment, and (b) to extract bottleneck states. But
why are these called candidate goals?

What is SR anyway? Splits value function into a *reward predictor* and a
*successor map*. They argue that the value function is the dot product between
the reward predictor and successors. The reward predictor here predicts the
*immediate* scalar reward for a given state, so it's not the same thing as the
value function which in general is the expected sum of discounted rewards (when
playing optimally?). The successor helps to weigh the reward by the state
visitation frequency, so the dot product seems plausible to me, particularly
because the state visitation frequency is discounted. EDIT: see Section 3 for
formalism.


**Technical Details** 

- They use function approximation (with a deep neural network) to approximate
  the SR so that the SR and reward function can be learned from raw inputs. See
  Figure 1 for the model architecture, which also includes a reconstruction
  (huh, like a VAE?).

- Looks like a lot of stuff is similar to the DQN paper, as in the loss
  function, etc., as one would expect. Also, the reconstruction loss is L2,
  which makes sense.

- They argue that subgoals are useful for the options framework and can help
  exploration efficiency. I should talk with Roy about this.


**Experiments**

- **MazeBase**: simple grid-world domains, which is fine for proof-of-concept
  stuff.

- **Doom**: seems to be an increasingly popular environment that I better try
  out one of these days.
