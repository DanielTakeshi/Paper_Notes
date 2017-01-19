# Recurrent Models of Visual Attention

The motivation for this work is that applying CNNs on large images is
computationally intractable because a vanilla CNN must "analyze" all of the
pixels. They instead propose a special kind of network that can focus
specifically on several areas of the image "at a higher resolution" (by this I
assume just apply normal convolutional computations on this small region). Their
model is *non-differentiable* and is trained using reinforcement learning, so it
is **hard attention**, not soft attention. 

(It turns out they use policy gradients, but interestingly enough, they can
split the training so that the "neural network portion" is trained with
backpropagation.)

Guiding principle:

> One important property of human perception is that one does not tend to
> process a whole scene in its entirety at once. Instead humans focus attention
> selectively on parts of the visual space [...].

Well said! This will provide lots of savings if we can focus selectively on
important parts of the input image. Now, how do we do that? Read on ...


## Their Algorithm 

The purpose of their algorithm **Recurrent Attention Model (RAM)** (see Figure
1) is, given a sequence of images so far (which might be video frames, for
instance, or just randomly selected MNIST images), their algorithm gradually
builds up an internal state which models the environment.  The next image seen
will have "attention" applied to it based on this internal model, as well as
whatever task is at hand (I assume this must be encoded somehow?). This can be
thought of as a RL problem where the agent interacts with the environment but
with sensors that can only look at small parts of the environment (i.e., the
image).

Intuition: they formulate it as a *computer game*. Interesting! The components
are:

- The **sensors** take in observations (not states!), which are the images x_t.
  The sensors don't view x_t as it is; in a *tiny region*, they view them with
  "high concentration", but have "low concentration" elsewhere. The latter is
  represented by a **glimpse network** f_g.
- The **internal state** of the agent summarizes the information from
  observations {x_1,...,x_{t-1}}. Updated over time by the **core network**
  f_h and its hidden layer.
- The **actions** are to focus somewhere on the next observation and to also
  impact the environment somehow. There's an **action network** f_a and a
  **location network** f_l. Wow, four different networks!!
- After executing actions, agent gets **rewards**, along with the next
  observation x_{t+1}. The objective is to maximize rewards. I see, they
  fortunately give an example: in object classification, the agent gets +1 if
  the image is classified correctly after T steps (I guess each step means we
  can focus on a different part of the image maybe?) and +0 otherwise.

The key now is how to convert the above formalism/setting into a useful problem.
For instance, how can we frame image classification as the computer game
described above? They seem to suggest we can do this by finding a policy \pi
which, when given \theta and s_{1:t}, must give us (l_t, a_t) as our action
tuple. Also for image classification, the final action the agent gives must
output the label of the image. So actions can equal labels in that case.

The policy in their case is defined by the RNN in Figure 1. That figure pretty
much explains it all. I think I'm getting it.

**Parameters**: these need to be trained. They consist of the glimpse, core, and
action network parameters. Since these together can be formulated into an
objective that consists of maximizing the reward, we can perform reinforcement
learning! Specifically, we'll do policy gradients, which involve unrolling out
the trajectories, then using them to adjust \theta so as to increase the log
probability of the sequence. They even use a baseline!

**Remark**: why are the location network parameters not part of the "parameters"
above? I believe this is because these parameters don't have a gradient, but the
others do. Hence use REINFORCE for this one, gradient descent (well,
backpropagation) for the others.

What's not used is TRPO, because it wasn't invented yet.  ;) Though I'm sure if
this were a 2016 paper, they'd be using TRPO.


## Their Experiments

Two major experiments:

- **Image Classification**: MNIST, of course, with centered and non-centered
  digits. For the latter, they had to translate the MNIST digits to create the
  data. Their retina was set at 8x8, which is only large enough to see part of
  the 28x28 digit. Finally, they also test using cluttered, non-centered
  datasets.

- **A Game of "Catch"**: this is a dynamic environment, showcasing the attention
  model's impressive versatility.

TODO I need to go through the descriptions again to understand the exact
progression. However, I think for MNIST, they keep looking at (i.e. "attending
to") different locations of the image until it feels confident that it knows the
number. Is that right?

In addition to describing their experiments, Section 4 also shows the design
choices for the RNN as a whole, which may help answer some of the questions I
have. The key thing, in my opinion, is *how they design the retina*. They do
this by using several patches centered at the location l from the location
network; the patches increase in size.


## My Thoughts and Takeaways

This paper was important for me to read since it accomplishes two learning goals
with one stone: understanding attention models and understanding recurrent
neural networks.

It took a few reads of Figure 1, but I *think* I'm getting it. The model design
seems very creative to me. I'm amazed that we can perform backpropagation on
something as exotic as this. The computational graph must be insane when
unrolled. But, calculus works! I still want to *implement* an attention model
myself, when I have the time. 

To practice understanding, it might be a good idea to frame various machine
learning problems into their attention model formulation. Let's get back to that
image classification problem. I know the actions are supposed to be the image
labels (well, some of them). But we only wanted those at the *end* of the RNN
right? Or at time T? What happens during times 1,2,...,T-1? What are the actions
in those cases? I'm not sure but I *think* they still output actions each
iteration ... so at time t, the action a_t is like the current prediction.  It
may or may not get changed, but should *improve*, to whatever is outputted at
a_T.
