# Learning Actionable Representations from Visual Observations

This is an extension of the "Time Contrastive Network" paper, another one of
those that I have heard but have not read through in detail. But the main idea
and contribution of *this* paper are hopefully OK:

- We assume we have two (or more) camera views of the *same* trajectory.

- Then, we can align the time steps. This gives us a supervisory signal for the
  question of determining whether two different frame clips represent the *same*
  "thing/scene", except just at different viewpoints.

- Design a CNN which takes these raw images from video, and convert to features.
  These are what they call the learned "representations" or "embeddings."

- (The agent can either watch a video of itself, or more impressively, watch in
  third person.)

- Then, run *PPO on top of those representations*. PPO is usually applied on the
  "actual" state, which are the joint angles (and velocities?). See OpenAI
  baselines, for example. Here, they show that PPO works just as well using
  these actionable representations, which is better because normally we (humans,
  when watching video) won't have access to a "true" state. They report on this
  experiment *after* confirming experimentally that their jlearned 
  representations can encode position and velocity.

I think the main extension over TCNs is that they can deal with multiple
consecutive frames. Incidentally, this allows them to encode velocity.

The nice thing is, given two frame clips, you can get *six* supervisory signals:
four negative attractions, and two positive attractions. That's a lot of data!

There are other technical details needed for this to work, such as ensuring
frame clips are at least `alpha` frames apart, etc. Unfortunately, the loss
function is not totally clear to me because they cite another paper for that ...
but it says "n pairs loss" and that makes sense. **Make the similarly-timed
frame clips similar to each other in the learned embedding space, while other
pairs of clips should be dissimilar**!

This is one of a long line of recent (past few years) work that use Deep
Learning for the purposes of learning from videos, since that's something that
humans do readily, and we want AI to do that. This is additionally about "self
supervision" which uses structural priors like temporal consistency to learn
something (usually without explicit labels like in standard supervised
learning).
