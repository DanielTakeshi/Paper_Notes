# Learning to Play with Intrinsically-Motivated Self-Aware Agents

Looks like another self-supervision paper about developing forward and inverse
models via exploration and deep neural networks.

**Main novelty/contribution**: they argue their novelty over some of Pulkit and
Chelsea's work by avoiding the need to take random actions to generate training
data. (To be clear, the "Learning to Poke" paper, or any self-supervision one
for that matter, actions are never truly random but are hand-designed to make
the data interesting. What the authors really mean is that they have a
mathematical way to bias actions towards interesting outcomes, not that it's not
random. Of course, this was kind of what the "curiosity-driven" paper did, and
that one added that value to the reward obtained.)

The closest work, as I expected, was Deepak's curiosity paper. They claim the
novelty is going from 2D to 3D, and the technical part is having a explicit
model for the agent's belief of the world (like model-based). This thus lets
their agent learn more sophisticated behaviors that can be transferred to other
tasks. 

The notation is a bit more cumbersome than the curiosity paper.

**Why does it work?** 

- Because we know self-supervision, curiosity-driven motivation for rewards, and
  mass data collection (+Deep Learning) work.

- Key is their policy which "antagonizes" the world, so it provides surprising
  events that act as key learning signals.

One drawback (for now) with the paper is that the physics simulator seems new so
unclear how accurate it is or if others will adopt it as a benchmark.
