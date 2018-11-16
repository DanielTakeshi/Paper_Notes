# Reward Learning from Human Preferences and Demonstrations in Atari

Reminds me of "Deep RL From Human Preferences." Unsurprisingly, given the author
affiliations, they say that paper is their predecessor. They also cite a bunch
of combo RL/IL papers, which by now I am deeply familiar with. Whew.

What's interesting is that they argue the "Deep RL from Human Preferences" paper
is inefficient in soliciting human feedback, yet one of the benefits of the
prior paper was the ability to learn from human preferences using just a small
amount of bits:

> In this work, we focus on reward learning from trajectory preferences in the
> same way as Christiano et al. (2017). However, learning a reward function from
> trajectory preferences expressed by a human suffers from two problems:
>
> Preferences are an inefficient way of soliciting information from humans,
> providing only a few hundred bits per hour per human.

By efficiency, I guess they wanted bits per hour, so while their predecessor was
efficient in terms of bit usage, it was inefficient in terms of time since the
humans had to spend more time on the task.

Main idea: use IL, but also *learn the reward function* from human feedback, and
then use RL on top of that. To be clear, the RL part uses their *learned* reward
function. Additionally, their IL part --- initially --- is actually the
pre-training stage in DQfD, which I have implemented. Evaluate on Atari, since
it's a common benchmark with natural true rewards, which enables a comparison
between learned (i.e., from the deep network) vs actual (i.e., from the game
emulator) rewards.
