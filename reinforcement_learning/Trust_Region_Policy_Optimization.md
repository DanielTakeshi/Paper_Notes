# Trust Region Policy Optimization

It's about time I got to reading this. I've known about TRPO a lot and it seems
to be the standard for policy gradients nowadays. I didn't want to use this
algorithm a lot without understanding the technical details. I know it's like a
policy gradient step with a KL divergence constraint to prevent dramatic
changes, but that's about it. So here goes.

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

Two variants of TRPO: **single-path** and **vine**, see Figure 1 for a visual.


## Experiments

They test with the following:

- **Simulated Robotic Motion**: these are from Mujoco. This being back in June
  2015, they only tested with three environments: *swimmer*, *walker*, and
  *hopper*. I know the second and third, but not the first ... huh, why is that?

- **Atari 2600 Games**: they test using the same seven games from the workshop
  paper that introduced DQN. OK, that makes sense (they didn't have to test with
  40-50 games!).

They were able to get state of the art performance despite minimal prior
knowledge and hand-architected policies (e.g. for Mujoco, hand-architected
policies explicitly encode balance). They said:

> To our knowledge, no prior work has learned controllers from scratch for all
> of these [Mujoco] tasks, using a generic policy search method and
> non-engineered, general-purpose policy representations.


## Conclusions, Thoughts, Takeaways, Going Forward

Wow.
