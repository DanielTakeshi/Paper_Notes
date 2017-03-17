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
paper was written. I'm not sure why they need three (!!) fully connected layers
at the end, each of which is actually pretty small.

It also uses something new: a **spatial feature point transformation**. One
reason is that researchers often design CNNs to throw away spatial information
to increase generality and robustness. 

TODO I'm not sure I understand why this is helpful. Might need to look at
Section 5 again.

They use a pre-training procedure, which makes sense.


### Other Details

I know the usage of \pi_\theta to generate the ultimate policy. But what are the
p functions, as shown in Table 1? Maybe think of all "p" functions as system
dynamics? I need to understand their interaction.

Also, I'd like to understand where the "linear-Gaussian" stuff comes from. What
is it for?

They use Bregman ADMM (alternating directions method of multipliers). However, I
haven't been able to go through the math.


## Evaluation and Experiments

This is where the paper shines, in my opinion, since they show robots performing
tasks and it's easy for humans to see the results. There are four sets of real
robot (PR2) experiments: hangar, cube, hammer, and bottle. They also test with
Mujoco simulations.

A few points:

- The PR2 has 7 degrees of freedom, and they can control "the full torque."

- States have dimension 30-40, which I *assume* is the length of the control
  vector. If this were Atari games, for instance, that "30-40" figure would be
  "3-15" since it represents game actions. However, each of the 30-40 dimensions
  is continuous, I think.

- The policy learned is Gaussian, which ... (TODO clarify here).
