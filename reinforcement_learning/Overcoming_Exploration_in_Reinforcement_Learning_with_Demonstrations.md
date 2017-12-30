# Overcoming Exploration in Reinforcement Learning with Demonstrations

**What is the paper contribution/novelty?**

- Present a method to solve RL environments with sparse rewards by using a small
  set of demonstrations to guide exploration. The focus of tasks is those with
  only a 0/1 binary reward indicating task completion, which is common in
  robotics environments, which they say:

  > The difficulty posed by a sparse reward is exacerbated by the complicated
  > environment dynamics in robotics. For example, system dynamics around
  > contacts are difficult to model and induce a sensitivity in the system to
  > small errors. Many robotics tasks also require executing multiple steps
  > successfully over a long horizon, involve high dimensional control, and
  > require generalization to varying task instances. These conditions further
  > result in a situation where the agent so rarely sees a reward initially that
  > it is not able to learn at all.

  and:

  > The primary contribution of this paper is to show that demonstrations can be
  > used with reinforcement learning to solve complex tasks where exploration is
  > difficult.

- They show that it works using the simulated block demonstrations, which was
  the same experimental setup in Hindsight Experience Replay. Oh, and it
  outperforms both RL/IL algorithms, AND common-sense baselines such as
  Imitation Learning starting from those demos, and fine-tuning RL. Interesting!


**Why does their method work?**

- Their method works since it uses LfDs with RL in a similar way as two prior
  IL+RL methods that work, DQfD and DDPGfD. 

- It also works because it's possible to sample goals to better generalize, as
  in HER. (They integrate HER in their method, btw, and normal HER is also the
  baseline.)

- They use several ideas, one of which is the Q-filter, which helps to mitigate
  the influence of potentially suboptimal demonstrator trajectories. Another is
  resetting demonstrations. These make sense for improvements to the HER+DDPG
  baseline.


**Could I implement this?**

- I *think* so. Well, I'd need the code, obviously. It's too bad that this robot
  simulator is stuck with OpenAI's internal code.
