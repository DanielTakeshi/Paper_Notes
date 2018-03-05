# Reinforcement and Imitation Learning for Diverse Visuomotor Skills

They present model free Deep RL method that can be used for robotics tasks by
mapping from RGB camera inputs to joint velocities. As I know, there are a
number of challenges to that, including sample efficiency, safety, exploration,
etc., and even if they did model-based stuff, there are challenges with those
(e.g., how to accurately model stuff).

Side note: I'm not a fan of their three key insights, because two are not novel.
The insight of using human demos to handle exploration has already been covered
(overcoming exploration, dexterous manipulation, DQN from demos, etc).  The use
of varying training conditions has also been covered (domain randomization,
dynamics randomization).

The good news is that the other insight of leveraging task-specific information
to accelerate training seems novel. The bad news is that it's task-specific.
However I think the contribution of the paper is to show that IL and RL can be
used for more complicated, longer-horizon tasks than prior methods? The
contribution also seems to be a preliminary zero-shot sim2real, but it's
"preliminary" so not sure how significant ... another contribution would be
their network architecture, I guess.

Their model (see Fig 2) relies on a GAIL-like objective in there, interesting,
along with PPO. So think of it as combining those two and extending them.

Demonstrations collected via teleoperation for tasks, and just 30 minutes per
tasks, and only demonstrator states (and not actions) are needed.

Future work? Improving sample efficiency and trying to reduce sim2real gap. So a
bit vague, unfortunately.
