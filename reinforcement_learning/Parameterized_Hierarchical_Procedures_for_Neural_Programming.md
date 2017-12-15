# Parameterized Hierarchical Procedures for Neural Programming

The TL;DR of this is that they apply a DDCO-like algorithm for solving neural
programs (it's technically [DDO][1] since they have discrete actions). They
extend DDO by adding "program counters" which indicate different levels of the
hierarchy. A similar Expectation-Gradient thing is done for the "Maximization"
step since maximizing is intractable.

They are doing LfD to train a neural program. This can be done using either
**strongly-supervised** (i.e., annotated) data, or **weakly-supervised** (i.e.,
not annotated) data. Or perhaps a combination of each. More precisely, think of
strongly supervised as having the latent variables seen, which can be viewed as
some for of annotation anyway.

In order to make effective use of DDO, we need to assume our data has the form
of a hierarchy. The hypothesis is that neural programs are supposed to simulate
"computer programs" as perhaps designed by humans, and those certainly have a
hierarchical structure, as users can call methods which then call other methods,
etc. If anything, the demonstrations we get with neural programs exhibit almost
an "exact" hierarchy, certainly much more than in normal IL/RL environments, and
they are very rigid, in the sense that any errors (e.g., in a long addition
task) make the entire output invalid. Contrast that to some control environments
which can tolerate slight deviations in the agent's actions as long as it
corrects itself later.

The algorithmic nature of the demonstrations here means that (e.g., with an RNN)
standard gradient descent with input-output pairs is unlikely to work, which is
why people use specialized architectures such as the *Neural Turing Machine*.


## How does it work?

They present the algorithm **Parameterized Hierarchical Procedures (PHPs)**. A
PHP is a *sequence of statements* `{s1,s2,...,sN}`, each of which, based on the
current observation can:

- perform an elementary operation.
- invoke another PHP as a sub-procedure.
- terminate and return control to the caller PHP.

Each statement `si` has an **index** which serves as the **program counter**,
allowing the PHP (or the set of PHPs, really?) to remember which PHP is actually
calling it. The overall policy should be a set of PHPs, right? Yes they use
`\mathcal{H}` as the finite set of hierarchical procedures, each of which is a
sequence of statements.

**Statements**: write these as `\sigma_h^t = (\eta_h^t, \psi_h^t)`.

- `\eta_h^t`: operation statement, as a function of the observation, returns
  either (a) an elementary action, or (b) another hierarchical procedure.
- `\psi_h^t`: termination statement, as a function of the observation, returns
  either 0 or 1.
- The `h` and `t` mean we *select* at step `t` of procedure `h`.
- And yes, the `h` is the same as the PHP. One of these must be the *root*.

**Neural networks**: given counter `c \in R` and observation `obs \in R^{...}`,
we map it to some representation of the three possible choices. Got it.

**How it executes**: (most helpful part of the paper)

- Maintain a *stack*: `[(h_1,t_1), ..., (h_n,t_n)]`.
- See Figure 1 for an example.

**Training**: use Expectation-Gradient, following the DDO/DDCO paradigm. What
changes is the program counter. Does that affect the math?

**Why should PHPs be better?**: the program counter enforces some algorithmic
constraints. The call stack and program counters effectively encode "memory" and
allow us to avoid using RNNs.


## Other Stuff

**Experiments**:

- NanoCraft: state is a 6x6 grid where the agent places blocks. The input is a
  5-D real-valued feature vector (wow, that's tiny).

- Long Hand Addition: they do a limited form of addition, with adding digits and
  carrying them over. The input is 44 dimensions, so slightly larger than
  NanoCraft.

For both of these, I wish there were more examples of execution traces.

**Questions**:

- Is the overall policy learned a *set* of PHPs? If so, how many are there?
  Answer: yes, the policy overall is a set of PHPs, with the actual amount
  probably up to the user (maybe do cross-validation to determine a good
  number?).

- Does each PHP have its own neural network? Answer: Not quite, they actually
  need *two* networks for each, one for the operation and another for the
  termination. The counter (just a real number) is concatenated with the
  observation as input. Got it. Their state spaces are small enough that 1 extra
  addition can make a difference; I wonder how it generalizes to MNIST (784
  dimensions).

- Does each PHP have its own program counter, or do statements within a PHP have
  their own program counters? I see some conflicting language. Answer: each PHP
  that is called has its own program counter, *which indicates the number of
  time steps that the PHP has been executed for elementary actions*. When we add
  PHPs to the memory stack, they start at counter 0. 

  (There may be multiple instances of the PHPs being called, just like how
  functions can be called many times, and for each of those calls, we have one
  copy of that PHP on the call stack.)

- How is the full memory state encoded? Answer: it's the entire stack of PHPs
  and their program counters.

- What's the difference between the input-output pairs and execution traces?
  Moreover, how exactly is the data formatted for demonstrations? I assume
  execution traces are more specific to the neural programming problem, but I
  still don't know the details. Answer: TODO.

- Approximate the root PHP with an LSTM? How does this work? Answer: TODO.

- Why does the naive approach to generalizing to multi-level PHPs result in an
  exponential-blowup? (The answer to this should be obvious but I just want to
  see it in math form.)


[1]:https://danieltakeshi.github.io/2017/11/24/ddo/
