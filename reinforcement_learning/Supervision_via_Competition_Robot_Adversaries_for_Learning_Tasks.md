# Supervision via Competition: Robot Adversaries for Learning Tasks

**Why (for me)**: I'm interested in this paper because I'd like to see how
adversarial learning works with *real* robots and not MuJoCo simulation.

**Why (for the paper)**: the paper fits in the popular theme of data-driven
learning of [insert task here] where due to the amount of data, we need
*self-supervision*. I think of this as unsupervised learning where we instead
have to supply labels in an automatic and principled manner. See Levine et al.,
2017 on their "ant/arm farm" paper.

Here, they propose to do the semi-supervision with an adversary, which should
make the protagonist more robust and be more effective than the important
baselines of a cooperative and collaborative robot (or another baseline of just
using sensors to measure success/failure). Hence, this fits with research on
robustness, which is obviously quite important. 

**Technique**:

- Here's what they say:

  > [..] if the first learner is trying to learn how to grasp the object; the
  > adversarial learner attempts to learn how to steal the object via snatching.
  > This in turn forces the original learner to learn to grasp in a robust
  > manner such that it cannot be snatched by the adversary.

  This sounds cool! For shaking, the antagonist has control over the
  protagonist's arm, but for snatching, it doesn't and instead it uses a second
  arm.

- They formulate the algorithm mathematically as the problem of learning `W^p`
  and `W^a` (see Section IV) and use convolutional neural networks. The CNNs
  predict the actions. So it is going from robot sensors (i.e., camera images)
  to actions (or joint forces).

**Experiments** (on grasping):

- They use a Baxter robot with two arms. One arm grasps while the other one is
  the adversary and can shake the arm or snatch the object. My intuition is that
  this requires some careful tuning to ensure that the problem is doable and
  that there is non-negligible benefit to having an adversary.
 
- Initial data collection is performed with random actions and measured based on
  success (i.e., self-supervision) via sensors. The same thing from Sergey's arm
  farm paper, BTW. They initialize `W^p` and `W^a` and *then* do joint training.
  Well, initialization is important!

- Their technique depends on the grasping task being highly reproducible and
  low-cost to start. For surgical robots, you can't just do random actions for
  initializing these networks.

- Some details related to grasping that may or may not be critical, including
  discretizing the angle of grasps into 18 values, etc.

I suppose one downside is that despite arguing how adversarial learning improves
sample efficiency, the authors had to initialize policies from their ICRA 2016
paper, which used a tremendous amount of data. So there's still a lot of data
here.
