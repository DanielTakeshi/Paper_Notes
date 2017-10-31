# Target-Driven Visual Navigation in Indoor Scenes using Deep Reinforcement Learning

This paper proposes to improve generality in Deep RL by having an agent take an
image of the *target* as input, *in addition to* the current image the agent
sees. Note that both the target and the current state are images, so they're raw
pixels. This should help improve generality as default RL policies map only the
current state to an action, so the target is implicitly embedded.

The target and current images are passed through a siamese network, which they
modify to output a policy (the actor) and the value function (a critic).
Basically it's similar to how A3C works, we have shared parameters between the
two and then they branch out at the end.

Training proceeds in a manner similar to A3C, where instead of having multiple
threads run independent episodes, different threads can run different targets.

I think this paper is also well-known for introducing the THOR environment for
navigation. I should try it out one of these days. It provides a rich simulation
environment for navigation. They show that their algorithm results in fewer
steps needed to reach a target as do competing methods.

The experiments with the real robots at the end are a huge plus, showing another
case of simulation-to-real transfer which is hugely popular these days.

Figure 5 is on data efficiency for **training** so they didn't do a training and
testing split there. But See Section V-B for held-out targets.
