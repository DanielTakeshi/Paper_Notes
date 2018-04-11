# Curriculum Learning

**Update April 11, 2018:** I read the paper again, this time in full. The
perceptron and shape recognition experiments are informative though a bit overly
simple and artificial (2009 papers might have been like that, to be fair).  I
can't comment too much on the language modeling stuff, only to say that language
modeling has clearly gotten heads and shoulders different in the decade since
this paper was published. And also, Figure 5 doesn't seem to show *that* much
benefit to the curriculum learning scheme, and the authors claim that "2.78" and
"2.83" are statistically significant.

The curricula here are really basic, and it's unclear how to automate this or if
the same would hold true today for "realistic" data/experiments or for the
neural network architectures that we use (w/sophisticated training procedures
like Adam). The general idea of curriculum learning, though, is still very
interesting, and I see several examples of this in (simulated) robotics. 

This paper has essentially no theory, but proposes lots of interesting
directions for future work. Seems to be the standard curriculum learning
reference for modern AI papers. I also like the perspective of curriculum
learning as a transfer learning technique, since the "transferring" part is on
the harder examples.

**Old notes**:

Uses a nice concept: don't present data randomly, but in order, gradually
increasing the difficulty/complexity. After all, that's how humans learn, right?
The authors attempt to apply this to Deep Learning, though this technique can
obviously be done in many other domains. (The abstract says that curriculum
learning can get to a better local minima, but Choromanska's 2015 AISTATS paper
probably puts that claim away.)

I think this work was motivated by Hinton et al's groundbreaking 2006 paper on
the layer-wise training, which kind of motives the "start short, then get
longer" stuff. Not sure if that's still the case; we don't think like this
anymore. But note that for Deep Learning, they're arguing for smoother to
non-smooth **objective functions**.

For some more formalism, see **Definition 1** and the associated discussion,
uses idea of entropy which is widely applicable.

Experiments use the perceptron, shape recognition, and language modeling.
