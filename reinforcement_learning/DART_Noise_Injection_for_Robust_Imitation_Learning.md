# DART: Noise Injection for Robust Imitation Learning

**TL;DR: add Gaussian noise to expert demonstrations so that Behavioral Cloning
works better, with noise in proportion to the error in the robot's trained
policy. The algorithm is called DART, for Disturbances for Augmenting Robot
Trajectories**.

Highlights:

- Behavioral cloning sucks due to covariate shift.

- Add noise so that the supervisor "visits" more states (in terms of density,
  probability, etc.), particularly those where it may be close to "the edge"
  (intuitively).

- TODO: I need to understand the theory.

Fortunately, the idea is simple enough that I could implement this if I wanted.
And intuitively it seems like it should work, so long as the added noise is
small enough, and that it's done in a principled way (this is why I need to know
the theory).

Note that this isn't good enough to exceed the expert supervisor performance.

See the blog post for more details:

http://bair.berkeley.edu/blog/2017/10/26/dart/
