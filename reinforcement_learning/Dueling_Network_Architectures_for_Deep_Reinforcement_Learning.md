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
the V and A values. See Section 3 for the details on this. They say the ideal
way is to force zero advantage ... but I think they're assuming we are always
choosing the max value (so no epsilon greedy). Then they say to take an
*average* instead of the max. This part kind of makes sense but it's still
confusing.

**Thought Question: why is it called "dueling"**? Not sure ... the point here
seems to be that if actions don't matter, then the value function will be
"general enough" and shared among different actions. That seems misleading,
though, isn't it just better to say that the advantages will be zero? We still
have to compute a dense value function, and it's just that the *advantages* are
trained to be zero (i.e. be a sparse function) most of the time. See the bottom
of Section 4.1 for this discussion, which seems to hit at the main architectural
benefit.

They also emphasize in the conclusion that the *value function is updated more
frequently*. I can kind of see why, with `V(s) = max_a Q(s,a)`, if we update
some `Q(s,a')` value based on some action `a'`, that may often not change `V(s)`
at all, and this grows increasingly likely as the number of actions increases.

Then they had experiments, I liked the discussion here, but for the most part
it's pretty easy reading so no need to take notes on it.

**Evaluation update**: how do they do evaluation? It's a relative metric that
compares against human and baseline agent scores, and converts that to a
percentage value.  This seems to be consistent with DeepMind's other evaluations
since they are less about raw scores and more about comparing against humans.
They post raw scores in the appendix, *but where are these from*?? Are they an
average over the last 100 episodes encountered during training, or some other
metric?

Wait: upon another read they cite the Gorila paper which discusses evaluation.
See that.
