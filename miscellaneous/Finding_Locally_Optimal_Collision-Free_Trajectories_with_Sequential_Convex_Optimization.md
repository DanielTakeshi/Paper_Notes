# Finding Locally Optimal, Collision-Free Trajectories with Sequential Convex Optimization

This seems to be a well-known paper about trajectory optimization. Think of this
paper whenever I see the software package called "trajopt." In fact, this has
more citations than the TRPO paper as of today, though the TRPO one will likely
surpass it by mid-2018 at the latest.

Anyway, let's get to the main purpose of the paper. What is it? It's to find
good trajectories for robots during motion planning, and the key here is to
*avoid collisions* in a fast, scalable, robust way. There's a decent
introduction to trajectory optimization here, which emphasizes the need for
optimizing (which here we use sequential convex optimization) and checking
collisions (signed distances here).

They say they don't deal with dynamic constraints (see Section III) but what
does that actually *mean*??

**Sequential Convex Optimization**, this definition makes sense:

> Sequential convex optimization solves a non-convex optimization problem by
> repeatedly constructing a convex subproblem --— an approximation to the
> problem around the current iterate x. The subproblem is used to generate a
> step ∆x that makes progress on the original problem.

The algorithm is actually important enough that it was discussed in CS 287 Fall
2015, when I took it. They use **quadratic programming** to approximate the
subproblem in a convex manner.

The parts about managing constraints, dealing with signed distances, etc., seem
reasonable. There's some novel stuff in **Section V**, I assume, about dealing
with continuous time constraints.  Why is this important? Because at each time
step they consider, the robot will be collision-free from their previous
algorithm. **BUT** ... the time steps are effectively discretized, so the
continuous motion from A(t) to A(t+1) means that intermediate points risk
running into collisions.

Experiments:

- **MoveIt!**. This is simulation.

- **PR2**. This is real-life.

- **Atlas Humanoid Robot**. This has the most DOF so it's the most challenging
  in theory, but it's a simulation compared to the reality of the PR2. Still,
  it's challenging!

The videos are quite informative. Check them out [on the paper website][1].

They show that their algorithms are superior to competing methods.

To review/learn:

- CHOMP
- STOMP

Yeah, I forgot how these work since I've been getting distracted with Deep
Learning lately. But CHOMP is related to Hamiltonian Monte Carlo in some way.

[1]:http://rll.berkeley.edu/trajopt/rss/
