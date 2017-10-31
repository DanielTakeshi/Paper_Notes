# Improving Generalisation for Temporal Difference Learning: The Successor Representation

This was the original paper that introduced the successor representation, in the
context of linear function approximation in discrete, finite MDPs where we can
describe transitions using transition matrices. This was 1993, remember, so we
didn't need to cram text into pages and have detailed plots, but it also means
the description and context is usually less clear.

The application is a maze with a barrier, but all I get out of this is that the
SR can tell us that states close to each other but on opposite sides of a mze
are actually "distant" in feature representation. But I don't see how this
*improves generalization*, and that's what I wanted to understand from this
paper.

I'm having a hard time understanding what this paper is trying to say,
unfortunately.
