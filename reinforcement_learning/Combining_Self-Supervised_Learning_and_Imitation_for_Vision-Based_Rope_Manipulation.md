# Combining Self-Supervised Learning and Imitation for Vision-Based Rope Manipulation

https://danieltakeshi.github.io/2018/03/23/self-supervision-part-1/

A nice, niche self-supervised robot learning paper about rope manipulation.
Main contribution is on self-supervised learning of a predictive model
specifically for rope manipulation, and being able to do tasks at test time when
given one human demonstration.

The paper is easy to read and the contribution is informative and impressive,
mostly about the rope part. I like the details in the setup, which are clear
enough for the reader to mentally reproduce. I understand how the method works.
The self-supervision is very cool, though it seems like they mostly build on the
earlier NIPS 2016 paper on "Learning to Poke by Poking."

Downsides? Well, they had to run the robot for 500 hours to get 60k
interactions. I wonder if a 10x reduction is possible. Unfortunately, without
the same setup and (important) the same Baxter robot, it's near impossible to
get a fair comparison.  It's also task-specific, they need one human demo for
each task, because they need to feed all the `I_t'` goal images.

Their hand-engineered baseline is a bit weak:

> In the absence of a model of rope dynamics, a simple way to move the rope into
> a target configuration is to pick the rope at the point with the largest
> deformation in the first image relative to the second and then drop the rope
> at the corresponding point in the second image.

I'm pretty sure there are more advanced models of rope dynamics, but that would
be hard to implement. However, to be fair, their nearest-neighbor and
no-imitation baselines are reasonable baselines.

Other "downsides" I can think of may have been addressed in the "Zero-Shot
Visual Imitation" paper or should be best thought of as future work. 

I suppose they could try and make this part learnable:

> In some cases the robot predicts a pick location that does not lie on the
> rope. For these cases we use the rope segmentation information to find the
> point on the rope that is closest to predicted pick location to execute the
> pick primitive.

So really, the actual act of *identifying* the rope is not the issue. It's *what
will happen after the rope is moved* that is unclear, and that's why they need
to learn a model automatically. If it were just identifying the model, we'd have
no need for learning.

Results: the standard deviation bars are a bit hard to read ... I like "error
regions" better but it's enough for ICRA.

A few questions to ponder:

- Confusing notation: they have V (non-italics) and V (italics), but both are
  described as: images encountered in a *human* demonstration. (??)

- Related note: despite how I know it works, I find it very surprising that they
  can get away with trying to transform I_1 to I_2, then I_2 to I_3, etc. Isn't
  there covariate shift? Or is the task too simple? For instance, if the robot
  gets input `(I_1, I_1')` but messes up, then what if `(I_2, I_2')` is
  impossible? Say the action makes the robot completely out of reach? I think
  the task may be easier because they can use perception to detect where the
  ropes are, so they can project a predicted grasp point towards the nearest
  rope point.

- Active data collection: I think I know what they did (basically, use the
  inverse model trained so far to predict useful actions... then execute those
  actions). Seems reasonable but might be self-reinforcing to lack diversity in
  actions ... they mention they didn't have enough time to test this. (TODO:
  re-read this part.)

- They say that predicting all three (discretized) action attributes is "naive"
  as they mention in Section IV-B, but it would be better to show the relative
  parameter savings in this part (they have room in the paper, after all).

- What about having no human demos? I think that's what the "Zero-Shot Visual
  Imitation" paper at ICLR 2018 did ...

- In the inverse model, when to use (s(t), s(t+1)) versus when to use (s(t), g)
  where g is the goal state? I had this question for the NIPS 2016 paper.
