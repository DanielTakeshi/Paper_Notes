# Deep Visual Foresight for Planning Robot Motion

General idea: it is expensive to require human feedback and (in some cases) labeled data for training. They develop a method whereby a robot can predict the effects of its actions, and show experimentally that their robots can push and handle objects, including those not seen in the training data. Their method combines two models together, one from video prediction (using deep nets) and another related to the model itself (not sure what this is yet).

Also, what's interesting is that they argue for model-based RL, yet the Value Iteration Network paper that I reviewed earlier seems to argue that model-*free* is better. There must be some tradeoff I'm missing. They say in section II:

> Our work represents an early step toward using learning to avoid the detailed and complex modeling associated with the fully model-based approach.

I guess *some* model-based stuff is fine, but *fully* model-based means having a model for *everything*, including all the 99% of the physics which might be unnecessary for this specific task of pushing objects.


## Introductory Material

There are lots of models involved in robotic actions, even those as simple as moving a cup of coffee across a table (side note: why is coffee always used as the example here?!?). But these can break down with unseen objectes or with modeling error, which can add up (or multiply!) to create awful situations (OK, I suppose coffee spilling on a business suit is awful). They describe it nicely:

> This is the essence of the unstructured open world problem: when the robot has to deal with the variability of the real world, methods based on rigid hand-engineered processes tend to suffer at the hands of special cases, exceptions, and unmodeled effects.

So their solution? They develop an MPC algorithm with minimal human intervention so that, given raw sensory observations of a bunch of objects (i.e. the robot sees the pixels with its sensors, which we would view as an image) it must, during test time, try to adjust the objects to achieve desired goal configurations. This usually involves pushing objects.

By the way, to clarify, here's what I think the terminology means:

- **Hand-engineered robotic manipulation pipeline**: This is when humans develop a bunch of modular systems that combine together to form objectives. The example in the paper is the cup of coffee, where one module fits a 3-D object model to the point cloud (from the sensors), and another module represents the physics of the system. They're different things, but can be clumsy or risk-prone.
- **Learned predictive model of raw sensory observations**: This seems the same as "learned predictive image model". The "raw sensory observations" are just what the robot "sees" with its sensors (LOL, remember Thomas L. Friedman's *Thank You for Being Late* where he discusses this?), and for us it's easiest to view it as a bunch of pixels.
- **Model-predictive control** (MPC): I'll describe this later.
- **Deep predictive models of video**: This must be Figure 2. Given a frame, along with an action, it will predict the next frame. In other words, it's predicting the new set of pixels. (See notes later.)

Are the second and fourth items on this list the same? I'm still confused. I think so, and I think the \mathcal{M} in the paper refers to this (these?).

The MPC and deep predictive models of videos are used *together* to choose robot controls, as discusse dlater.


## The Algorithm

Note: there are two main components, the deep predictive model of videos and the MPC.

(1) **Deep predctive model of videos**: it is predicting *an approximate distribution* over the sequence of *future frames* given a projected sequence of actions, i.e. we can say p(future_frames | current_frame, actions_to_take). The model looks pretty complicated ... with lots of layers and "probabilistic flow maps" to model motion. It uses convolutional LSTMs to predict images. Thank goodness for TensorFlow!!

Remember, try not to get too bogged down into the model description. (It seems like I'd have to read prior work to understand it anyway.) It's just a model, there may be other ways to express it. Also, one thing I want to improve on in the future is to better understand what people mean when they display diagrams. In my SIMPAR 2016 paper, I use a bad design for my neural network.

However, I'm confused about how the training works. This is unsupervised, right? They say the model is trained via maximum likelihood. But do we need labeled data for that? Or does the data come from the frames, i.e. if we see frames x1, x2, x3, and we're at x1 and trying to predict the future (i.e. (x2, x3)) does the model predict the next two frames and then compare how closely it matches (x2, x3)? I forgot how it works.

Connections with HMMs? I remember that we can predict future states in HMMs.

(2) **MPC**: this is the part of the algorithm which tries to achieve the following goal:

> This suggests a natural approach to planning using maximum likelihood: determine the sequence of actions that maximizes the probability of the designated pixel moving to the goal position.

This was when they described it in simple one-pixel terms, but they can actually extend this to multiple pixels by simply summing up the log probabilities.

Huh, they use the cross-entropy method to find a good action sequence. They contintually resample the action sequence, and then execute it.

**The overall algorithm** (see Algorithm 1 in the paper) combines the two components above. It receives a user-specified input (see Section IV-A) and then chooses controls to obtain that goal. I get it. Fortunately, specifiying (and making progress to) the goal *does not* require explicit object models!


## Experiments

Dataset used to train the "deep predictive model of images":

- 50k pushing attempts using 7 DoF arms
- Hundreds of objects
- Bins in front of robots contain 10-20 objects
- The (target?) poses are chosen randomly (?). (I'm a bit confused about this.)

At least it's publicly available. It seems like there are some TensorFlow files with it. When am I going to be an expert in those?

Here's what they do during test time: define a task as moving pixel or group of pixels from current position to a desired position. They need to answer the questions: (1) will this work on novel objectes in testing and (2) can training entirely on raw pixels be enough to infer behavior of physical objects? Note their important disclaimer:

> Note that the intent of our experiments is not to demonstrate that our approach provides the highest accuracy or performance for precise nonprehensile manipulation, but rather to demonstrate the flexibility of a data-driven, learning-based approach and illustrate that, even with no prior knowledge about objects, physics, or contacts, a predictive model trained entirely on raw video data can still infer characteristics of the physical world that are useful for robotic manipulation.

The quantitative results seem to be entirely in Table I, showing that their method is better than the baselines. Then they show special cases with images.


## My Thoughts and Takeaways

The paper seems impressive. There are a lot of model details that I really don't have time to read, unfortunately, but I think I could reproduce this if I had the Tensorflow files (the data is online).

I don't really get the full steps. I need to reread and think it over, as I'd like a video to play in my head on how the whole thing works.

Actually, just watch the video. Thank goodness there's captions (sorry I was in a grumpy mood after the CS 61C lectures at Berkeley somehow decided to disable the captions ...).

I suppose one weakness of the paper might be that the task seems relatively simple, at least judging from the video. They did say that they're not aiming for "highest accuracy or performance" so that's a good defense. Their baselines in Section V-B also do not use object identity and other physics information.
