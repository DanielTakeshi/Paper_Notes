# Progressive Reinforcement Learning With Distillation For Multi-Skilled Motion Control

Proposes *Progressive Learning and Integration via Distillation (PLAID)*.

Rationale:

- It is of interest to effective merge different skills so that one agent can
  invoke them as needed. Or said another way, it is important to continually
  acquire new skills for lifelong learning.
- Sometimes, these new skills to be learned will not already have a ready
  teacher. In PD, for instance, they assumed for each task (i.e., Atari game)
  there existed a teacher for it. Also, it's important not to acquire skills at
  the cost of forgetting older ones, or exhibit deteriorating performance.
- It is also interesting to apply policy distillation in the continuous control
  setting in their bipedal stuff (note this is where Jason Peng was working
  before he came here).

Algorithm:

- Train a policy on a task. Then for the other tasks after that: 
    - Start with the current policy trained on the next task to learn, so it's
      like learning the task, but with pre-initialized weights based on the
      earlier tasks, hence Transfer Learning (TL). They use an existing policy
      network that is *initialized from the previous distilled policy*!
    - But now how do we maintain skill in older tasks? This is where
      distillation comes into play. I think it's very simple: use trajectories
      from all the tasks to compute the distilled policy that can perform well
      on all of them, with the key being to use both expert and student
      trajectories in this step, and then anneal so we start with mostly expert
      actions in the data, then anneal with student actions, in a form of
      DAGGER. So, the distillation is based on imitation learning since all
      policies output state-dependent joint means. **(TODO: better check all
      this...)**
    - With distilled policy, repeat for subsequent tasks (so, TL then PD).
  Hmmm ... this contribution seems somewhat limited and obvious, but perhaps it
  has more impact than I would think? A lot of the paper is unclear, which is
  hindering my ability to understand the algorithm.
- When they say "expert policy" I assume this is what happens after
  distillation, so we assume we have an expert on the previously trained tasks,
  and then the student is the new network that has to learn from this expert
  *and* the other network that was trained (via TL) on the newest task?
- As with most policy gradient methods for continuous control:
    > The policy network models a Gaussian distribution by outputting a state
    > dependent mean.
- Input injection: a control policy can be augmented with additional inputs
  (w/out adversely affecting performance).

Clarifications from OpenReview:

> Re:  during distillation, do we start from random policy or expert policy?

> The networks were initialized from the most recently trained policy, i.e., the one trained on the new task.

https://openreview.net/forum?id=B13njo1R-

So distillation was initialized from the *one trained on the newest task*.


Experiments:

- Benchmark methods/policies for merging skills: **MultiTasker**, **Parallel**,
  **TL-Only**, and (their algorithm) **PLAID**.
- Evaluate on a continuous motor control problem with a 2D humanoid walker:
  *robust locomotion over 5 distinct classes of terrain*, similar to the setup
  of Jason Peng's earlier SIGGRAPH paper. What's interesting here is the need to
  both traverse over terrains *and* match motion capture gait of a human. So
  this is "more realistic."
    - Tasks (i.e., terrain types) are presented sequentially.
    - Input is: (a) a character and (b) a terrain state representation.
- The limitation here is that they are demonstrating continual learning on
  *related* tasks. What if they are different? That means transfer learning is
  obviously harder.
