# Manipulating Highly Deformable Materials Using a Visual Feedback Dictionary

**TODO: notes in progress**

Manipulation of cloth is an important problem in robotics, yet it is challenging
due to complex material properties. I agree. Their approach here involves a
dictionary.

They use **histogram of oriented wrinkle** features. If you plot these, it looks
like a bunch of gradients (clearly inspired by computer vision pre-Deep
Learning). This paper does not use Deep Learning. The process is:

(TODO: flesh out carefully)
1. Set an `(RGB image) ==> (histogram of features)` pipeline.
2. Then, `(manipulation data) ==> 

The experiments involve a YuMi, like what we have. They benchmark three tasks:

- Human-robot jointly folding cloth with one hand each, so two total
- Robot folding cloth with two (i.e., both) hands, no human involved
- Human-robot stretching a cloth with both hands, so four total

Sure ... looks interesting.

The paper cites some of the same papers as in my arXiv preprint, and a few
others that look interesting.
