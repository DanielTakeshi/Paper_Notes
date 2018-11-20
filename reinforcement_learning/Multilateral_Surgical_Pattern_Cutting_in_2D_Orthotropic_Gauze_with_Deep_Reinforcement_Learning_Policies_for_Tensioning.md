# Multilateral Surgical Pattern Cutting in 2D Orthotropic Gauze with Deep Reinforcement Learning Policies for Tensioning

**Main idea**: design policies for surgical cutting robots that rely on
*tensioning*. The surgery task uses two tools, (1) a grasping tool and (2) a
surgical cutting tool (i.e. scissors), and the robot's objective is to use those
tools to successfully cut a pattern (typically a circle) on a small sheet of
surgical gauze. I know from watching the lab videos that the gauze is suspended
at the four corners, which is easily done in simulation.

**Intuition**: it is challenging to cut through the gauze to accurately match
the target pattern on it. One way to improve this is to use *tensioning*, where
the grasping tool can grab part of the gauze to make cutting easier. Analogy:
think of cutting a soft fabric. Just cutting it normally like we would for paper
might not work since the scissors don't have the appropriate force to slice
through.

To *perform* the cutting, use **deep reinforcement learning** (**Trust Region
Policy Optimization**) to learn *tensioning* specifically. AKA this is not
Learning from Demonstrations. TRPO is the default for continuous control tasks,
such as what we have here (the RL learns a policy for the robot *controls*). In
addition, as it is costly to collect data on this, the robot needs to use a
**finite element fabric simulator** to simulate datapoints; performance
therefore depends on the accuracy of this simulator. It's described in Section
IV but there's not much I can say/critique about it given my lack of simulating
experience and limited physics knowledge. The simulator models the fabric using
a rectangular mesh of vertices (i.e. grid) so there is a known fidelity level,
and more vertices means higher fidelity.

The scissors actually follow a *pre-planned* route, while the tensioning is done
by providing the robot with tensioning directions, which must be what the
learned policy will do. The **reward** is the symmetric difference between
desired contour (provided as input) and actual cut contour (this makes sense).
It's also the evaluation metric so this means the reward is perfectly ideal.


## The Policy/Algorithm:

They call it **Tensioning Policy Search**, see Algorithm 1 and Figure 2. Here it
is in order for my intuition:

- We are given a closed or open curve which does not intersect itself. This is
  the cutting *target*.

- The curve must be decomposed into *cutting segments*. They mentioned "8-12
  fudicial points" but that's for their *state space* (more on that later) not
  the number of segments. I don't know how many they use; they never report it??
  Most likely it's on the order of 3-5 since they have to brute-force over
  orderings later. Segmentation is needed because of a limitation with the dVRK
  robot since it can't cut continuously without its two arms colliding.
  Unfortunately it's unclear how we find these segments. They just say "find
  local minima and maxima of the curve" but *what are those*?

- We then sample grasp points for tensions, i.e. where the gripper holds the
  gauze. It looks like points are sampled uniformly across the gauze.

- For *each* sampled grasp point:
  - Find the best ordering among the segments by brute-force searching (and best
    = performance on a "fixed tensioning policy"). It may be better to try and
    have an algorithm to search for the best ordering. Thus, after this step we
    have a **cutting trajectory**.
  - Then with a cutting trajectory and single grasp candidate point, do DeepRL,
    **TRPO** with a fully connected neural network to learn parameters \theta to
    control the gripper's tensioning directions. Number of parameters isn't that
    large I thnk, as the net is 2-5 layers (the paper is really unclear about
    the network architecture). TRPO is designed to find a *continuous control*
    policy, so given a state, we must find the 3-D direction for the gripper to
    move. It looks like action space is in [0,1]^3 so 3-D continuous vector with
    values bounded by 0,1. Right, that makes sense, remember that the gripper is
    staying fixed at the grasp location so it is limited in its range (but it
    can move in one direction several times to "go further"). By the way, the
    *states* are vectors with information about "location of [8-12] fiducial
    points chosen randomly on the surface of the gauze". It is NOT the full
    sheet or raw pixels, just to be clear. I assume the curves are fixed
    throughout so the tensioning will mean those points reposition themselves in
    principled ways.
  - (This sounds like a lot of work, but we can run this in parallel for each
    grasp point.)
