# Using dVRK Teleoperation to Facilitate Deep Learning of Automation Tasks for an Industrial Robot

This is a Deep Learning from Demonstrations (Deep LfD) paper relating to a
surgical robot and an industrial (grasping) robot.  The "LfD" part means that
we're in imitation learning land, while the "deep" part means we have a policy
represented by a deep neural network.  Note that the surgical robot (dvrk) is
not actually used, but instead its "mater manipulator" is used to actually help
the industrial robot (YuMi). So to be clear, the YuMi is doing the tasks.

How does data collection for IL (or LfD more accurately) work? We have (1)
kinesthetic teaching, and (2) low DOF teleoperation. This work proposes to do
something that seems to be a better version of (2) using what's known as a
"master-slave bilateral teleoperation system."

> Master-slave bilateral teleoperation systems such as those used in robot
> assisted surgery [13] or based on virtual reality [14] can mitigate these
> issues by enabling users to directly control a robot's end-effector through a
> master pair of arms designed to record natural human motions.

Hmm ... maybe it's actually the best of (1) and (2) together?

Their contributions:

- Develop a Python library to manage this system.

- Three major experiments: (1) scooping a ball into a cup, (2) untying a knot in
  rope, and (3) pipetting liquid from a beaker to a graduated cylinder. Most of
  their benchmarks are compared with kinesthetic teaching, and they show that
  their way is just as good if not better.

- To be clear, they *only* use Deep LfD for *one* of the tasks, the "scooping"
  one. They do, however, test the master system for **data collection** in all
  three tasks. That's why the paper is called "facilitate" instead of another
  better verb.

The notation in this paper is horrendous, but I don't know a better alternative.

This paper seems to fit a small niche. The master teleoperation system is nice,
but I'm not sure how it generalizes to other robots.
