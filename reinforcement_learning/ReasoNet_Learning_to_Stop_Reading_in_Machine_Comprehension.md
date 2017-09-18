# ReasoNet: Learning to Stop Reading in Machine Comprehension

An interesting paper at the intersection of Deep Reinforcement Learning and
Natural Language Processing.


Main points:

- A new neural network architecture, **Reasoning Networks** (ReasoNets), trained
  with reinforcement learning (because the target is non-differentiable), to
  dynamically analyze a document to answer some questions and to stop reading it
  when it thinks there is no benefit remaining.

- See Figure 1 for the architecture. It consists of memory, attention, internal
  state, termination state, and answer. The answer is sampled after the
  termination state returns true. The termination gate variables are thus 0, 0,
  ..., 0, and then 1 at the end, whatever time-step that may be.

- The reward is sparse, and is only 1 at the very end if the answer is true. The
  reward is 0 otherwise, *and* also 0 during the intermediate times before the
  final one. The goal is to maximize the reward, and they train using the
  standard REINFORCE estimator. But their baseline is NOT the expected reward,
  since it is less effective for their problem domain; see Section 3.1 for their
  proposed alternative *instance-dependent* baseline formulation for reducing
  gradient variance.

- Interesting connection to RL: ReasoNets can stop at different time steps. This
  is the same for RL stuff like MuJoCo trajectories, which can end early if the
  agent dies early. Can similar concepts transfer? And why in RL do we use that
  baseline?

- Training data consists of <query, passage, answer>.


Experiments:

- Experiments on CNN (not Convolutional Neural Networks...), DailyMail, and
  SQuAD datasets. There is also a synthetic graph reachability task, designed to
  test longer-range tasks. This is a good balance, and synthetic datasets are
  good if they can clearly test some variable well.

- The maximum reasoning step is just five for CNN and DailyMail. I thought it
  would be much higher... for SQuAD it's 10, for Graph Reachability it's 15 and
  25.

- Table 1: why are there no error bars? I cannot tell how significant the
  results are. Same for Table 2, but maybe the people in NLP have intuition on
  the significance of these results.

- They plot the distribution of the termination steps to find out when reasoning
  stops (e.g., early, late?). In the Graph Reachability task, instances
  terminate at the last step frequently (with longer max times).

- I think the Graph Reachability dataset is designed to answer queries of: is
  there a path from node X to node Y? So it's not really NLP but an abstract
  version which can be easily manipulated via code. And it's easy to see how
  time plays a role: you have to explore some of the graph before you can make a
  decision.


Questions:

- What are "cloze-style" datasets?

- **Single-turn vs multi-turn reasoning**: how do we ensure that the comparison is
  fair? The latter seem to allow for multiple passes over the data, which should
  be (monotonically?) better than single-turn reasoning. Are there metrics such
  as amount of knowledge gained *per time* for better comparisons? Note: these
  are the two categories the authors propose for inference methods on
  cloze-style datasets.

- (TODO later) more detailed understanding of their instance-dependent baseline.
