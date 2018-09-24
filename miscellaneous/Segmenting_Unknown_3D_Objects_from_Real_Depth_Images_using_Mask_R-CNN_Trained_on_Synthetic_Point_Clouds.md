# Segmenting Unknown 3D Objects from Real Depth Images using Mask R-CNN Trained on Synthetic Point Clouds

**Contribution**: borrow a computer vision architecture, Mask R-CNN, train on
automatically-generated depth data for the segmentation problem, where we have
to identify "relevant/interesting" areas in a given image. In robotic grasping,
this is for identifying objects, which helps for obvious reasons!

The paper's contributions include a method for automatically generating
(synthetic) training data.

- I think this is a general trend: many robotics papers use Deep Learning, and
  if they use images, the papers often borrow computer vision techniques.  But
  the CV people do not usually have to worry about dataset generation, whereas
  in robotics, data collection is _itself_ a critical problem.  That's why
  self-supervision for grasping is really nice, we can get data.

- Details: 50k depth images (!! I only needed ~2k...), 320k object masks (!!).
  Seriously did they need that much ...? 800 real images took 35 hours (!!) to
  label.

They use depth data, which makes sense to avoid having to deal with colors as
confounding factors. Robots nowadays use depth cameras, and as they get more
sophisticated (or at least, their accessories), I conjecture that we will simply
get better and better depth images at lower cost (they can be noisy with NaNs).
What I also like about the paper is its generalization to *lower resolution*
cameras.

The WISDOM data has simulated and real data, and the real data is itself further
split into half high-res, half low-res. The simulated data is generated via
their task and observation distributions; see Section V. OK, seems reasonable.
And they are advertising this as an alternative dataset specific to warehouses
and cluttered environments, rather than "natural" images as in many computer
vision datasets. But they *also* test with the natural version, which I believe
is COCO.

I see, like in Jeff's CoRL paper, they use a pybullet simulator to simulate
objects falling into a bin. That's how you can try and get various "heaps."
Yeah, it seems like Jeff's paper handled a lot of the initial technical details
and engineering work of generating object heaps, so that follow-up work can just
invoke it.

After reading this paper, it looks like there are several things I need to brush
up upon:

- Mask R-CNN, obviously!
- ResNet, well I already reviewed this but should probably do it again.
- The COCO dataset: http://cocodataset.org/#home
