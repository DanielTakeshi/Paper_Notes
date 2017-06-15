# A Case Study of Trajectory Transfer Through Non-Rigid Registration for a Simplified Suturing Scenario

Goals to understand after reading this paper:

- What is the "trajectory transfer" they talk about?
- What is the "non-rigid registration" they talk about?

The algorithm applied in this paper is entirely based on their submission to
ISRR, which didn't come (i.e. get accepted) until later. That's why this paper
is "a case study," though really it's two since they conducted two sets of
experiments: a simulated Raven II system and a real-life PR2 system. The videos
of the simulator look interesting but the software is likely outdated.

An experimental tip for me: to find out if something generalizes, consider
adding noise as needed. This is what they do to test trajectory generalization.
See Section V-E.

Unfortunately, I don't understand their algorithm completely because I don't get
the "thin plate splines" thing. Well, other than that it helps us to find a
mapping from demonstration-to-test scenes. I *have* seen that cartoon warping
demonstration diagram before, so I think it will come naturally to me if I need
to spend more time understanding it. It's probably in Pieter Abbeel's CS 287
slides. FYI: that warping thing consists of the first two components of Figure
2, i.e. the non-rigid registration and warping.

**Thoughts**: I wanted to read this paper to see how we can automate suturing,
since that seems to be a key target of interest (see also Ken Goldberg's
suturing papers). Yeah, for these robotics conferences, we really need at least
one set of real-life experiments. The conclusion for this paper seems awfully
obvious: that similarities among demonstrations vs test trajectories are
important. I don't think we need a full paper for that!

Registration means anything that can tell us the 3-D coordinates of some desired
thing? And non-rigid means ... what? That whatever they're using to model
surgical skin is deformable and thus not rigid? I don't know.

As for generating the trajectory, that's in Section III. I thought this was
trajopt, and maybe it is, but they don't cite it. The optimization problem
formulation seems to have a similar flavor, though.
