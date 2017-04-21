# Active Perception and Reinforcement Learning

This is a very old 1990 paper, and gives a flavor as to what the quality of
research was like back in those days. This paper also mentions "attention" and
must have inspired some recent work on attention models. (Though this paper
hasn't been well-cited in recent years.)

Their modeling assumptions vary slightly from the standard MDP stuff I'm used
to (again, welcome to 1990 research) but it can be converted over, and more
importantly, they also maximize the sum of discounted rewards as I would. Yay. 

This work is about trying to do this while also having *perceptual aliasing*.
Unfortunately, it's not clear to me what they mean by that. My *guess* is that
by that term, they mean multiple real-world states get mapped to a common
internal representation (i.e. "featurization") and thus they get aliased and
have the same utility (value) function, even though the actual states might vary
considerably in real-world value.

To overcome this, they propose an algorithm which lets them perform several
*perceptual* actions to adjust the state configuration, doing so in a way that
changes the *sensors*, but not the actual world state. This results in several
different viewpoints of the world. *Then* they perform a "real" action. This
makes sense. (Unfortunately, they don't describe *how* to choose the most
informative set of perceptual actions w.r.t. a cost budget.)

What doesn't totally make sense is the need to have a "lion" state as they
describe it. The lion, roughly speaking, is an internal state that best matches
the outside, real world state. I *think* this is like what happens in POMDPs?
Maybe they're equivalent but the literature at that time doesn't explain it
clearly enough.

This seems more about how to choose the proper internal representation of the
state and not necessarily the "best" way to actually achieve that.
