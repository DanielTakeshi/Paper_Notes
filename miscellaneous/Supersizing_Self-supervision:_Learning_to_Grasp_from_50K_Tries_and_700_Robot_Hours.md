# Supersizing Self-supervision: Learning to Grasp from 50K Tries and 700 Robot Hours

This remarkable paper shows that by increasing data (i.e., grasp attempts) by
40x compared to the largest prior work, one can continually run a robot to
attempt grasps and eventually train it to grasp objects reliably, including
novel objects not seen during testing.

Key technical contributions:

- Self-supervision, where objects (in CLUTTER) are placed on the workspace, and
  the (Baxter) robot continually tries to pick up objects.

- A simple "curriculum" or "staged learning" phase when, after some random
  exploration/actions, the robot uses a trained neural network and then samples
  actions based on that. Hard negatives (i.e., inaccurate grasps predictions,
  sort of) are used to assist in training by providing greater signal.

- The neural network takes in a camera image, processes it through a set of
  pre-trained AlexNet layers, and then performs an 18-way binary classification
  problem, because there are 18 different parallel-jaw grasp angles (discretized
  into bins) and multiple angles could lead to successful grasps. You can apply
  18 softmaxes, one per discretization, if you want a probabilistic
  interpretation. Also, classification is easier than regression.

  **Update 09/2018**: wow, how did I not see this before? It's not the *camera*
  image but a *patch* that is *within* the larger camera sensor image. The
  patches are centered about an object --- which one can find by simple Mixture
  of Gaussian or other non-learning methods --- and then we get a bunch of
  angles about the patch center.

- Grasping is done entirely by vision-based judgment, not an analytic model of
  3D objects, etc. No human labels as well, either. The labels come from
  measuring the forces on the gripper after gripping, which is similar to what
  Sergey did, and works well for objects that are not too thin (which would be
  hard to grasp anyway).

This paper shows the remarkable ability to learn grasp policies that actually
generalize to novel objects outside the training data.

How does this relate to Sergey's arm farm paper? It was the predecessor to it.
It doesn't have the continual servoing mechanism that Sergey's paper has and
requires camera calibration, but this one has a lot of what the later one does,
i.e. just learning from tries with automatic data labels. Also, the arm farm
paper did not use the "discretize angles into bins" approach as this one does.

**Potential limitations?** It's data hungry, I suppose. But this work is very
impressive, so hard for me to criticize ... unless one just doesn't like
self-supervision papers (the critics, heh)? In terms of writing, I think the
experimental section needs to be improved to match the quality of the rest of
the paper, it's lacking some clarity. For example can the 79.5% figure be
derived by the stuff in Figure 8, which shows other accuracy results? It doesn't
look like it.

IMPORTANT NOTE ON EVALUATION: for testing, they can execute grasps with the
robot by sampling a set of patches and determining the best patch+angle
combination. However, their network is evaluated based on whether it *correctly
predicted the outcome* (success or failure). I got confused by this originally,
because I thought this was purely about *success rates in practice* (i.e., it
grasped 80% of objects successfully), but they actually measure accuracy. That's
somewhat disappointing, actually, as I wanted to see actual success. They do
report that in Section IV-F though it seems to be less heavily emphasized as the
other parts of the paper. Rates are roughly 66% to 73%, not bad!
