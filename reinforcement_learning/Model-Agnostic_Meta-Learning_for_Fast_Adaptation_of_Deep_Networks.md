# Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks

I'm interested in this paper and the MAML algorithm because I want to do
meta-learning:

> The goal of meta-learning is to train a model on a variety of learning tasks,
> such that it can solve new learning tasks using only a small number of
> training samples.

**TL;DR MAML trains models to be easy to fine-tune.** Nice!

The paper is well written and Section 2 has a nice overview of the problem. I
understand the overview of meta-learning. (I am most interested in the iid
supervised learning case.) The key seems to be this:

> The model f is then improved by considering how the *test* error on new data
> from q_i changes with respect to the parameters. 

I wasn't totally clear on this part. Note that f is the neural network and q_i
is the distribution over data points, or initial states if we're dealing with
sequential data such as reinforcement learning rollouts.

Note that this is *not* how we officially evaluate the meta-model. That happens
*after* this part. So there is one test stage which is "embedded" in the
training procedure. The key to their procedure is to find a set of weights which
is generic to all tasks from p(T) but also sensitive enough so that it can
rapidly adjust as needed to new tasks via K samples *and* while using gradient
descent. PS: one of the reasons why they claim their algorithm is generic is
because all it assumes is that stuff is trained with gradient descent. Makes
sense. There's also less machinery involved with RNNs as compared to the RL^2
paper ("preprint").

The above is high level and makes sense (make weights sensitive enough without
overfitting, etc.) but what is the real meat here? It's that the optimization
problem looks like this:

```
min_theta sum_{T_i} loss_{T_i}(f_{theta_i'})
```

where T_i is task i out of the distribution over tasks. The key is that we are
minimizing theta for the meta-model, **except that the actual values in the
objective are computed with the `theta_i'` values, the task-specific stuff!**.
I like the idea, it sounds intuitive and seems to be effective.

Remember, *after* we get the `theta`, then we're not done. For evaluation
purposes, we need to collect a new set of samples for the separate tasks, train
(fine-tune, really) under a limited budget, and then test. Got it.
