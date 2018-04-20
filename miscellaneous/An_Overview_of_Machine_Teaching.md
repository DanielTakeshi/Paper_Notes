# An Overview of Machine Teaching

Summarizes a NIPS 2017 workshop, views it as mostly optimizing the dataset given
a fixed machine learning algorithm.

Good examples at the beginning, great optimization-based formulation later, then
some higher-level discussions with some future work that I should read ...

Example of why we want machine teaching:

- Consider trying to convey a known target which is abstract. For example, how
  to categorize rocks, a teacher has to deliver the "decision boundary" somehow
  to a student, but unclear how he/she can do that. By example, by ... ?

- Or an adversary attacking a known target model.

Note some machine teaching techniques:

> More broadly, human teachers afford the opportunity to teach beyond labeled
> examples: they can teach by features, pairwise comparisons, rules, etc.
> provided that the learning algorithm is equipped to accept such teaching
> signals. This is an active research direction in machine teaching.

Labeled examples are the key one, but there's others. I suppose curriculum
learning counts as well, data ordering. Again there are lots of hybrids. I like
the idea of pairwise comparisons. That's what OpenAI did with their "DeepRL from
Human Preferences" paper.

What about assuming students have limited memory? Could be useful, maybe, but I
doubt it applies to robots as we can get RAM pretty cheaply compared with the
price of a robot.

Unfair coding tricks: interesting, similar to what was discussed in
"Interpretable and Pedagogical Examples."

Human/Robot interaction: develop smarter learning algorithms for robots that can
*anticipate* being taught by a human teacher. (Definitely fits in with my
interests!) This is in their section on a machine teaching formulation for
reinforcement learning agents. So, rather than an algorithm to learn from
trajectories, we have to tell the teacher the most informative trajectories.
