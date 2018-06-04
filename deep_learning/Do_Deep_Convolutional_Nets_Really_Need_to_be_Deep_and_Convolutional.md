# Do Deep Convolutional Nets Really Need to be Deep and Convolutional?

Intriguing paper arguing that deep CNNs really need to be deep (and have
convolutional layers). 

But what about distillation techniques? They empirically argue that even though
the student networks may be smaller than the teacher (which is one of the main
benefits of knowledge distillation and similar strategies), the student still
needs a sufficient amount of convolutional layers to attain similar performance.
Some of this paper seems to argue that the results of "Do Deep Nets Really Need
to be Deep" (NIPS 2014) do not generalize to domains such as image
classification.

Evaluation in this paper is done on ... CIFAR-10. Ah well, this was late 2016
work, hopefully we've gone beyond that point to CIFAR-100 as evident by the
*Born Again Neural Networks* paper.

Also, I wonder how much of this depends on whether the nets are allowed to be
"wide", as in have a lot of filters in one conv layer, and lots of hidden units
on one hidden layer? Hopefully that kind of stuff is considered under a
"parameter budget" for fair evaluation.

For evaluation, the main idea is to use Bayesian optimization to search over
architectures and learning hyperparameters, and then show that even the most
accurate among these shallow models doesn't match the performance of deeper
models. They also are careful to optimize the teacher as well, arguing that this
was a weakness in prior work.

A nice, thought-provoking paper!
