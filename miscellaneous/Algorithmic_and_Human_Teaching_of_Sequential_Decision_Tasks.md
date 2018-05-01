# Algorithmic and Human Teaching of Sequential Decision Tasks

https://danieltakeshi.github.io/2018/04/29/algorithmic-teaching/

Assumes we have an agent that runs IRL (from Abbeel et al, etc.). They present
an algorithm for inferring the most informative demonstrations:

> Based on the formulation of the IRL agent, an informative set of
> demonstrations is one that allows the agent to compute relevant feature counts
> and infer the weights as accurately as possible. Hence, the most informative
> demonstrations are the ones that reduce the uncertainty in the reward
> estimation.

Examples are on a simulated maze (grid-world style) and then using some human
teaching. Compare with random demos, demos suggested by algorithm, etc.
