# Model-Ensemble Trust-Region Policy Optimization

The paper discusses trends in model based/free:

- model free: sample inefficient but works well with Deep Networks
- model based: sample efficient but doesn't work well with Deep Networks

They say:

> Although it is a straightforward idea to extend model-based algorithms to deep
> neural network models, so far there has been comparatively fewer successful
> applications.

and note the *model bias* problem with model-based RL:

> While this issue can be regarded as a form of overfitting, we emphasize that
> standard countermeasures from the supervised learning literature, such as
> regularization or cross validation, are not sufficient here – supervised
> learning can guarantee generalization to states from the same distribution as
> the data, but the policy optimization stage steers the optimization exactly
> towards areas where data is scarce and the model is inaccurate. This problem
> is severely aggravated when expressive models such as deep neural networks are
> employed.

Interesting ...

They propose a *model-based* method where deep nets are used for both learning
the policy *and* the model and uses a model ensemble to regularize the process.
By doing so, they are able to run on MuJoCo tasks and be about 100x more sample
efficient!!!

PS: why are they saying that their method is "purely model-based" when they use
the model-free TRPO as a component of their algorithm? I'm confused.
