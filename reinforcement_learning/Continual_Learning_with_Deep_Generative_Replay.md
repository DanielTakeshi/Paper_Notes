# Continual Learning with Deep Generative Replay

What if we want to do sequential learning of various tasks (e.g.,
classification), we can do this with deep neural networks:

(train task 1) -> (train task 2) -> ... -> (train task k)

But when you do this, performance on task 1 will deteriorate rapidly as you go
on to training tasks 2 and beyond. Then performance on task 2 degrades, etc.
This is the *catastrophic forgetting* problem in neural networks. It is normally
alleviated with a "replay buffer" where we store examples from earlier tasks and
replay them (so it's similar to experience replay in DQN except this is done for
different tasks). But this is costly. The authors propose a more efficient way
for doing so that more matches human biology (OK, hopefully...).
 
They argue to not use experience replay buffer but a *generative model* that can
generate high-dimensional images matching samples, because the human version of
memory can generate "false memory" of execution traces.

> In our deep generative replay framework, the model retains previously acquired
> knowledge by the concurrent replay of generated pseudo-data. In particular, we
> train a deep generative model in the generative adversarial networks (GANs)
> framework to mimic past data. Generated data are then paired with
> corresponding response from the past task solver to represent old tasks.

and 

> Generative replay has several advantages over other approaches because the
> network is jointly optimized using an ensemble of generated past data and real
> current data. The performance is therefore equivalent to joint training on
> accumulated real data as long as the generator recovers the input
> distribution.

(This is immediately raising red-flags for me ... not sure how well this will
generalize to more complicated tasks, especially in the RL setting.)

At each time step, the generator needs to be trained, then the solver trained
after that. (Two independent training procedures.) These together are "scholars"
and these do both teaching and learning.

The vast majority of experiments are based on MNIST and variants ... not the
most promising stuff (with GANs I figure you can use more complex image
datasets). Obviously performance of this method also depends on the quality of
the generative model.
