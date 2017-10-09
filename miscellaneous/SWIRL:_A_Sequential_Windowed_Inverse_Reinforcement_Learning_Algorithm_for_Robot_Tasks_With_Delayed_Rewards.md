# SWIRL: A Sequential Windowed Inverse Reinforcement Learning Algorithm for Robot Tasks With Delayed Rewards

This paper builds upon the earlier TSC 2015 paper (and TSC-DL from 2016) for
LfD by performing inverse reinforcement learning. This lets them build reward
functions based on expert demonstrations.

Again, (stochastic) linear dynamical systems are assumed, with `x(t+1) =
A(t)x(t) + w(t)` or something like that.


## Technical Details

(I'm going to have to review Maximum Entropy IRL again...)


## Experiments

- **Simulation**: Parallel Parking with noisy dynamics, a Two-Link acrobot, and
  a 2D GridWorld. (I assume if OpenAI gym were around then, they would have done
  that.)

- **Real**: dVRK. They had the dVRK cut along a line in a gauze, as done in
  Murali 2015 (who did line cutting in addition to debridement). They provided 5
  demonstrations (wow, just 5?) by directly touching the end-effector. With my
  dVRK experience, I have more intuition on their description, including why the
  movement is planar, etc.
  
  Figure 4 says they demonstrate the location of the line without actually
  cutting it, but is that for the test scenario? I don't think so, and it better
  not be ...

  Honestly I feel like there is still not enough detail in the dVRK
  descriptions. Sigh ...
