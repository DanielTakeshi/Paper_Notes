# A Deep Hierarchical Approach to Lifelong Learning in Minecraft

They present an architecture, **Hierarchical Deep RL Network (H-DRLN)** for
lifelong learning, and apply it to Minecraft. A few points:

- They talk about "skills" and these are the same thing as "options." I mean,
  almost literally, right? Their H-DRLN architecture outputs both primitive
  actions (e.g., UP, DOWN, etc.) and skills (skill1, skill2, etc.). The skills
  are themselves policies that can execute and return control back to the master
  policy upon termination. This is the same thing that Sanjay and Roy were doing
  a few years ago, well, at around the same time these authors likely worked on
  this paper.

- To be clear: their H-DRLN is supposed to learn a policy that decides when to
  use primitive actions versus skills. Then the Q-values of those is used for
  determining what to actually do. Also, seems like the skills are *pre-learned*
  somehow, I assume this means they are able to execute their agent in the
  relevant subset of the state space corresponding to the skill.

- They pre-train a set of N *Deep Skill Networks* using vanilla DQN and then
  combine them using either an array (having independent DSNs) or distillation,
  where they use PD to put all these into one network (with the usual separate
  output layers for each skill). They argue this is a novel variant of PD, but
  how can it be if they are using the same exact algorithm? It looks like the
  only difference is that they use PD for skills, but skills themselves are
  policies, they simply are the lower-level ones that are invoked by the
  higher-level master policy?? They say:

  > In contrast to policy distillation, our novelty lies in the ability to, not
  > only distil skills into a single network, but also learn a control rule
  > (using the H-DRLN) that switches between the skills to solve a given task.

  but the control rule is not part of PD but part of the overall high level
  control. It's still standard PD.

- The training for the overall policy is the same idea: minimize Bellman error.
  But to use skills, they modify the Bellman equation slightly, but it's not
  too much of a change. So this is how the learning actually works.

- Yes, after seeing Figure 4 (navigation, pickup, etc., in simplified Minecraft
  domains) it's clear what they mean by skills, and that it's also clear they
  can easily train a policy for that. Then that becomes their pre-trained skill,
  embodied into one Deep Skill Network (DSN).
 
- Weakness: the skills could only be pre-learned, right? They didn't train them
  on the fly?
