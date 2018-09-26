# Dex-Net 1.0 and Dex-Net 2.0

I'm putting both version 1 and version 2 here. (Version 3 is a bit of a
different flavor, I'd say.) I've read the papers but don't have time for
polished notes. Fortunately, Jeff already did that:

http://bair.berkeley.edu/blog/2017/06/27/dexnet-2.0/

Update, some Dex-Net 2.0 comments:

- The paper has three main contributions: (1) a *synthetic* dataset, (2) the
  GQ-CNN architecture, and (3) a planning method for grasps.

- The dataset generation results in 6.7 million data points (!!), requires
  taking 3D object models (in simulation) and randomly sampling grasp candidates
  (those are the red/green lines in the paper's images), and then sampling for
  the robustness.

- The GQ-CNN uses *depth* images as input, and has to predict the probability of
  success of a grasp. It takes in an image and a grasp candidate, so that's
  similar to prior work. BUT some of the information inside the 'grasp
  candidate' is used to transform the input image to make it 32x32 and with the
  correct angle. It seems like the only thing that's left in the grasp candidate
  is the z, the depth value.

- The grasping policy involves sampling (with Cross-Entropy Method as needed)
  and then querying the GQ-CNN, so it's very similar to Sergey's paper.
