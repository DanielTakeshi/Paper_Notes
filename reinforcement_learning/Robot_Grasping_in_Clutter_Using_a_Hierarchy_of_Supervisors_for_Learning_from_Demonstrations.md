# Robot Grasping in Clutter: Using a Hierarchy of Supervisors for Learning from Demonstrations

I read this paper in detail about a year ago (spring 2016). I will re-read it
again to refresh my memory.

Here are the paper highlights:

- This is a Learning from Demonstrations task, but with a *hierarchy*, so it's
  NOT just humans providing demonstrations. The three tiers are: a fast motion
  planner, crowd-sourced human workers, and a human expert. The motion planner
  is cheap, but bad, in part because it only moves in a straight line. The human
  expert is great, but expensive. The crowd-sourced workers are in between. I
  see, they build upon DAgger, and that requires expensive human supervision. So
  this paper considers the budget/tradeoff.  That's great!

- The major experiment in this paper is a singulation/decluttering task. They
  are trying to move objects out of the way so that a robotic arm can reach
  towards the golden "goal" object. (They use a similar experiment in their
  subsequent [follow-up ICRA paper][1], which uses the same robot and a similar
  interface for how the human can "correct" or "guide" the robot.)

- For developing a policy (since, after all, that is the goal of LfD) they use
  deep learning with the camera image as input. The camera image is actually
  turned to 0/1 binary-scale. Heh, I remember when I complained about this to
  the authors last year. One reason why they do this is that they don't assume
  knowledge of dynamics, so learning is the way to go. With dynamics, it's
  better to make use of that information. Remember that the data is accumulated
  via DAgger.

CASE is not a top-tier conference, though this paper is definitely well above
the CASE threshold. I like the idea and the experiments are acceptably
convincing and reasonable. I also enjoyed the (input camera) to (action) aspect,
from Deep Learning. Actually, this paper's experiments seem *better* than their
ICRA paper (though that paper has better theory). Admittedly, though, the
figures could look somewhat better but that's a minor nitpick.

[1]:https://github.com/DanielTakeshi/Paper_Notes/blob/master/reinforcement_learning/Comparing_Human-Centric_and_Robot-Centric_Sampling_for_Robot_Deep_Learning_from_Demonstrations.md
