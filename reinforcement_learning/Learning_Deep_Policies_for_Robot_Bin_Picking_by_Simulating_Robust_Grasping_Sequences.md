# Learning Deep Policies for Robot Bin Picking by Simulating Robust Grasping Sequences

Quick highlights:

- Formulates the problem of **bin picking** as a Partially Observed Markov
  Decision Process. The more objects that get cleared from the bin, the more
  rewards.

- They present Dex-Net 2.1, which introduces the datasets for bin-picking. They
  can sample 3-D models from a dataset and then drop them in a pile, using the
  pybullet simulator. (So that's how they get data!)

- Use simulation to train a policy, test in simulation and then in the real YuMi
  robot, fitting in the theme of simulation to real, a trendy research topic
  nowadays.

- The POMDP is actually solved using imitation learning, and it looks like they
  apply DART and other forms of data augmentation. The expert supervisor is
  algorithmic as they can algorithmically find where to pick up objects.

- Given a point cloud of the objects (that's what the robot sees) use the robust
  grasping policy from Dex-Net 2.0 to pick the desired grasp, which is the
  top-ranked grasp from the set of sampled grasps after applying the Cross
  Entropy Method.

- The actions are not "move up, move down ..." nor are they continuous control
  in the technical sense like MuJoCo actions, but the actions instead first
  identify where to grasp and at what angle, and then once that's chosen the
  robot moves in a straight line there. I guess it's technically continuous
  control, though?

The major downside of this paper is that, as usual, it's hard to get good
intuition without having actual experience with Dex-Net and/or the YuMi robot.
The physics model isn't completely clear to me. I don't know how much
"mechanical wrench space analysis" is necessary; did they have to implement this
or did pybullet do it?
