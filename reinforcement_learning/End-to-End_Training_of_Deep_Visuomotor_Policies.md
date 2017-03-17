# End-to-End Training of Deep Visuomotor Policies

I'm reading this paper because I would like to see how to learn policies in
application with *real* robots. Their main contribution is succinctly stated in
(half) a sentence:

> [...] we develop a method that can be used to learn policies that map raw
> image observations directly to torques at the robot's motors.

Remember this at a high level as I read the paper; most prior work developed
separate pipelines for different steps of the robot, which is costly and
requires lots of human engineering.


## Intuition

The "policy search" phrase, incidentally, just means what I usually think of in
MDPs, i.e. trying to maximize sum of discounted rewards, etc. No value
iteration, of course! =) They use an algorithm called **Guided Policy Search**,
so they can find policies by supervised learning, and supervised learning is
generally "easy" for us.  My immediate question, of course, is: how do we
transform this into a supervised learning problem? There is no labeled data,
correct? I may need to check the GPS literature.

Another claim in the paper:

> Our method is sample efficient, requiring only minutes of interaction time. To
> the best of our knowledge, this is the first method that can train deep
> visuomotor policies for complex, high-dimensional manipulation skills with
> direct torque control.

To answer my question about labeled data for supervised learning, they
*generate* this by executing trajectories. Interesting ...

Their GPS algorithm is split into two phases:

- **Supervised Learning**. This seems to be what trains their ultimate policy?

- **Trajectory Learning**. This seems to provide the trajectories which helps
  the supervised learning to work (to get the final policy), since otherwise
  errors compound, like what happens in behavioral cloning.

See Figures 2 and 3 for additional information. Figure 2 is especially helpful.


## Technical Details

### The CNN

The CNN has seven layers, which is low nowadays but I guess not at the time the
paper was written.

It also uses something new: a **spatial feature point transformation**.


### Other Details

They use Bregman ADMM (alternating directions method of multipliers).


## Evaluation and Experiments

This is where the paper shines, in my opinion, since they show robots performing
tasks and it's easy for humans to see the results. There are four sets of
experiments: hangar, cube, hammer, and bottle. All of these are done with the
PR2 robot.

A few points:

- The PR2 has 7 degrees of freedom, and they can control "the full torque."

- States have dimension 30-40, which I *assume* is the length of the control
  vector. If this were Atari games, for instance, that "30-40" figure would be
  "3-15" since it represents game actions. However, each of the 30-40 dimensions
  is continuous, I think.
