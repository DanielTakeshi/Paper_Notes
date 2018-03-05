# Imitation from Observation: Learning to Imitate Behaviors from Raw Video via Context Translation

**Main contribution**: a framework for learning how to imitate when we're not
given direct (state,action) pairs we can feed into a supervised learning
algorithm, but from *videos* that are observed in a different
"context/perspective", or in "third person".

**Why does it work**? It works because they can develop a neural network that
translates from the video perspective into the "first-person" robot view. By
doing this, we can translate everything into the standard view for IL
algorithms.

Didn't go through the details on their algorithm, but might do so later if it's
important to me.

There's a reinforcement learning component that needs to be done (in addition to
the context translation model) and for that, they use TRPO for simulated tasks
and GPS for the real ones.

Experiments are done on four simulated tasks and several physical tasks. In the
physical ones, the tools were set to be the same so that there isn't *too* much
systematic domain shift. If there was, just apply some of the domain adaptation
methods from computer vision. (This is something I need to remember.)

This paper immediately reminded me of third-person imitation learning, and the
authors claim their algorithm is more robust than that (and GAIL, fyi). Note
that this current work makes no use of GANs whatsoever, but was inspired by much
of the computer vision literature on GANs for translating in different contexts,
so it's definitely an interesting line of a "(robot, computer vision, imitation
learning, generalization)" paper.

They use real robot simulations and the YouTube video is impressive ...  it's
nice to see the actual context translation image from their model, which shows
that their method is working.
