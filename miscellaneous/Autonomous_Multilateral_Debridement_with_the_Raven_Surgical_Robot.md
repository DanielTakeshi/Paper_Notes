# Autonomous Multilateral Debridement with the Raven Surgical Robot

First, what *is* debridement? They say:

> This paper considers the sub-task of surgical debridement: removing dead or
> damaged tissue fragments to allow the remaining healthy tissue to heal.

OK, got it. In fact, the actual introduction of "debridement" (for robot
surgery, that is) seems to be novel. Note that as the title implies, this is
*not* with the dVRK, but with another robot. However, it looks quite similar in
design ... and I've been told that they're similar (and that the dVRK is in some
way better). They say that Raven's main challenge is *state estimation*, which I
interpret as "does the robot really know where its arms/joints/grippers are
located?" I assume dVRK has the same problem. Yeah, it's all about uncertainty.
See Mahler et al (2014) about another paper which tries to accurately calibrate
the Raven robot, like what I'm trying to do for the dVRK.

Their contributions also include the first reliable autonomous robot performance
using Raven. Another thing to know is that Raven is "multilateral" meaning that
there is more than one arm. Coordination between the two (or more!) arms is very
important.

As would be expected, there is a spectrum of difficulty for debridement tasks,
and they can start easy and gradually build up to more challenging tasks.
(Sanjay suggests that this paper creates a slightly too-easy debridement
scenario) For this paper, they seem to designate damaged fragments on a planar
workspace, so the robot must detect and remove/pull those out.

Their system uses:

- **Stereo Vision**, and not Kinect-like RGBD cameras. They used a custom camera
  since they were having difficulty using off-the-shelf ones. The vision system
  is used to (a) estimate pose (I assume this means "the x-y-z coordinates of
  the grippers?") and (b) construct a "point cloud" of the setting. It seems
  like I should check out [this Wikipedia page][1] and references therein.  They
  said that the cameras must be "registered" and they did so with a
  checkerboard. I assume this is similar to what I was doing a few months ago.

- **Inverse Control**, this is a byproduct of their stereo vision system. It
  seems really detailed and complicated, so I'm skimming for now. Hopefully this
  stuff can be automated away.

- **Optimization-Based Motion Planning**, this uses **Model-Predictive
  Control**. I'm going to have to review this at some point, but fortunately it
  looks like Stephen Boyd (that guy again!) has his slides online. I wish more
  professors were like him. Anyway, MPC is needed to keep replanning the
  trajectory using optimization-based procedures (using the "trajopt" software,
  BTW) to avoid collisions. They allow each arm to move at most 2.5cm before
  replanning.

- **A Centralized Planner**, a single 12 DOF planner controls two 6 DOF arms,
  rather than having a decentralized plan.

**Experiments**: they used a baseline with a medical student (a human obviously,
and it's definitely not Walter Doug Boyd). This is for speed comparisons.
Results are in Table I, though I wish it were designed and presented in a
cleaner way. (Actually, there are fewer experiments than I expected.) One thing
that *was* expected, though: planning and perception took a lot of time; they
said 50%.

Similarities to what I do: They simplify the vision aspect of it by using a
white background. They use HSV, presumably from OpenCV. They also have colored
markers on the end-effector --- interesting! I used red tape. For tracking
centroid of fragments, they use left and right camera points and the disparity
to get the position of the fragment centroid in "3D space."

Final thought: I keep wondering whether it's possible to apply Deep Learning or
simulation to improve any steps of this pipeline.

[1]:https://en.wikipedia.org/wiki/3D_pose_estimation
