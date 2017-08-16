# Curriculum Learning

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
