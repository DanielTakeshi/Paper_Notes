# Sim-to-Real Reinforcement Learning for Deformable Object Manipulation

Combines the following:

- sim-to-real domain randomization
- DDPG reinforcement learning with a bunch of extensions
- deformable object manipulation

The main novelty is the deformable object manipulation and the tasks of trying
to fold sheets and to put sheets on a hanger, and to implement this as a
reinforcement learning environment, with sparse rewards. (By now we know we can
do some IL/RL with sparse rewards. They also seed with demonstrations, so they
use the DDPG algorithm from DeepMind with demonstrations, which is analogous to
DQN from Demonstrations (DQfD).
    
The simulator for the sheet is implemented by building on top of PyBullet.
Unfortunately they had to modify the simulator somewhat ... and I am not sure
how easy it is to do that as the code is not available. Fortunately their DDPG
is built on top of OpenAI baselines, as they claim. But again I will need to see
the cloth simulator. How does that work?

See the website here: https://sites.google.com/view/sim-to-real-deformable

Hopefully they can put some code documentation there.

**EDIT/UPDATE**: I found Jan Matas' repositories, though, so I could use those
if needed. Coding PyBullet would require a lot of work, though...

The real experiments involve 30 trials on the robot. And, their success rates
look good. Again, it would be easier to see more videos of this, rather than
only the stuff in the currently-released video. There are different metrics of
success, depending on the distance to the target, as shown in Table 2.
Surprisingly, one of the distances for the tape-folding task in the real-world
outperforms the simulated version, but the simulated one must have had some
stricter success requirements.
