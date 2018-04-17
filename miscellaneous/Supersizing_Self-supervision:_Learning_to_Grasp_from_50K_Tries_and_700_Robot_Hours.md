# Supersizing Self-supervision: Learning to Grasp from 50K Tries and 700 Robot Hours

This remarkable paper shows that by increasing data (i.e., grasp attempts) by
40x compared to the largest prior work, one can continually run a robot to
attempt grasps and eventually train it to grasp new objects.

Key technical contributions:

- Self-supervision, where objects (in CLUTTER) are placed on the workspace, and
  the (Baxter) robot continually tries to pick up objects.

- A simple "curriculum" or "staged learning" phase when, after some random
  exploration/actions, the robot uses a trained neural network and then samples
  actions based on that. Hard negatives (or grasps predictions that are very
  off from what actually happens) are used to train the network.

- The neural network takes in a camera image, processes it through a set of
  pre-trained AlexNet layers, and then performs an 18-way binary classification
  problem, because there are 18 different parallel-jaw grasp angles (discretized
  into bins) and multiple angles could lead to successful grasps. Sigmoids are
  used to get a probabilistic interpretation for each of the 18 bins. Also,
  classification is easier than regression.

- Grasping is done entirely by a vision-based judgment, not an analytic model of
  3D objects, etc. No human labels as well, either. The labels come from
  measuring the forces on the gripper after gripping (I suppose they can just
  measure gripper width like Sergey did).

This paper shows the remarkable ability to learn grasp policies that actually
generalize to novel objects outside the training data.

How does this relate to Sergey's arm farm paper? Well, it was arguably the
predecessor to it. It doesn't have the continual servoing mechanism that
Sergey's paper has, but this one has a lot of what the later one does, i.e. just
learning from tries. I think the patches and angles (which were discretized into
18 bins) in this paper also didn't appear in Sergey's paper, which just told the
robot to grasp.

Potential limitations? Well, it's data hungry, I suppose. But this work is
very impressive, so hard to criticize ... unless one just doesn't like
self-supervision papers? In terms of writing, I think the experimental section
needs to be improved to match the quality of the rest of the paper, it's lacking
some clarity.
