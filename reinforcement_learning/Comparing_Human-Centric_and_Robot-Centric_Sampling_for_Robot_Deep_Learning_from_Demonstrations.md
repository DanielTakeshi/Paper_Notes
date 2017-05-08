# Comparing Human-Centric and Robot-Centric Sampling for Robot Deep Learning from Demonstrations

## The Main Ideas

There are two main classes of algorithms for learning from demonstrations (from
*human supervisors*):

- **Human-Centric Sampling**: the human demonstrates to the robot what to do,
  via "teleoperation." I assume this is like physically guiding a robot's arm to
  move, except the "physically guiding" part is dome via computer connection?
  Then the robot tries to imitate by framing the human trajectories as a
  supervised learning problem, e.g. with behavioral cloning.

- **Robot-Centric Sampling**: this is when the *robot* performs its own
  trajectories, but the humans provide *corrections*. (This happens with
  DAgger, which I *really* need to better understand at some point ... and they
  also cite Michael Laskey's other paper from CASE as an example of this .. I
  think I agree.)

OK ... just from this, it seems like the robot-centric sampling will be better
but let's read on ... OH, I see, during *deep* learning from demonstrations, the
human-centric method can be better and this seems to be the paper's main
conclusions.

In addition, here's what they mean by "teleoperation":

> Teleoperation uses an interface such as a joystick or video game controller to
> control the position of the robot end effector.

So I was right on that.


## Technical Details

The notation comes from control theory and their modeling assumptions make
sense. Whew. They use a **supervisor** (indicated by "theta-star") and a
**surrogate loss function** to measure the difference in controls. This
"difference" is from the supervisor's vs the current robot's controls. To be
clear, the supervisor is fixed, and the robot is the one learning. Look at
Equation 1 to understand the objective.

The **theory** in the paper is expressed in their abstract:

> We prove there exists a class of examples in which at the limit, HC is
> guaranteed to converge to an optimal policy while RC may fail to converge.
> These results suggest a form of HC sampling may be preferable for
> highly-expressive learning models and human supervisors.

The analysis is formed in the "regret framework" of online learning; see Peter
Bartlett, Shai Shalev-Shwartz, and others for details. I haven't gone through
the details of this (yet).


## Experiment Details

There are three main sets of experiments:

- **GridWorld**: The advantage here is simplicity and that they can randomly
  generate lots of (slightly) different scenarios. The downside is that this is
  relatively trivial and it already seems intuitive that behavioral cloning-like
  algorithms can do a good job with a perfect supervisor and a deep model.

- **2D Point-Mass**: Similar to what I read in *One-Shot Imitation Learning*
  paper. The robot is a point mass and has to arrive at a specified 2-D
  coordinate. They show that HC outperforms RC.

- **Physical Robot Singulation (Object Separation) Task**: They recruited 10 UC
  Berkeley students to provide demonstrations via xbox controllers. This is the
  "human" aspect of this paper.


## Thoughts and Takeaways

The paper's main conclusions seem to be that with "classic" machine learning,
robot-centric methods for LfD are better, whereas with "deep learning"
human-centric methods are superior. Interesting ... though with many papers like
this, the experiments are sometimes either not convincing or they make sense but
were tested in environments in which the conclusion would probably be known in
advance. I'd like to see a situation when an experiment was genuinely
surprising. This isn't a knock on the authors but more about ICRA (and other
papers) papers in general.
