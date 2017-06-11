# Automating Multi-Throw Multilateral Surgical Suturing with a Mechanical Needle Guide and Sequential Convex Optimization

This paper is about one of the problems of FLS, that of suturing. It's supposed
to be the "third easiest" from what I've heard. (Cutting, which is featured in a
lot of Ken Goldberg's papers, is slightly easier, the "second easiest" task,
thus suturing might be more suited for future papers.) Also, as suggested by the
paper title, it does **not** use any deep learning, but instead **sequential
convex optimization**.

I think the idea of this paper is that prior work was able to automate some of
the suturing process, but here they're attempting to automate *longer* sequences
and/or combine some of the steps together.

The main contributions are:

- A *mechanical device*, Suture Needle Angular Positioner (SNAP), which is
  designed to (a) **align and hold** needles in specific orientations. This was
  3-D printed, BTW, but hopefully I can just use this if needed without
  redesigning something from scratch. Judging from the video, it seems like this
  is designed so that the robot can approach a needle from slightly different
  angles, and its shape will naturally "guide" the needle to its center (again,
  see the video). In other words, it increases robustness to needle position,
  always a good thing.

- The other thing they develop to increase robustness to needle position is to
  use a needle tracking system using computer vision techniques and a Kalman
  filter.

- A Sequential Convex Programming (SCP) formulation of the needle suturing task.
  (This is a bit of a black box to me, currently and I need more intuition on
  the end-to-end pipeline.) The collision constraints and entry/exit points are
  easiest to understand. The kinematic constraints are a bit confusing, though.

- To understand the SCP, look at Figures 3 through 5, which I really like.
  Figure 3 shows that the **entry point** of the needle matters a lot. Figure 4
  shows that they are allowing some variation in needle angle, because tissue is
  not going to allow needles to pass through seamlessly. Figure 5 shows that
  angle of orientation matters.

- They *also* propose a **Finite-State Machine** for the entire process (I
  think?).  I'm not totally clear on the difference between this FSM and the SCP
  formulation, but I *think* the latter is contained in the former, so that the
  FSM designs the end-to-end pipeline, and *utilizes* the SCP to establish a
  planned trajectory.

The results are expressed as 86.3%, though I think (judging from their videos)
that this is for **one** set of needle insertions + needle re-grasping. In
reality, such a task requires **four** sets of those, and obviously more
"throws" (i.e.  needle insertions + needle re-grasping) may result in more
failures. Their video shows examples of failure modes.

(Their first set of experiments test robustness to needle uncertainty, which I
suppose is necessary.)

Thanks for having a video. These make it so much easier to understand these
papers.

Questions:

- What is SPP? They abbreviate Sequential Convex Programming as SCP, but what is
  **SPP** then?? Is that a typo (they use it multiple times)? If this is indeed
  SCP, then Figure 5 shows the output of the algorithm, though it might be a
  simplified overview. [EDIT: I see, SPP is the **Suture needle Path Planning**
  problem, which is solved using SCP so really, either abbreviation would
  probably work.]

- Can steps in this pipeline be replaced by Deep Learning?
