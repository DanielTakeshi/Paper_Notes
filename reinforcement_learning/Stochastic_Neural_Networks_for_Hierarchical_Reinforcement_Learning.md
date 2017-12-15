# Stochastic Neural Networks for Hierarchical Reinforcement Learning

General idea: most deep reinforcement learning still uses naive exploration
strategies, such as uniform sampling or (especially) epsilon-greedy. But in
tasks with *sparse rewards*, which is the focus of this paper, this will not
work that well. There are two natural choices: design a hierarchy over actions
or use intrinsic rewards to guide exploration (I don't completely get these).
They seem to be proposing a solution that takes the best of both worlds by using
*stochastic neural networks*, which have stochasticity in the computational
graph (more on that later).

This paper *again* fits in the overall theme lately of "reinforcement learning
at a higher level" that I've been seeing a lot lately and have reviewed here on
this github repository, e.g. with the RL^2 paper, also the option learning
paper, actually even the end-to-end stuff (video prediction paper, etc.) is the
same because we abstract away specific models and just *learn* them. Also,
there's information theory here like in lots of other papers. I should keep this
in mind; all these papers are connected in some way.


## Introductory Material

They emphasize the following, which are all the rage nowadays:

- They use Hierarchical Reinforcement Learning (HRL) to reduce the search
  complexity of the problem
- They can represent multimodal policies. I guess these are policies that can
  invoke different sub-policies?
- Their method requires minimal domain knowledge.
- Their method works on domains with sparse rewards and (I think) uses lots of
  transfer learning, so it can generalize to other domains.

Notation: they use standard discrete-time MDP with rewards using r(s_t,a_t). The
\tau represents a full trajectory.

**Problem statement**: we have a collection of MDPs, \mathcal{M} (like some of
the other papers!) and wish to solve a collection of tasks *with minimal sample
complexity*. Assume that the MDPs share a subset of important states (the paper
uses \mathcal{S}_{\rm agent}), with the other states not mattering that much.
Yeah, I guess we can have several MDPs, all representing some robotic grasping
task, and the "irrelevant" states may include specific descriptions of objects
(we don't always need these to grasp). They also assume the same action space. I
was expecting this because having the same actions means our networks (if
they're outputting action or Q(s,a) values) can be consistent with the same
design across domains.

On a related note, their domain also assumes that they can construct a
**pre-training task**. Their description doesn't say much; it just says that
they "let the agent freely interact with the environment in a minimal setup" and
they have to *build* that setup depending on what tasks they want to perform. (I
should think about if these assumptions make sense or if they're overly
burdensome; so far they seem acceptable to me, but could easily be otherwise.)

OK, *given* the pre-training environment, their problem assumption is to learn
many "skills". They could do this separately by skill (i.e. one policy per
skill) but it's tricky to do that. Two problems: (1) sample complexity and (2)
policies may learn same skills (which is undesirable in this context).


## The Algorithm

1. **Sample Complexity**: Use Stochastic Neural Networks (SNNs), which they show
to have lower sample complexity than training simpler policies separately.
Restricted Boltzmann Machines and Deep Belief Networks are a subset of these. My
understanding of this part of the paper is likely to be minimal as I am not
familiar with those concepts. Somehow they use latent variables which encourages
different policies. This reminds me of **Latent Dirichlet Allocation** since it
is "forced" to create different topics. Maybe it's a similar concept?  
2. **Learning Same Skills**: This is where their information-theoretic
regularizer appears. I will have to look at the prior work to understand this as
well. They designed the theory so that it can be implemented/approximated by
maintaining state visitation counts.

Now wait a second, for (1) they say SNNs, but Figure 1 shows fully-connected
networks. Sorry, what's the relationship between the two?

For the *high-level policy*, they train a *separate* neural network. I assume
that each iteration, if there are K skills, the high-level policy has to pick
one of K actions, similar to the meta-policy paper from Ken Goldberg's lab?

As expected, they use TRPO for pre-training and the high-level policy training.


## Experiments

They rely mainly on a "three-link swimmer" (?!?) and the task is to navigate
through mazes and get sparse +1 rewards. The pre-training task is ... (?) Some
flat simple stage? I don't know. It looks like it's described further in the
paper "Benchmarking DeepRL for Continuous Control". OK, the videos seem to
clarify.

There are three subsections of the experiments, designed to answer different
questions. It looks like for the swimming plot, they're using six different
skills, with each one corresponding to a swimming direction. OK, fair enough,
this wasn't quite what I had in mind when I thought of "skills", though.
**UPDATE**: talked to Carlos. He clarified a bit, the swimmer should be facing
the same direction. Got it.

Figure 5 shows evidence that SNNs can cause the swimmer to go in more directions
than the hierarchy multi-policy, which is their strong baseline with
independently trained skills (recall that these are bad due to high sample
complexity, and SNNs overcome that).

Text size is REALLY small in Figure 6. And the plots are borderline unreadable
for me. :(

From Figure 6, it looks like of the four scenarios, only the maze 0 shows that
mutual information (the information theoretic regularizer?) actually helps out
their method.


## My Thoughts and Takeaways

For the experiment, I'm wondering if the policies learned are really that
distinct, in Figure 4(c) and 4(d) the color trajectories look very similar.

The paper only reports on the swimming scenario, but I know other papers which
have focused on only one experimental domain. Fortunately, apart from knowing
that there are six actions, it seems like there is not much domain knowledge.

Interesting connection with option learning:

> This issues could be alleviated by introducing a recurrent architecture at the
> manager level or allowing the manager to terminate the commitment to the
> current skill, similar to the option framework.

The biggest takeaway for me is that I need to review SNNs such as RBMs and DBNs
so that they're as familiar to me as FC nets, CNNs, and RNNs. There should be
enough online resources out there to help me out. That would help me understand
the paper better. I'm still confused about the information-theoretic
regularizer.
