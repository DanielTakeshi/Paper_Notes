#  Virtual to Real Reinforcement Learning for Autonomous Driving

This paper fits in the theme of virtual-to-real reinforcement learning (e.g.,
OpenAI's domain randomization technique), this time with applications to
autonomous driving. The obvious question, though, is how to ensure that virtual
training generalizes to real world data, given that it's hard to model the
latter's distribution.

Their framework involves taking virtual images, putting them through an
encoder/decoder to get a **scene parsing representation**, and then through a
SECOND encoder/decoder network to get a real image. (This seems a bit
surprising, since normally in the VAE framework we think of just one encoder and
one decoder. But it sounds interesting.)

The point of the above is to get **realistic images** starting from the fake
ones in their original simulator (via GAN-like tactics). They then supply the
realistic images as data for the reinforcement learning agents. In order for
this to work, they need the intermediate "scene parsing" representation as
suggested by prior work, which must have justified the architecture choice.
(Think of that as a latent variable? Also I think the relevant prior work is
Isola et al., CVPR 2017)

Looks like an interesting follow-up paper to read `Soon^TM`: "Image-to-Image
Translation with Conditional Adversarial Networks," by the Berkeley computer
vision group. Oh, and also NVIDIA's arXiv paper on end-to-end learning for
self-driving cars.

BTW: even though it's advertised as "real" reinforcement learning, even the
"real" part is a RL environment in simulation. It's not on a real-life car. So
it's sometimes ambiguous what we mean by "real" or "virtual."

- To get virtual images, take them from the TORCS simulator.

- To get real images, take them from real-life data; they used an
  Autopilot-TensorFlow reference.

- For the real RL environment ... I think (?) they used TORCS but instead
  augmented it with real images. Not sure, and this is kind of important. What's
  confusing is that they say
  
  > [...] the trained policy was tested on a real world driving data to evaluate
  > its steering angle prediction accuracy.
  
  But that is supervised learning. The RL part must have come beforehand.

The results plot (Figure 5) needs a few more curves and error bars to be more
convincing, in my opinion.