- We score each tensioning policy which, remember, came out of a single sampled
  grasp point. Then we choose the best one.

It would be interesting to know the typical ordering of the cutting. Their
algorithm seems to cut discontinuously, but I imagine it might be easier for the
robot to cut continuously for simplicity. Or maybe not?

**Intuition**: surgical gauze in place, held by four corners, with some curve on
it. A cutter and a gripper. The gripper grasps a point on the gauze ... **and
stays there throughout one RL algorithm run or test curve**. The states are
fixed points which at the start are randomly sampled among the gauze points. The
gripper continuously holds its spot on the gauze but *changes directions/force*
to change tensions. While doing so, the cutter cuts each segment one by one in
the ordering that results in estimated highest scores, which is *not* generally
a continuous clockwise or counterclockwise ordering of the segments. The robot
arms are constantly rotating and changing to make room for each other; Figure 5
shows some robot limitations (the cutter can only cut in one cardinal direction
so I assume the gauze must be rotated appropriately).


## Experiments

Three major ones:

- Simulation, 17 open/closed curves, compare with three others: (1) no tension,
  (2) fixed, and (3) analytic.
  - No tension: just a single arm cutting, no tension unless we count the four
    ends which hold the gauze (no they don't count ;) ...) so this is *no
    policy* at all, right? I assume what happens is the robot "sees" the exact
    curve and the cutting trajectory follows it.
  - Fixed: we keep it fixed at one point (just like in their real algorithm)
    *BUT* apply no directional forces. So the gripper just stays in place.
  - Analytic: we can compute forces/magnitude and determine where to put
    tension. This seems like it should be the best but actually DeepRL is better
    ...
- Simulation again, this time only for their algorithm but gauge noise
  robustness.
- Then 4 curves on a *real* **da Vinci Research Kit (DVRK)**. It's supposed to
  be relatively complicated to use. :(

Lots of detail on these! Table I shows extensive results for the first of the
experiments above, and fortunately, also has images of the curves. Yay! There
are a few failure cases but I suppose there is not enough room in the paper to
describe it.

The robustness: yeah, first and second make sense, former decreases resolution
of the grid, and the latter adds noise to a bunch of stuff (always an option).
The modeling error is a bit more mysterious.

Real robot: not much to say here, the section is a bit shorter and still shows
their algorithm is better **BUT** it's better in 3 out of 4. That's a very small
sample size, unfortunately. Also, they can't perform two of the baselines.


## My Thoughts and Takeaways

There's lots of depth in the experiments, but not so much breadth. The real
robot examples are impressive and has clear applications. The downside is that
there are many assumptions one has to make, and reproducibility is difficult to
impossible due to the highly-specialized nature of this research.

The cutting is also planned out beforehand. Is it possible to learn the cuts as
we go, along with the tensioning?

In addition, is it possible to relax the assumption of the gripper staying at
one point? Otherwise is it possible to learn the best grasp point, i.e. to do
better than uniform sampling across the gauze? They said the operation wasn't
differentiable, but the max function is differentiable almost everywhere.

I still want to know more about the cutting trajectories. Are they typically
continuous, clockwise/counterclockwise, or completely random, one segment, then
another segment which is not generally next to it, etc., until all segments have
been cut. That requires extremely fine-tuned controls but dVRK is for surgery so
it better be like that.

I suggested trying out CNNs when I first reviewed this. The state space they use
doesn't require CNNs but it seems like it's better to get the complete picture
of the gauze, not just 8-12 special points.
