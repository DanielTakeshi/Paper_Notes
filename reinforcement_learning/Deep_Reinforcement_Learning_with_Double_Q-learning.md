# Deep Reinforcement Learning with Double Q-learning

I know the main concept of this paper, but I'm reading it to get more intuition
on what is meant by a "Q-value". (This paper has had quite an impact in the
sense that anyone running vanilla DQN is advised to run double DQN, i.e. DDQN,
and in fact, OpenAI baselines makes DDQN the default version of DQN.) At the
time of publication (remember, this was AAAI, *not* ICLR, ICML, NIPS, etc.) it
had state of the art results.

**Update Dec 2018**: been a while! I re-read the discussion here and it is
making more sense. I can also implement the algorithm. :-) Thank goodness for
Berkeley's DeepRL course.

Also, if you're still confused by why Q-Learning overestimates values, the
simplest way is probably to write down both updates carefully, and then
reasoning about what happens when we take the max values. In normal DQN, we take
the max action over the target net, and in double DQN, we take a value that has
to have a Q-value less than or equal to that quantity since the action is chosen
by the other network.

## Main Idea

**In vanilla Q-learning (including parameterized variants like DQN), values
of `Q(s,a)` are systematically overestimated. By doing DDQN, they can mitigate
these overestimates**. The result is not only  more accurate `Q(s,a)` values,
but also faster convergence and better overall performance on Atari 2600 games.

Specific update rules:

- Normal: `Y(t) = R(t+1) + gamma * max_a Q_theta(s(t+1),a)`
- Double: `Y(t) = R(t+1) + Q_theta(s(t+1), argmax_a Q_phi(s(t+1),a))`

(where `phi` is another set of parameters like `theta`)

For DDQN in particular, the second set of parameters is from ... *drum roll* the
target network!! Yeah, that should have been obvious in hindsight.

Another nice contribution of the paper is some brief theoretical analysis on how
estimation error leads to over-optimism. I might want to take another look at
this again. Figure 1 might be useful to think about; they rely on `Q(s,a) =
V_opt(s) + eps` where `eps` is Gaussian noise representing value function
estimation errors.

Quick pointers:

- Why overestimate? Because Q-learning relies on learning a policy indirectly
  where we define the relationship of ideal Q-values as `Q(s,a) = r + gamma *
  max_a Q(s',a')` and the "maximum" operator means it's overly optimistic.

- It *might be OK* to overestimate values. Why? For instance, suppose we have
  six actions and given a state s, if we overestimate `Q(s, a_i)` by a value of
  1 for all six actions, then it doesn't matter! We would still pick the correct
  action. Thus, uniform overestimation does not appear to be a problem. The
  issue here, of course, is that Q-learning does *not* uniformly overestimate.
