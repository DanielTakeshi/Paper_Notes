# Deep Reinforcement Learning for Robotic Manipulation with Asynchronous Off-Policy Updates


**What's the contribution?** They take a prior algorithm (well, it's also
theirs) called NAF (similar to DDPG, which they also benchmark) and parallelize
it across multiple works like in A3C-fashion. Then they apply it to real-world
robotics, showing that you can do RL on them *without* any demonstrations or
simulators beforehand. That's great.

The other technical contribution is to show how to put in constraints in the
motions of robots to make exploration applicable in the real world. In my
opinion, though, this is not really the strength of the paper and they also just
mention "heuristics" at one point, without going in depth.

Here's their argument:

> To the best of our knowledge, this is the first direct deep RL method that has
> been demonstrated on a real robotics platform with many DoFs and contact
> dynamics, and without demonstrations or simulated pretraining

Experiments done on: (a) robot reaching to a location, (b) door pushing, (c)
door pulling, and (d) pick and place. For real robots, they did it on (a) and
(b). Furthermore, with just 2.5 hours of training *from scratch* they can train
a robot to open doors. Having multiple workers in NAF helps. And their way is
better than a linear baseline. 

Note: this is *not* using raw pixels as input, but joint angles, quaternions,
etc. That's the state space, not raw camera images.

**Why does it work?** Well, NAF works ... and it's obvious that the asynchronous
version should work by getting faster collection of more diverse data. In their
experiments, the robots are working on the same system (more or less) so the
data won't be *too* diverse from robot to robot.

The main weakness is that part of why it works is due to careful reward function
design. But (Nair et al, 2018) showed that you can get something similar to work
(on simulated robots, though) with just a binary reward by utilizing Hindsight
Experience Replay. 

**Could I implement this?** Unfortunately I need the robot and probably a lot
more intuition on joint angles and quaternions. If you just give me the states,
and tell me how to do the action space, I could probably do this in simulation.

Details of state space for the simulated reaching task:

> State features include the 7 joint angles and their time derivatives, the
> end-effector position and the target position, totalling 20 dimensions.

Details of actions (I think) for simulated tasks:

> We modeled a 7-DoF lightweight arm that was also used in our physical robot
> experiments, as well as a 6-DoF Kinova JACO arm with 3 additional degrees of
> freedom in the fingers, for a total of 9 degrees of freedom. Both arms were
> controlled at the level of joint velocities, except the three JACO finger
> joints which are controlled with torque actuators.

Details of state space in the physical robot tasks:

> The orientation of the door was measured by a VectorNav IMU attached to the
> back of the door. Unlike in the simulation, we cannot automatically reposition
> the door for every episode, so the pose of the door was kept fixed. State
> features for the door task include the joint angles and their time
> derivatives, the end effector position and the quaternion reading from the
> IMU, totalling 21 dimensions.

Thus, I should think of "states" as being joint angles, derivatives, current
*and target* end-effector position. I'm not clear about the action space, but I
think it's the delta (change) in the joint angles. Or the policy could literally
output the joint angles that the robot should go to; I think that's what the
DDCO paper did.

**Overall thought**: I like this paper, except the main thing I wish they had
done was talk more about what's necessary to get RL to work in the real world. I
don't think there's enough discussion of that, and they could have replaced some
of the asynchronous NAF discussion with that. Again, to me, it seems obvious
that parallelized asynchronous NAF would work better than normal NAF, but how to
actually handle RL on physical robots is a bigger question.
