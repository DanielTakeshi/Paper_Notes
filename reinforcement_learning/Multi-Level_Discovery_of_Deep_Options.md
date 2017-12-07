# Multi-Level Discovery of Deep Options

Update: [notes available here][1].

**What is the main idea?**

- Using the (assumed) hierarchical structure of demonstrations (in an RL/IL
  environment) automatically discover useful options (i.e., primitives) which
  can execute for some time steps before terminating, and returning control back
  to a higher-level meta policy.

**Why does this work?**

- Options are supposed to be smaller fragments and should be executed for
  several time steps without being hindered by random exploration.
- Expectation-Gradient will maximize latent variables + likelihood, and works
  just like EM is supposed to "work". So we not only maximize likelihood (which
  intuitively should lead to matching the expert) but also sample the latent
  variables.
- TODO: really need to try this myself.

**Could I implement this?**

- Update: I know how the EG update works at the lowest level now, since I
  derived the math. Now to blog about it. :-)
- Almost there ... still slightly confused on (a) how to augment it with options
  after a certain point, but I will work on the GridWorld stuff soon^TM, and (b)
  how does the Baum-Welch update works? (I know B-W but I forgot it.)

**Possible drawbacks?**

- Atari RAM, rather than just the usual Atari. Figure 3 is not entirely
  convincing and at this time of AI research, reviewers need to see more than 5
  Atari games with greater significance. 
- There are too many experiments based on GridWorld, and GridWorld seems to be
  the only experiment that involves more than two hierarchies (and it uses
  three, not an arbitrary amount?).
- Some conjectures are stated but not fully investigated.
- Little analysis of how the quality of the supervisor matters.


[1]:https://danieltakeshi.github.io/2017/11/24/ddo/
