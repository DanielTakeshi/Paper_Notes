# DDCO: Discovery of Deep Continuous Options for Robot Learning from Demonstrations

**Update**: wow, this summary is out of date. I've read the paper in far more
detail since then, and feel like I understand almost everything. It's some great
work, especially with how unstable the dVRK is, from personal experience. I also
like how Sanjay got to show the different options visually by looking at the
network output.

This paper builds upon the earlier TSC and SWIRL papers to generalize the LfD
algorithm to be DDCO (which additionally generalizes their DDO paper!).

## The Problem

Why do they want to study this? Their prior method, DDO, automatically discovers
options while training the agent. But this is not for continuous control, hence
that is the extension to this paper. And the reason for wanting hierarchical
control (as in DDCO/DDO) is that this imposes a strong (but reasonable) prior
that helps make learning tractable, e.g. by having highly developed primitives
that we can simply call from a higher-level "meta policy".


## Technical Details

They use the expectation-gradient algorithm with a slight modification of a
distribution choice for actions, and then they apply a cross-validation
procedure to tune the number of options. (Thus they avoid tuning the parameter
by running the policy in the environment.)


## Experiments

There are three: one in simulation with a three-link robot, and two real robot
experiments (with the dVRK) doing **needle insertion** and **needle bin
picking**.

- **Simulation**: the two options are moving the box forward or backwards. OK
  ... their results make sense. And they show evidence of how the
  cross-validation likelihood performance is correlated with actual performance.

- **Needle Insertion**:

- **Needle Bin Picking**:

The major downside from these experiments is that they are not super long. It
would be interesting to apply this to suturing, for instance, like the TSC paper
did (but that one only tested whether they could identify *transition* points).
