# Dueling Network Architectures for Deep Reinforcement Learning

The main idea of the paper is a new architecture for model-free RL, which they
correctly note is complementary to other approaches in RL which "simply" add
neural networks (or focus on the algorithm, not the architecture). Specifically,
they use two components in their overall architecture:

- **State value estimator**: a scalar quantity `V(s)`.

- **Action advantage estimator**: one scalar `A(s,a)` for each action, so it's a
  vector.

These two estimators are combined into one Q-Network which predicts `Q(s,a)`
values like in normal DQN, with s as the input and the output an |A|-dimensional
vector with the softmax-ed probabilities for each action a in A. The two
components of Dueling DQN actually share a lot of the same structure, most
notably the convolution (+related) layers, which is why they're really viewed as
one network (which just so happens to have two separate estimators embedded in
it).

See Figure 2 for some nice intuition on the difference between the value
function and the advantage function. The advantage tends to occur only when
different actions would result in drastically different consequences.

They say:

> To strengthen the claim that our dueling architecture is complementary to
> algorithmic innovations, we show that it improves performance for both the
> uniform and the prioritized replay baselines (for which we picked the easier
> to implement rank-based variant), with the resulting prioritized dueling
> variant holding the new state-of-the-art.

Question, is this or A3C the state of the art? Not sure. But probably this one,
dueling, *prioritized*, *double* DQN, but A3C might be faster. Yep.

Also:

> The key insight behind our new architecture, as illustrated in Figure 2, is
> that for many states, it is unnecessary to estimate the value of each action
> choice.

I agree.

The other key innovation here is not to simply sum `V(s)+A(s,a)` to get the
`Q(s,a)` final output layer (with V "broadcasting" as in Python code), but to
force zero advantage to gain identifiability, so given a Q-value, we can obtain
the V and A values.

**Thought Question: why is it called "dueling"**? ...
