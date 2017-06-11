# Domain Randomization for Transferring Deep Neural Networks from Simulation to the Real World

Related blog posts:

- https://blog.openai.com/spam-detection-in-the-physical-world/
- https://blog.openai.com/robots-that-learn/ 

The core of this paper is trying to address how to transfer stuff trained in
simulation to the real world in the context of **object localization**, though
of course their technique *should* be able to transfer to other tasks. As I
would expect, this involves a lot of "data augmentation" done via simulation to
make the deep network more robust to noise, variation, and so forth (similar to
image classification).

> With enough variability in the simulator, the real world may appear to the
> model as just another variation. [...] 


**Specific contributions**:

- They solve the following problem: given desired objects (potentially cluttered
  with *distractor* objects) and a camera frame, train an object detector which
  can accurately return the Cartesian coordinates of those objects.

- See **Section III-A** for the specific stuff they randomized in simulation.
  This will be very application-specific. One point of concern is that they
  mention using MuJoCo, but I don't know how to use MuJoCo for this (and I only
  use it indirectly via OpenAI gym).

- See **Section III-B** for their neural network object detector, which is a
  variant of VGG-16. The loss function was L2 loss of the coordinates, which
  makes sense to me.


**Experimental results**:

- Note that these rely on mesh representations of objects in their simulator.
  Ugh. I'm not totally sure I understand clearly the localization experiment.

- There are a lot of "ablation studies" and by that I mean varying different
  aspects of the pipeline (random noise or not, pretraining or not, etc.) to see
  what parts are essential. This is a common practice in robotics papers, but I
  worry that these are sometimes a crutch just to get more non-significant
  results in a paper. Then again I might sometimes do the same thing, darn.

- OK, now the real deal is with their actual robotics experiments. In 38 out of
  40 trials, their robot was able to successfully pick up the target object.
  Yay!


**Thoughts, Takeaways, etc.**:

Note that their contribution, domain *randomization*, is not the same as domain
*adaptation*, though the ideas could be combined as hinted in the conclusions.
In fact, they deliberately *avoid* domain adaptation to show that their method
somehow still works.

I like the *idea* of this paper, though I think the paper itself could be a lot
better (better writing, better figures, better set of experiments, more
real-world instead of ablation, need another figure like Figure 6, etc.).

> To our knowledge, this is the first successful transfer of a deep neural
> network trained only on simulated RGB images (without pre-training on real
> images) to the real world for the purpose of robotic control.

Yes, I think the above makes sense after reading the paper, and this idea should
be pursued vigorously in other applications.
