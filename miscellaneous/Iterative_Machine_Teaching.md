# Iterative Machine Teaching

Considers the case when both the teacher and student are linear models, and when
there is repeated interaction between the two. The student provides information,
then the teacher provides an example (just one per iteration though multiple
examples is certainly possible) and the cycle repeats. The goal is for the
teacher to pick examples such that the student quickly gets closer to the
optimal weights. The student learns via SGD and assumes a convex loss function.

They define various categories of this setup, depending on the teacher's extent
of the student:

- **Fully Omniscient**, knows everything about the student. Here we assume
  synthesis, combination, or pool-based teaching capabilities, ordered from
  least restrictive teaching technique to most restrictive teaching technique.

- **Surrogate Teacher**, can query the output label from the student but cannot
  access the weights. Optimize the surrogate loss function by utilizing the
  first-order convexity condition.

- **Imitation Teacher**, learns to approximate the student's function, `<w,x>`.

(I'm still unclear on the difference between the last two?)

Then they present experiments: (1) teaching linear models on Gaussian data, (2)
teaching linear classifiers on MNIST, (3) teaching fully connected layers in
CNNs, and (4) ego-centric visual data of infants. 

For (1) the teaching outperforms SGD (which is just like a random teacher), as
expected (and required for this to be an interesting research result).

For (3) they use binary problems, so only 0s vs 1s for MNIST, or 3s vs 5s, etc.
The data are 24D (really?? Must be a lot of PCA ...).

For (4) they are NOT teaching the FULL CNN parameters, but just the final linear
layer of it (huge catch here, for full CNN teaching it's best to use Born Again
Neural Nets ...). Applied to CIFAR-10. We can view the stuff before the linear
layer as a featurization.

For (5) the dataset was reduced to 64D via PCA and then we apply the usual
linear model.
