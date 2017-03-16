# Third-Person Imitation Learning

[Reviews at OpenReview][1]

This is another imitation learning paper, in the flavor of generative
adversarial imitation learning where inverse reinforcement learning is not
needed (or at least, avoiding IRL's weaknesses). The interesting part about this
paper is the definition of a new problem setting: **third-person imitation
learning**. Rather than observing the expert trajectories directly (in "first
person"), the agent now only observes them in "third person." 

Their official definition is concisely stated in **Section 4**. Note in
particular that they assume *two* Markov Decision Processes. Here's another way
of framing it:

> Here third-person refers to training an agent to correctly achieve a simple
> goal in a simple environment when it is provided a demonstration of a teacher
> achieving the same goal but from a different viewpoint; and unsupervised
> refers to the fact that the agent receives only these third-person
> demonstrations, and is not provided a correspondence between teacher states
> and student states.

Why is their work unique? They argue:

> This past work, however, is not directly applicable to the *third* person
> imitation learning setting. In third-person imitation learning, the
> observations and actions obtained from the demonstrations are *not* the same
> as what the imitator agent will be faced with.

A notable challenge is that we need to be sure that we're defining an
"interesting-enough" problem. The third-person setting has to be notably
different from the first-person setting.

Their algorithm combines two related works:

- Domain confusion, from computer vision (unfortunately, I'm not familiar with
  this).
- Generative adversarial networks (OK, I know this, a bit!).

And they benchmark on three Mujoco problems, with TRPO as the expert:

- Pointmass (expert trajectories are at a different angle)
- Reacher (same as Pointmass)
- Pendulum (expert trajectories are in a different color)

This might not sound much harder to humans, since we can relate different angles
and colors easily, but this is at the pixel level and the computer only sees
pixels. OK, this sounds reasonably convincing to me. And also, their algorithm
only looks at raw sensory data, where no features are available. This is pretty
much the bare minimum requirement nowadays.

Their algorithm requires a cost function formulation. They base it on GAIL by
adding more terms to the agent's objective (which for GAIL is to maximize the
discriminator's cross entropy, i.e. make the discriminator perform poorly in
classification, since classifiers are supposed to *minimize* cross entropy).
They say that GAIL will not do well in the third-person setting since the
discriminator can tell the difference between the two settings, but they should
have run an experiment to prove that.

OK, so how do they resolve this? This is a bit confusing for me. They argue that
they can split up a deep network into two parts: a **feature extractor** and a
**classifier**. This seems wrong to me, because all weights do contribute to the
final classification and to feature extraction. As a *general rule* I can agree
that features are extracted in the first few layers, but I don't see why they
can declare a hard split. The notation in this paper is also confusing.
**EDIT**: after thinking about it, why not use two separate networks, and have
the output from the feature extractor be input to the second one?

It looks like they're arguing this by adding in a mutual information criteria.
Unfortunately, it mostly reflects me to other literature. I think the "domain
confusion" intuition is that the discriminator will be confused about which
policy the inputs (i.e. states) came from.

They also provide two time steps, which makes sense. What's more confusing is
that function G which flips the sign. Really? They have to do this to destroy
the signal from backpropagation.

Yeah, I'll need to look at this in more detail.

This paper is a fast read other than the math formulation for their setting,
which unfortunately mostly refers us to other work. I wonder if there are other
possible new problem settings?  Also, what other insights can we derive from
human behavior?

[1]:https://openreview.net/forum?id=B16dGcqlx&noteId=B16dGcqlx
