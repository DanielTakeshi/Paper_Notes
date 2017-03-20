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

**Section II** defines the problem setting. Thankfully, it's clear: linear,
Gaussians, etc.  Look back at this if I get confused, or look up some reference,
since there's nothing new here.
