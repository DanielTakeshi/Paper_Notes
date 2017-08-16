# Learning Task Error Models for Manipulation

Chief contribution: show that one can use Gaussian Process regression to
estimate nonlinear "residual errors" **on top of** existing forward kinematics
models which have some error. This is important for calibrating robots so that
when we command it to move somewhere, it actually lands at the correct location
in real life ("robot space"?).

Notes:

- For calibration, they use two sets of targets. One is a grid with 32 markers
  in it (it's 4 by 8) and this is most similar to the kind of target I used for
  calibrating the DVRK in the past. The second target is about grasping a cube.

- Yes, there is uncertainty in calibration. They argue that their robot has two
  main sources: one for the "head kinematic chain" and another for the "cable
  stretch and motor-side encoders". Thus, their two calibration targets are
  designed to test w.r.t. those two respective sources of uncertainty.

- Gaussian Process Regression is used for correcting **residual errors**. These
  aren't done alone, they're built on top of existing forward models. Also, GPR
  probably could be replaced with a neural net or a random forest, but they also
  mention that GPR has the advantage of being essentially zero outside the state
  space in the data, so if there's a configuration that's far from what was
  normally seen, the GPR will at least not cause performance degradation.

- They had to estimate `(x,y,z,yaw,pitch,roll)` I think as described in Section
  III-A, but for the DVRK it's easier with the built-in API call to get the
  current Cartesian frame, right?

- There are two main sets of experiments. Both of them show figures where the
  error in some way (standard deviation for the first, centimeters and degrees
  for the latter) decreases with the extra GP model. These figures show solid
  improvements, and I can definitely do something like this for the DVRK.  First
  one relies on randomly generating test configurations. The second relies on
  storing those 1786 end-effector poses. I need to see the videos because
  neither is clear from the writing, e.g. I don't quite get the 10-dimensional
  configuration for the first set of experiments.

- I find it somewhat amusing that they keep emphasizing that they "only consider
  relevant tasks" or something like that. It seems like you don't have to say
  that. Also, they spent too much time discussing the definition of GPR.
  Honestly, this could easily be cut to a six-page paper with no loss of
  quality.

TODO: watch the videos, and figure out why standard deviation is the 
metric used in the first experiment's results. (Why is it not centimeters and
degrees like in the second one? I am still not sure but maybe watching the
videos would help.)
