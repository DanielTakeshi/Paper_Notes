# Intrinsic Motivation and Automatic Curricula via Asymmetric Self-Play

The authors propose a new paradigm for automatic design of curricula by having
two agents, Alice and Bob. Alice's job is to show a task. Bob's job is to
complete it (whether via repeating it, or doing it in reverse, depends on the RL
environment). Alice's reward function is designed to pick the hardest task
within an environment that Bob can't complete, but with an implicit constraint
on the number of steps, so in some sense it's at the frontier of Bob's
abilities. That's what the automatic curricula is about.

In test-time tasks, Bob just goes on his own, Alice doesn't play a role.

Both Alice and Bob are parameterized by neural networks taking in two states
(start/goal) and outputting actions.

I have some concerns over the paper because the whole paradigm can seem highly
artificial, if that makes sense. I'd also like to take another pass to better
look at the reward functions and how specifically the curricula are developed in
the different environments.
