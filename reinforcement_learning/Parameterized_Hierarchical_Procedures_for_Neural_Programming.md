# Parameterized Hierarchical Procedures for Neural Programming

Think: why does this work, and could I implement this? (I only had time to read
this briefly so I don't yet know how to implement this.)

The TL;DR of this is that they apply a DDCO-like algorithm for solving neural
programs (it's technically DDO since they have discrete actions). They extend
DDO by adding "program counters" which indicate different levels of the
hierarchy. A similar Expectation-Gradient thing is done for the "Maximization"
step since maximizing is intractable.
