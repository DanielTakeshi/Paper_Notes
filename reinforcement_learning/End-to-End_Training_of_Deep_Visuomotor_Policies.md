# End-to-End Training of Deep Visuomotor Policies

I'm reading this paper because I would like to see how to learn policies in
application with *real* robots. Their main contribution is succinctly stated in
(half) a sentence:

> [...] we develop a method that can be used to learn policies that map raw
> image observations directly to torques at the robot's motors.

Remember this at a high level as I read the paper. The "policy search" phrase,
incidentally, just means what I usually think of in MDPs, i.e. trying to
maximize sum of discounted rewards, etc. No value iteration, of course! =) They
use an algorithm called **Guided Policy Search**, so they can find policies by
supervised learning, and supervised learning is generally "easy" for us.  My
immediate question, of course, is: how do we transform this into a supervised
learning problem? There is no labeled data, correct?


## Intuition

TODO


## Technical Details

TODO
