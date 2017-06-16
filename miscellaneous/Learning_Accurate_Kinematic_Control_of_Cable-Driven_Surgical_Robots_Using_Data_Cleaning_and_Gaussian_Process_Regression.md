# Learning Accurate Kinematic Control of Cable-Driven Surgical Robots Using Data Cleaning and Gaussian Process Regression

TL;DR: this paper performs automated debridement (not suturing!) with extra
features of data cleaning and GPR for learning mappings from observed to desired
states.

What are the key aspects/features of this paper?

- The use of **Gaussian Process Regression**. I know Ken's done some work on
  this. As they say:
  
  > Gaussian Process Regression (GPR) is a data-driven technique that can
  > estimate a non-linear mapping between control inputs and sensed kinematic
  > responses. 

  Hmmm ... what are the "sensed kinematic responses"? I assume we also need some
  Gaussianity assumption somewhere in the input (see Kevin Murphy's book for a
  description of Gaussian Processes). I think this boils down to us managing
  joysticks which then translate to positions of end-effectors (or just
  positions) of the Raven surgical robot's arms. But we're going to be
  autonomously programming the joysticks --- maybe "joysticks" aren't the right
  word but in theory computers can programmatically apply the same forces we do
  in person --- so that we want what the computer does to map to something good
  in the actual "kinematic responses" which are the Raven's arms/end-effectors.

  This mapping must be the function **F** that they use in the paper, mapping
  between *observed* and *desired* states. But what *are* the observed and
  desired states?!?

  The contributions to the GPR aspect are specifically (i) **adding velocity as
  a feature** and (ii) **using data cleaning**. I can see how both would be
  useful (well, data cleaning should be useful!), though I need more intuition
  on the velocity. By adding it as a feature, we're just making the design
  matrix (if this were linear regression) have one extra component. 

- The "states" seem to be (pose, derivative(pose)), that's it. But it's still
  not good enough for my intuition.

- The data cleaning part may or may not be a critical part in the pipeline, and
  I only question this because in virtually any setting, we'll do *some* form of
  data cleaning, whether informally or formally.

- Experiments use the **Raven II surgical robot** and follows the setup of Kehoe
  et al (ICRA 2014), which I've already read but should probably review. What
  seems to be surprising is that these two papers have roughly the same amount
  of citations (29 and 32). I would have thought that Kehoe's paper, the first
  to perform autonomous debridement with Raven, would have more citations.

  This paper unfortunately doesn't even have a picture of what the damaged
  tissues look like for debridement.

  But anyway, they did mention they'd like to apply this on the dVRK, but I
  don't know if they did that.

There are a lot of surgical robotics papers. I need to get a diagram which
shows a relation mapping between the papers.

Also, Ken Goldberg really likes long paper titles.
