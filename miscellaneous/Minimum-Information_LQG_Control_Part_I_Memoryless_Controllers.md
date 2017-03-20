# Minimum-Information LQG Control Part I: Memoryless Controllers

This is a control-theory paper. I'm not too familiar with this field, but there
is some overlap with material that I know. And I imagine that papers like this
are interested in the following style of problems:

> As devices become smaller and more ubiquitous, power efficiency and physical
> restrictions dictate that communication become a limiting factor in the
> agent's operation.

OK, I agree, and it also isn't surprising that classical control theory could
disregard those costs, since the circumstances were different in those times.
Getting a theory of communication here seems to require information theory.

This paper is about **memoryless** controllers, because for some systems it is
impractical to have any sort of memory representation (of the system's
environment). They

- Present a memoryless controller design which satisfies an external cost
  criteria.
- Present the "solution" (I assume this is how the controller decides to act) in
  a simple form.
- Study it!


## Technical Details

**Section II** defines the problem setting. Thankfully, it's clear: linear,
Gaussians, etc.  Look back at this if I get confused, or look up some reference,
since there's nothing new here. We assume LTI controllers and LTI plants,
resulting in a Gaussian stochastic process. This means each time step t is
itself a Gaussian random variable.

**Figure 3** provides some helpful intuition on *controller complexity*, along
with their explanation:

> For example, we can consider a noiseless binary channel and measure the
> controller complexity by the number r of bits per time step that it transmits
> from its sensor to its actuator.

Intuitively, the more complex the controller, the more strength it should have
to preserve the original information, i.e. more bits. (It's like neural networks
vs less powerful models.) This "channel" is their main area of concern in this
paper, and actually they now assume a *Gaussian* channel, not binary.

See **Problem 1** for their main formulation. If our channel is very powerful,
then x_t is "more informative" (since we can better utilize it) and so
I(y_t;x_t) goes down (good) but the costs go up. Their solution appears to
transform the problem into a type of Bayesian network, and then ... boom,
**Lemma 2** comes up with the solution.

Question: what is accomplished by transforming the Bayesian network from Figure
2 into the one in Figure 4? It introduces two extra states which are MMSE's. I
think they do this because the y_t cannot be directly used to get the control
u_t. We have a limited channel so we can't use the entire data. They also argue
Figure 4 by saying that x-hat for u is a "sufficient statistic of u_t for x_t."
I may need to look at this again.

Wait, **Theorem 1** is their main contribution, not Lemma 1. Oops. I will need
to look at this again because it's a lot to process.


## Appendices

Appendix II is probably the easiest for me to understand. I'm not sure if this
was needed for the paper (even for an appendix)? These could just be assumed.
