# C-LEARN: Learning Geometric Constraints from Demonstrations for Multi-Step Manipulation in Shared Autonomy

Not totally sure on the technical details for this, but at a high level it
builds a knowledge base for robot manipulation *with constraints*, which can be
enforced (with certainty) unlike with an LfD using supervised learning. So,
augment LfD with support for geometric constraints.

Like a lot of meta-learning now (from Berkeley and OpenAI, at least, heh) they
do a two stage process where they build a knowledge base (the prior) and then
learns a multi-step manipulation task using a *single* demonstration.

The other cool thing is the robot-to-robot transfer despite differing
kinematics.
