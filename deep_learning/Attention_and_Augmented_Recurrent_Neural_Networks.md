# Attention and Augmented Recurrent Neural Networks

This is actually on Distill. [Click here to read.][1] Depending on how useful
and active Distill turns out to be, I will read it often. This "web-article" is
about recurrent neural networks (and when they say that, that includes LSTMs I
think) and four variants. Here are the four, along with my takeaways:

- **Neural Turing Machines**. These perform *read* and *write* operations. These
  are differentiable why? Because we can read and write *everywhere* according
  to some weighted distribution, kind of like how a Gaussian density assigns
  mass to the entire real line (and in practice, we only care about the parts
  which have non-negligible mass).

- **Attentional Interfaces**. These can be put as interfaces between RNN-RNN
  sections, or CNN-RNN sections, to attend to specific regions in the respective
  input networks. Thus, tasks such as language translation and image captioning
  become easier. Attention is again differentiable assuming we have a weighted
  distribution over the input.

- **Adaptive Computation Time**. These let RNNs perform different amounts of
  computational steps per "time step" (i.e. per input to the RNN). Intuition:
  some sections of the input are more important than the others, hence we should
  spend more resources at those times. How do we choose the number of steps?
  Weigh all the steps!

- **Neural Programmers**. These write *programs* to solve expressions (such as
  algebraic expressions). To do so, they have to select which operations come
  together to form the expression, and the task of "picking the next operation"
  is once again differentiable if you weight all operations!

Other stuff: I think I get the high-level ideas of these. Not the details, of
course, but that's impossible to do with just a blog-style post. I think if
there's anything I should focus on, it's #2 here, the "Attentional Interfaces"
section. The others don't seem immediately relevant to me. Another takeaway is
that if we have some discrete quantity, just weigh them all and that's
differentiable!

[1]:http://distill.pub/2016/augmented-rnns/
