# Residual Reinforcement Learning for Robot Control

Given a task, decompose it into a part that can be solved with traditional,
classical methods (control theory) and a part that uses RL. That's the
'residuals', reminds me of the work that Ken Goldberg's lab has done.

> In this paper, we study control problems that are difficult to approach with
> conventional feedback control methods. However, the problems possess structure
> that can be partially handled with conventional feedback control, e.g. with
> impedance control. The residual part of the control task, which is the part
> that must consider contacts and external object dynamics, is solved with RL.

I think contacts (i.e., collisions) are perhaps the main difficulty involved in
designing physics simulators. That's why the RL part can be used since we can
just learn from the reward, and we can learn complicated functions (i.e., the
collisions and results from it) rather than design it ourselves with flawed,
approximate physics models.

Their application domain is with manufacturing --- **block assembly** --- as I
probably should have guessed from how Siemens is involved. I also assume
covariant.ai might be onto this.... There's a simulation and a physical/real
environment. I need to ask them how they design their MuJoCo environments ...

The rewards are of the form:

```
r_t = f(s_m) + g(s_o)
```

where `f` represents a geometric relationship, moving the robot gripper, while
`g` is a general class (nonlinear) representing reward for keeping the blocks in
the right position. The control is simply:

```
u = pi_H(s_m) + pi_theta(s_o)
```

where `pi_H` is human-designed, and `pi_theta` is learned via RL. Optimize these
jointly -- see paper for details. Reminds me of important considerations with
DQfD, with mathematically dis-similar training signals. *But, a bit confused: it
seems like they develop their hand-engineered policy `pi_H` independently and
before we do any `pi_theta` optimization*. Hence, isn't this just a two-step
process? Not joint optimization?

As usual, we should be careful interpreting the conclusions. The videos show
very preliminary results. It's far away from manufacturing capability, but might
be closer than I think if Siemens actually needs stuff like this in practice.

This paper reminds me of the literature on combining IL and RL (i.e., DQfD,
etc.) because enabling RL for certain tasks can be done by decomposing the task
in a similar way, except this time treating the IL part as a separate aspect
which can be used to get the agent to a good initial state, and then we do RL.
There's a lot of literature on tasks which RL is used as a subset of something
(in fact even GAIL can be viewed like this, since it's imitation learning but we
still do 'TRPO steps'...).

They mention the contrast with this and the demonstrations stuff:

> However, providing demonstrations requires humans to be able to teleoperate
> the robot to perform the task. In contrast, our method only requires a
> conventional controller for motion, which ships with most robots.

