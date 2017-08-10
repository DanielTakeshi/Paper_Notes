# Learning Accurate Kinematic Control of Cable-Driven Surgical Robots Using Data Cleaning and Gaussian Process Regression

## TL;DR Stuff

**TL;DR**: this paper performs automated debridement (not suturing!) with extra
features of data cleaning and GPR for learning mappings from observed to desired
states. It helps tackle the problem of **calibration**, or more generally
**uncertainty in kinematics**, which I know by now to be a huge problem. This is
the case with the dvrk as well:

> We consider a problem in which we have sensors external to the system that
> measure the state, however we can only control the system in its native
> coordinate frame with its imprecise native controller.

With the dvrk, I can measure stuff "external" to the dvrk (e.g.  measuring
through camera location) but I can only control it by telling it to move in the
x-y-z and yaw-pitch-roll coordinates (or the more un-intuitive joint
coordinates). This is the problem with cable-driven robots such as the dvrk and
the Raven since they are designed for teleoperation and not for automation.

**(A second TL;DR)**: We get observed states and desired states. We want the
robot to go to the desired state, so to do that we'll run command **F(x_d) =
x_c** where **F** is learned from training data using constrained least squares
and Gaussian Process Regression. The GPR can be thought of as **learning the
residuals** since the rigid transformation won't capture everything.

So in particular, `F(x_d) = H(G(x_d))`. Rather confusingly, `G` is NOT the
Gaussian Process Regression but the rigid transformation! `H` is the GPR.


## Other Paper Details

- The use of **Gaussian Process Regression**. I know Ken's done some work on
  this. *[Edit: skipping some of this because I think I get it and right now the
  lab uses random forests instead of GPR. Just think of random forests here,
  it's probably on par in performance.]*
  
  This mapping must be the function **F** that they use in the paper, mapping
  between *observed* and *desired* states. But what *are* the observed and
  desired states?!? (Update: they're 12-D vectors, see the notation.)

  The contributions to the GPR aspect are specifically (i) **adding velocity as
  a feature** and (ii) **using data cleaning**. I can see how both would be
  useful (well, data cleaning's always useful?), though I need more intuition on
  the velocity. By adding it as a feature, we're just making the design matrix
  (if this were linear regression) have more components.

- The "states" are (pose, derivative(pose)), i.e. velocity.

- The data cleaning part seems pretty simple. It's described in Section IV-C.

- Experiments use the **Raven II surgical robot** and follows the setup of Kehoe
  et al (ICRA 2014), which I've already read but should probably review.  This
  paper unfortunately doesn't even have a picture of what the damaged tissues
  look like for debridement. 

- It can be a bit confusing to reason about `x_c, x_d, x_o`. I think of it as
  `x_o -> F(x_d) -> x_d` where `F(x_d) \approx x_c` and `x_c` is the command we
  "send" when we see `x_o` and really want to be in `x_d`.

There are a lot of surgical robotics papers. I need to get a diagram which
shows a relation mapping between the papers.


## Questions

- What kind of motion planning did they use? Or is it just open-loop? [Edit:
  indeed, they do some open loop stuff at the end and show that it works well.
  But in an earlier experiment they also use trajpot as well...]

- Or did humans guide the trajectories? [Edit: no, I'm pretty sure there's no
  human guidance here at all.]

- What is precisely the difference between what I have with the dvrk and their
  PhaseSpace motion capture system? It seems like theirs is more sophisticated.
