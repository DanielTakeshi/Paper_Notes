# Learning Modular Neural Network Policies for Multi-Task and Multi-Robot Transfer

Interesting paper about some transfer among TWO dimensions: tasks and robots
(all simulated via MuJoCo, btw).

Basic idea is nicely summarized in the figure when they show their modules. They
have a 2x2 setup with two robots and two tasks. They can train on three out of
the four combinations, and then they get a task-specific module and a
robot-specific module, and show that applying the appropriate combination to the
new task results in good zero-shot generalization, or is good for fine-tuning.

It's slightly different from the robot-to-robot teaching idea since we're not
really using a teacher but training robot-specific modules, so each robot gets
its own neural net that's trained beforehand (because even on the held-out task,
the 2x2, 3x3, 3x4, etc., nature of their experiments guarantees that the module
will be needed sometime.
