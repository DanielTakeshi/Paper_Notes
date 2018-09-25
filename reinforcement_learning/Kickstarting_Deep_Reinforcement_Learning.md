# Kickstarting Deep Reinforcement Learning

Similar to other teacher-student papers, e.g., DQfD from DeepMind and
"Overcoming Exploration" by Ashvin Nair. They use different scenarios and
agents, though, so it's not an easy comparison. (Thank goodness DeepMind is
trying stuff other than the Atari games...) And also, they are now letting the
student surpass the teacher as needed, which by now seems almost mandatory for
state-of-the-art deep imitation learning algorithms.

The contribution:

- Framework for using previously trained agents (i.e., teachers) to speed up
  training of new, student agents.
- Use a loss function which augments normal loss of an RL algorithm (A3C) with a
  distillation loss, which encourages the agent to match the expert's actions
  via cross-entropy. (It seems like only discrete action environments are
  considered.)
- It's similar to policy distillation, but the student is still running RL and
  not restricted to simply match the softmaxes or to visit the states the
  teacher visited. Hence, it should be able to eventually surpass the expert.
  Also, the emphasis on matching the expert softmaxes decreases over time, as
  one would expect. (The paper has a good analogy with entropy regularization
  for A3C, where here we are regularizing it only to match teachers, but it is
  not a hard constraint.)
- Uses parallelism ("IMPALA"), so the A3C has multiple workers collecting
  rollouts, and each actor itself is a student, which has its own `lambda_k`
  constant to emphasize the importance of matching the teacher softmaxes.

How is the evaluation done?

- DMLab-30, with 30 distinct tasks. I don't know too much about these,
  unfortunately.
- Ah, they are taking a window of 30 million (!!) environment frames for the
  agent, and then using a "human normalized" score to report. So, like other
  papers, I think they evaluate *during training*, so there are no held-out
  "test set rollouts." They run for 10 _billion_ training steps. OK, this is
  getting ridiculous...
- And for the full score, we average over the 30 tasks. Sure.
- They use two different network sizes, small and large, and show that
  kickstarting with a small teacher is very helpful for training of the large
  student. Also, with multiple teachers it's better as well. In particular,
  multiple small teachers are better than one large teacher.
- They also show kickstarting is better than policy distillation.

To their credit, the plots clearly show benefits to kickstarting. Obviously, if
we have trained teachers, we should use them rather than train an RL agent from
scratch. Also, as expected, the distillation weights decrease over time, so
there is less emphasis on matching the expert softmaxes.
