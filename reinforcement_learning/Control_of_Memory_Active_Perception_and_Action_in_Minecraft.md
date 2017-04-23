# Control of Memory, Active Perception, and Action in Minecraft

Wow, looks like an interesting paper. [The source code is on GitHub][1], in
Torch unfortunately, but maybe I should get around to learning how to use it.

This paper proposes several new architectures to handle the simultaneous
challenges of:

- Partial observability
- Active perception
- Generalization

However, their paper isn't using "active perception" the way I usually think of
it. I view "active perception" as when an agent is in an environment and has to
actively perform actions (e.g. search for items) which will maximize its
knowledge and reward. Think of that Stanford robotics experiment where the robot
adjusted either itself or the object to gain a better understanding of it. This
paper seems more concerned with using perception "as a side effect" when the
agent is walking around in a maze and picks up information as it moves, e.g.
with the indicator functions.

This paper is very heavy on the experiments, both in terms of breadth and depth.
I'm impressed! The danger is that some people may think of these as just grid
worlds, but it's actually different because the agent is seeing raw pixels in
Minecraft environments as humans would see it. Minecraft images are processed to
be 32x32 RGB images as input to the CNN portion of their architectures.

The architectures they use are based on Deep-Q-Networks (yay) and Neural Turing
Machines (uh oh). Looks like I really need to know how NTMs work.

Bottom line: nice experimental paper, I want to know more about the
architectures, but it's perhaps less relevant to my immediate research as I
would have hoped.

[1]:https://github.com/junhyukoh/icml2016-minecraft
