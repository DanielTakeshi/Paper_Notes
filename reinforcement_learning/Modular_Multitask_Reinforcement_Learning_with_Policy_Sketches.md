# Modular Multitask Reinforcement Learning with Policy Sketches

This is an exciting hierarchical reinforcement learning paper. The key idea is
to use **policy sketches**, which can be thought of as (minimally supervised)
high-level actions, but made up of **more specific tasks**, each represented by
its own neural network policy function. Those tasks can be **re-used**
throughout different sketches, as in the simple but helpful Figure 1 example (in
code, this means they have the same parameters).  Indeed, this reminds me of
Neural Module Networks. No wonder Jacob Andreas made the connection!!


## Training

The general framework can be trained with curriculum learning and an
actor-critic algorithm. It's surprisingly simple; see Algorithm 1 and Algorithm
2. The curriculum learning scheme lets the model start with easy tasks and then
scale up to harder ones. Otherwise if it started with the harder ones, it would
never see a reward, and if it stuck with the easier tasks, it would miss out on
higher-reward stuff. Here, the "difficulty" seems to be the sketch *length*. (I
might be able to use this idea for a project that I've shelved for now!)

(More on actor-critic and curriculum learning ...)


## Experiments

It seems to be robust to architecture choice:

> In all our experiments, we implement each subpolicy as a feedforward neural
> network with ReLU nonlinearities and a hidden layer with 128 hidden units, and
> each critic as a linear function of the current state.

Here are the three sets of experiments:

- **Minecraft**.

- **2D Maze Navigation**.

- **3D Locomotion**.

There are also several ablation studies. Yeah, I need to do those for my future
papers.


## Concluding Thoughts

**My Verdict**: I think it's a great paper! I should try running the code.
