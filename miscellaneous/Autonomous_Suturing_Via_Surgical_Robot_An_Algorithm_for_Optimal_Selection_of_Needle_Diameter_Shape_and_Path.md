# Autonomous Suturing Via Surgical Robot: An Algorithm for Optimal Selection of Needle Diameter, Shape, and Path

This is an interesting follow-up paper to the AUTOLAB's surgical suturing paper
(from ICRA 2016). Like the AUTOLAB's paper, they 3-D printed a "SNAP"-like
gripper to securely grasp the needle. Though unlike us, they use the Raven II
and not the dVRK (but they used the Raven II sparingly, most of the contribution
is based on simulations).

**Main contribution**: develop an optimization-based approach for determining
"good" parameters for needles and insertion paths for surgical suturing.

**More formally**: inputs are tissue geometry, surgical needle entry/exit
points, etc., and outputs are the needle geometry and suggested path. This makes
sense! Also, this paper correctly points out the novelty of this, since it seems
reasonable that prior work would not have optimized this and instead just
grabbed whatever needle "seems good" in the lab. I think that's what Goldberg's
former students did.

**Other details**:

- They focus on "Fixed-Center Motion" where the needle is only rotated about its
  geometric center. This seems easiest to deal with, though in practice for us I
  don't think we're worried about this versus a moving geometric center.

- They develop a kinematic model for describing needle-tissue geometry and the
  suture path. Bleh.

- They formulate a loss function which is the sum of six components (well,
  weighted, but they end up using equal weights ...). See Table I for what they
  use. There are A LOT of variables. This is my problem with these papers,
  there's so much explicit programming that I wish we could do away with. I
  don't like Tables II and III, and wish there were better ablation studies.

  Suture parameters: (1) needle entry angle, (2) needle exit angle, (3) distance
  between actual/desired entry, (4) distance between actual/desired exit, (5)
  needle depth, (6) needle wound symmetry.

  Needle variables: (1) needle center, (2) needle diameter, (3) needle shape. By
  now I know (2) and (3) since those are advertised when you buy needles. (1) is
  really two things, for both the x and y coordinates

- Use the loss function for an optimization problem, and solve it using MATLAB
  and brute-force search.

- As expected, they use offline simulations to evaluate accuracy and
  performance.  They also did eight trials on the Raven II, using SNAP, which is
  what Sid did for ICRA 2016. Note, however, that they only do ONE throw here,
  whereas Sid did four throws. Of course, he only got 50% success for that ...

- Overall, they make a contribution with telling us to optimize for stuff we may
  have taken for granted or side-stepped with human judgment (so cite this paper
  to mention that people have tried optimizing for needle diameter and shape),
  but I doubt the usefulness and generality of this method, even when
  considering that most robotics papers are like this. The optimization problem
  seems so cumbersome and we have to measure a lot of things I'd rather automate
  out or ignore.
