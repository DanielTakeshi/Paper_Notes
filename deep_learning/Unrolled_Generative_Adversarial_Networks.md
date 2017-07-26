# Unrolled Generative Adversarial Networks

This looks interesting since it (a) can apply to sequences which use RNNs, and
(b) might help tackle the infamous *mode collapse* problem when training GANs.

To improve the Generator update, they have to modify its loss function for
optimization  by using an "unrolling technique." I think I'll have to read this
reference to really get it:

```
Dougal Maclaurin, David Duvenaud, and Ryan P. Adams. Gradient-based
hyperparameter optimization through reversible learning, ICML 2015.
```

They also cite one of those "learning to learn" papers "Learning to learn by
gradient descent by gradient descent" as an example of how unrolling works.
Maybe learning to learn is another way we can view unrolled optimization?

## Technical (and Non-Technical) Details

After some background material, which is fortunately review for me at this
point, let's get to the technical details:

- Introduce a **surrogate objective function** for the Generator, mainly because

    - (a) the true objective is highly nonconvex, 
    - (b) updates are not constrained (as in trust regions, etc.), and 
    - (c) we never get the optimal discriminator each step (as we'd want, I
      think) because we have alternating Adam steps. 
    
    This seems like applying Adam k times to the Discriminator update. But that
    violates the alternating gradient update "rule" right? The experiments
    should hopefully prove me wrong. Note also that by adjusting k, we can
    interpolate between the default GAN update rule (k=0 or k=1 depending on
    what indexing we're using) and the true GAN update rule (k=infinity). Yes,
    this makes sense, reminds me of the interpolation in the Generalized
    Advantage Estimation paper.

- Yay! The authors caught me thinking ahead of time:

  > It is important to distinguish this from an approach suggested in
  > (Goodfellow et al., 2014), that several update steps of the discriminator
  > parameters should be run before each single update step for the generator.
  
  I like it when that happens! **TODO read this section again and do the math**.

- Lots of higher-level discussion after that on the structure of the gradient
  and how clever unrolling can help *tackle mode collapse*. This seems
  interesting. I wish I completely understood the notation, though. More
  intuition:

  > Giving more information to G by allowing it to "see into the future" may
  > thus help the two models be more balanced.

## Experiments

- Another contribution of their paper is to present alternative evaluation
  metrics. They correctly point out that with normal GANs, mode collapse might
  actually make images more *realistic* to humans and thus by the human
  evaluation metric we actually **overrate** this GAN. 

- I think:

  > We experimented with both a fixed minibatch and re-sampled minibatches for
  > each unrolling step, and found it did not significantly impact the result.
  > We use fixed minibatches for all experiments in this section.

  means that we shuffle the data to start, but then iterate in a fixed order,
  which is typical of what we do nowadays for large datasets (for speed
  purposes).

- They provide five datasets of increasing complexity: 

    - (a) mixture of Gaussians,
    - (b) MNIST with LSTMs even if the Discriminator is a CNN --- interesting!! ---,
    - (c) augmented MNIST to test mode collapse, 
    - (d) augmented MNIST to test manifold collapse, and
    - (e) CIFAR-10.

**Concluding thoughts**: this appears to be an interesting application of a
relatively insightful idea (though it was derived from an earlier paper) and
seems well suited for reducing the tendency of GANs to mode collapse.
