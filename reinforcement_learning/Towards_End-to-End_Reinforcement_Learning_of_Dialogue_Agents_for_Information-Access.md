# Towards End-to-End Reinforcement Learning of Dialogue Agents for Information Access

A paper which combines Deep RL and NLP and databases. It proposes **KB-InfoBot**
to help humans search Knowledge Bases (KB) in a *differentiable* manner.
Previous methods needed to query a database (which is natural and intuitive),
but this not differentiable.


Main points:

- Replace symbolic queries with soft distribution over the KB to make things
  differentiable, like hard to soft attention models. Go from **Hard-KB** to
  **Soft-KB** lookups.

- What motivates this work at a high-level? There are lots of conversational
  agents on the market (e.g. Siri) but:

  > In particular, they [Siri, etc.] lack the ability to learn from interactions
  > with a user in order to improve and adapt with time.

- In order to train using (deep) RL, they need a simulator which can mimic
  users, to provide online updates. This is OK and standard in the dialogue
  community.

- The **Belief Tracker** (language understanding) and **Policy Network**
  (dialogue) are the two components of the system with trainable parameters. The
  non-differentiable parts are the **Soft-KB Lookup** and **Beliefs Summary**.
  The agent as a whole takes in user utterance `u_t` and returns action `a_t`.
  Actions are either requesting information from a slot in the KB, or informing
  the user and terminating the environment.


Training and Experiments:

- To test their hypotheses, they introduce hand-crafted versions of the belief
  tracker and dialogue policy to benchmark versus the neural network versions.

- Training is done with REINFORCE. But not quite, there's an imitation learning
  phase which initializes the agent to help performance. There's also two
  separate policies to learn, `\pi` and `\mu`. Rewards are -1 for a "failed"
  dialogue, and -0.1 for each turn (i.e., one user action and one agent action)
  to encourage shorter sessions. By changing the reward signal, one can adjust
  the behavior towards maximizing success or minimizing the turns.

- Experiments use a movies KB from somewhere. I don't do work in this area so I
  can't comment on the datasets used.

- They do brief analysis with human subjects (using typing as the interaction
  medium). But they admit they don't get the best performance:

  > The E2E gets the highest success rate against the simulator, however, when
  > tested against real users it performs poorly with a lower success rate and a
  > higher number of turns. Since it has more trainable components, this agent
  > is also most prone to overfitting.
