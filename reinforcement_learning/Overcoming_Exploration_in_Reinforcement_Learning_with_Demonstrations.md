# Overcoming Exploration in Reinforcement Learning with Demonstrations

See: https://danieltakeshi.github.io/2018/02/28/sample-efficient-rl/

**What is the paper contribution/novelty?**

- Present a method to solve RL environments with sparse rewards by using a small
  set of demonstrations to guide exploration (reminds me of DDO). Use tasks with
  a binary reward indicating success, which is common in robotics environments.

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

- They show it works using simulated block demos (same as in *Hindsight
  Experience Replay*), and that it outperforms both RL/IL algorithms AND
  common-sense baselines such as Imitation Learning starting from those demos,
  and fine-tuning RL. They use 100 human demos, collected via virtual reality.
  The key is, beyond initialization, to use those precious demos efficiently.


**Why does their method work?**

- It uses LfDs with RL in a similar way as two prior IL+RL methods that work,
  DQfD and DDPGfD.

- Uses two replay buffers, one for demos, one for agent exploration, though that
  was done in prior work and can easily be done with just one replay buffer with
  different weights attached to the data points.

- It's possible to sample goals to better generalize, as in HER. (They integrate
  HER in their method, and normal HER is a baseline.)

- Three new ideas:

  - **BC loss for actor**: it's the same as the IL loss, but it's a "new idea"
    here because they can add this loss to the RL agent's cost function (the
    actor only). Normally the actor is trained via policy gradient, so here the
    gradient is PG *plus* the IL loss. We are jointly optimizing the Bellman
    equation for Q-values *and* the expert data. 
 
    There's no way this is new, though, right?

    At least I understand this: if we jointly optimize those two, then the actor
    is not going to improve much beyond the expert demos. Why? Because doing so
    would increase the IL loss portion of the cost function. This brings us to
    the next novelty ...

  - **Q filter**: apply the IL/BC loss *only* to states where the *critic*
    determines that the demonstrator action is better than the actor action.

  - **Resetting trajectories to demo states**: we're already doing DDPG in an
    episodic framework, so we start fresh each episode which is like a reset.
    This technique means, for some trajectories, instead of executing the
    agent's current exploration policy from the starting state distribution, we
    start the episode from a random expert demo state. 

    In any case, the contribution of this part seems questionable until we get
    to stacking 6 blocks. Figure 4 shows via an ablation study that the method
    "Ours" converges to better performance than the method "Ours, Resets",
    though Figure 5 shows the opposite with more blocks.

    I see, they argue that it's important to have rollouts start at random
    expert demo states since those include instances when there are 4/6 and 5/6
    blocks stacked, and so the agent can experience those states rather than
    always starting from 0 blocks stacked. (See discussion among the ablation
    experiment.)


**Could I implement this?**

- I *think* so ... though it's much more complicated than Reverse Curriculum
  Learning. Well, I'd need the code, obviously. It's too bad that this robot
  simulator is stuck with OpenAI's internal code.
