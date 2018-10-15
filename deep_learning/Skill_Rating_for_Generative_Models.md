# Skill Rating for Generative Models

I like the idea of this paper. Use tournaments to evaluate generative models.
They propose: **tournament win rate** and **skill rate**.

You can use this to evaluate the performance of a single machine learning
model's training trajectory by running an internal tournament among saved
snapshot of the learned model. Interesting!

Recall the note/paper which emphasis how tricky it is to evaluate generative
models.

https://arxiv.org/abs/1511.01844

You cannot use tournament rating to get an absolute scale, only a *relative*
one. And ratings among different tournaments are not comparable.

The paper has a lot of future work which I was hoping to see in the actual
paper, but these experiments certainly can take a long time to run. Having a set
of agents for a tournament (rather than just one) can increase software
complexity, not to mention the need to track frozen snapshots of machine
learning models as needed.
