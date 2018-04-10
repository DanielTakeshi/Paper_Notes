# One-Shot Imitation from Observing Humans via Domain-Adaptive Meta-Learning

TL;DR like the one-shot visual imitation paper from CoRL, except they figured
out how to get the robot to do one-shot imitation from a video of a human, not a
single tele-operated robot trajectory. And no, it's not their two-headed
architecture, it's based on a WaveNet-like model that can model actions, and
using both human videos and robot teleoperated videos in the training data,
where the robot teleop-ed videos can provide ground truth, and the human videos
can be used to train the WaveNet-like model to look at the relevant pixels that
move.
