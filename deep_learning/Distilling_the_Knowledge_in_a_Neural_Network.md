# Distilling the Knowledge in a Neural Network

A workshop paper with limited results, but has tremendous interest. :-)

The main idea is simple: how do we transfer knowledge from one (teacher) neural
network to a (student) neural network? Usually the student is much smaller so
that this can be viewed as compression.

To train, we train the student network to match the *softmax* of the teacher
network. The softmax can be softened with a temperature in case the non-argmax
cases are too small to be of use. The idea is that the non-argmax information
contains a rich training signal that should be utilized.

There's also some interesting stuff about ensembles, but it uses an internal
Google Dataset.

Overall I wish there was a LOT more work on this ... but I think that's what all
the 700+ citations have done.
