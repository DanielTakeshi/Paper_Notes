# Model-Driven Feed-Forward Prediction for Manipulation of Deformable Objects

(This journal paper combines several of the 'laundry robotics' stuff from the
folks at Columbia University. [Looks like the website is here.][1])

Main contributions:

- Use a pre-computed and simulated database of deformable objects (i.e., cloth
  and garments) and efficiently retrieve from it as needed to optimize
  trajectories for physical robots to manipulate similar, but real, objects.
  Their task is *folding laundry*.

- How does the simulation work? Use "mesh models" for garments and change the
  pose with gravity applied. They use Maya as the physics engine for simulation.
  I remember using Maya many years ago. :)

- Uses "non-rigid registration" which means trying to "align" different things
  together (by "things" I refer to anything, like images, which are stored in
  a computer with numbers that can be matched), but without a rigid body
  transformation.

- A lot of their experiments involve the Baxter robot rotating an object, which
  isn't something we currently do with the HSR or Fetch, though I assume it's
  possible. They use the Kinect camera to get a depth image for the cloth, which
  can then be used for matching (i.e., registration) in the database.

This paper combines prior work from 2014/2015 (TASE is a journal so I assume the
referee-ing process was delayed), so it's before the Deep Learning in Robotics
revolution.  "Feed forward" and "model-driven" do NOT mean "feed forward neural
nets" and "model-based RL or learning a transition model," respectively.

I think it differs from the earlier work of Abbeel/Goldberg (2010-2012 ish) by
not focusing on aligning edges and corners, and also because they use a
database.

The nice thing: the garments start at a random and messy state, similar to
Abbeel's towel folding.

A bit unclear on this statement, though:

> None of the previous works focus on trajectory optimization for garment
> folding, which brings uncertainty to the layout given the same folding plan.
> One possible case is that the garment shifts on the table during one folding
> action so that the targeted folding position is also moved. Another case is
> that an improper folding trajectory causes additional deformation of the
> garment itself, which can accumulate.

What do we actually mean by 'trajectory optimization' and why are
Abbeel/Goldberg's work not 'trajectory optimization' wrt the folding?

I'm surprised I didn't find this when writing our bed-making paper. I will
rectify that ... and also check out if Maya makes sense for other projects.

[1]:http://www.cs.columbia.edu/~yli/garment_db/
