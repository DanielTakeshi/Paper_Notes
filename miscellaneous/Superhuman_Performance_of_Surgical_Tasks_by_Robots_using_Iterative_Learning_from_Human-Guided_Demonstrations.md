# Superhuman Performance of Surgical Tasks by Robots using Iterative Learning from Human-Guided Demonstrations

This is an apprenticeship learning paper (recall Pieter Abbeel's 2004 paper ...)
that advertises teaching robots to conduct trajectories in a superhuman manner
w.r.t. **speed** and **smoothness**. Tasks are a **figure eight trajectory** and
a **two-handed knot tie**. (In general, it seems like aiming for two broad tasks
is a good idea for an ICRA paper.)

Yes, we get human demonstrations, and while each individually is not perfect,
they each deviate in different ways so an apprenticeship learning approach with
a smoothness prior should be able to infer (by some rough notion of "averaging")
the intended, ideal *reference trajectory*. To learn *faster* trajectories, what
they do is incrementally increase the speed while (I think) improving the
approximate dynamics model each iteration. If we knew the *exact* dynamics
model, then we could jump straight to fast trajectories.

(This paper has had a reasonably high impact level for a surgical robotics
paper.)
