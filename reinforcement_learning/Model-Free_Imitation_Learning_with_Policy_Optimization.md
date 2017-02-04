# Model-Free Imitation Learning with Policy Optimization

The paper's focus is on the *imitation learning* problem, training an agent
using demonstrations provided by an expert. Behavioral cloning and raw IRL are
not the best choices here, the latter because of the need for an internal RL
loop (interesting). They get around this by *not* learning a cost function (as
is usual for IRL) and the "learning signal" is a "class" of cost functions. To
be more precise ... [TODO]

## Theory and Algorithms

Notation is fairly clear; the setting (apprenticeship learning) also clear
though to a lesser extent for me. I'm still not sure how apprenticeship learning
works if the restriction is that we have to perform "as good as the expert."
That could be very challenging, right, even when considering the restriction of
the cost function to be in a class (of perhaps linear functions)?


## Experiments

TODO


## My Thoughts and Takeaways

There are lots of connections with the [G.A.I.L. paper][1]?

Some interesting intuition I got: in IRL, the assumption of expert optimality
serves *as a prior* on the potential space of policies for our trained agent.


[1]:Model-Free_Imitation_Learning_with_Policy_Optimization.md
