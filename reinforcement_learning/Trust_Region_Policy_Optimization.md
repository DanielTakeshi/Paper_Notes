# Trust Region Policy Optimization

It's about time I got to reading this. I've known about TRPO a lot and it seems
to be the standard for policy gradients nowadays. I didn't want to use this
algorithm a lot without understanding the technical details, and I'd like to
implement it anyway. I know it's like a policy gradient step with a KL
divergence constraint to prevent dramatic changes, but that's about it. Is that
really going to help me out a lot? 

See also: [lectures notes and video from Berkeley][1]. The emphasis in the RL
class was also on natural policy gradients, though. John Schulman said the goal
was to reduce policy optimization into an optimization problem. What this means
is that we want to have a loss function to optimize, as in Q-learning (but *not*
as in vanilla policy gradients), if that makes sense. He made the analogy with
deep learning, which tries to reduce learning into (highly nonlinear)
optimization problems.


## Intuition, Ideas, etc.

They present a theoretically-justified procedure, and then the modification of
it (to make it practical) is TRPO. The theory says that they can guarantee
monotonic improvements in the policy, and TRPO "essentially" maintains this in
practice.

TRPO is a **gradient-based** algorithm; it serves as an alternative to
derivative-free strategies (CEM, CMA, and evolution strategies).


## Technical Details

(Note: I won't go through much of the math here, for that I'd need to turn to my
blog where it's easier to write math.) 

One key detail is to understand why they guarantee monotonic improvements. They
require a **surrogate loss function**.

It looks like TRPO is defined in **Equation 12**. It looks deceptively simple.

For actual implementation details, see **Section 5** where they discuss how to
approximate expectations with samples, etc. Also: two variants of TRPO:
**single-path** and **vine**, see Figure 1 for a visual. I'm more familiar with
the single-path variant, of course.


## Experiments

They test with the following:

- **Simulated Robotic Motion**: these are from Mujoco. This being back in June
  2015, they only tested with three environments: *Swimmer*, *Walker*, and
  *Hopper*. I know the second and third, but not the first ... huh, why is that?

- **Atari 2600 Games**: they test using the same seven games from the workshop
  paper that introduced DQN. OK, that makes sense (they didn't have to test with
  40-50 games!).

They were able to get state of the art performance despite minimal prior
knowledge and hand-crafted policies (e.g. for Mujoco, hand-crafted policies
explicitly encode balance). They said:

> To our knowledge, no prior work has learned controllers from scratch for all
> of these [Mujoco] tasks, using a generic policy search method and
> non-engineered, general-purpose policy representations.

It's a bit unclear how exactly they make the plot, though. They say:

> Learning curves showing the total reward averaged across five runs of each
> algorithm are shown in Figure 4.

But what is "total reward" here? The current reward of the current episode or
averaged across a window or held-out?

## Conclusions, Thoughts, Takeaways, Going Forward

Wow.

[1]:http://rll.berkeley.edu/deeprlcourse/
