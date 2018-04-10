# One-Shot Visual Imitation Learning via Meta-Learning

Update: see my blog post.

**Main idea of the paper**: show that MAML can be extended to real robotic tasks
so that robots can learn a task given only one demonstration (hence, the
"one-shot" part) and where the demonstration is done using videos, so simple
pixels (hence the "visual imitation" part). The motivation behind this is to
build general-purpose robots that can quickly adapt to new tasks, just like
humans can!

> By reusing data across skills, robots should be able to amortize their
> experience and significantly improve data efficiency, requiring minimal
> supervision for each new skill. In this paper, we consider the question: how
> can we leverage information from previous skills to quickly learn new
> behaviors?

**How and why does this work?**

> In essence, we learn policy parameters that, when finetuned on just one
> demonstration of a new task, can immediately learn to perform that task.

- It works because MAML worked ... and this algorithm is based upon that, using
  the idea of making their parameters "easy to fine-tune."

- Previous MAML: image recognition and reinforcement learning. Now: MAML
  extended to imitation learning (and from raw pixels to boot!).

- Assume a dataset of demonstrations. For each *meta-training* task, we have two
  demonstrations.  (For the *meta-testing* tasks, we only need one
  demonstration.) In training, we use one demo for updating a task-specific
  parameter, and then use the second demo for the meta-update.

- Two head architecture: the purpose of this is to avoid the need to explicitly
  enumerate an expert's actions, since it is more convenient to provide a video
  of a demonstration. (Only applies during *test-time*.) I'm going to have to
  look at this again since it's not totally clear ...

- Also note their *bias transformation* layer, which they argue is important for
  meta-learning.

**Could I implement this?** I'm pretty sure I could implement MAML itself. Their
newer architecture is slightly more complicated, unfortunately ... but
definitely easier than the "One-Shot Imitation Learning" alternative.
