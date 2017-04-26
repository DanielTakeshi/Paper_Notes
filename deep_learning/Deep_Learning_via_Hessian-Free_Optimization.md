# Deep Learning via Hessian-Free Optimization

The title seems to imply that the paper is about *second-order* optimization,
rather than first-order (like gradient descent, momentum gradient descent, and
also ADAGrad, ADAM, RMSProp right?) optimization for updating weights. I think
this is *different* from backpropagation, which is still used to *get*
gradients, except that the gradients are "used" in a different way in a
second-order update because we need a Hessian. However, the Hessian is too
computationally intractable so some approximation must happen.

(When reading the paper, remember that it's from 2010, before AlexNet, which is
why the author said in the introduction that backpropagation "does not seem to
generalize to deep networks".)


## Main Ideas

- Use **Hessian-Free** optimization, AKA the **Truncated Newton** method, for
  training networks. The algorithm is *not* new, but it hasn't been used in
  deep neural networks.

- Why? Normal gradient descent (+ backpropgation to compute gradients) performs
  poorly with bad curvature settings. Yes, I know this and it's common
  knowledge. It also makes sense that exploding/vanishing gradients are a
  problem with deep learning.

- Modify off-the-shelf HF algorithms for neural networks, such as using a
  "backpropgation-like" algorithm to compute something like a gradient (?),
  sorry I'm not too familiar with this. 
  
- In addition, the paper proposes ways on how to use this in a minibatch
  fashion. This must be done with care, since otherwise Conjugate Gradient will
  not work well.

- Quite interestingly, they have a full section (albeit small) on weight
  initialization! This turns out to be a very active area of research. Ignore
  this section since more recent work will have better initialization
  procedures.


## Thoughts and Takeaways

I remember reading this paper when I was working on a project for neural network
optimization, but I didn't quite get it the first time. Now I'm reading this two
years later (in Spring 2017) so the material is clearer to me.

PS: I just had to laugh at this line:

> Learning the parameters of neural networks is perhaps one of the most well
> studied problems within the field of machine learning.

Wow, then it must be *the* most well-studied problem, seven years later!

Last note ... what I'm still not sure about is what I talked about earlier. In
other words, does this HF algorithm work *with* backpropagation, like SGD?
(Backprop to compute the gradients, SGD to update the weights.) It *seems* like
HF optimization is similar. The paper says:

> Pearlmutter (1994) showed that there is an efficient procedure for computing
> the product Hd exactly for neural networks and several other models such as
> RNNs and Boltzmann machines. This algorithm is like backprop as it involves a
> forward and backward pass, is "local", and has a similar computational cost.
> Moreover, for standard neural nets it can also be performed without the need
> to evaluate non-linear functions.

I *think*, therefore, that the above means backpropagation is computing
gradients, and then the HF optimization is further computing the second-order
information from this in order to determine the actual weight update.

The big question for me is whether HF optimization is practical for *modern*
(2017-style) Deep Learning. Do people use this at all? Or do people borrow
ideas? I know that TRPO uses some similar ideas, and I'm trying to flesh that
out.
