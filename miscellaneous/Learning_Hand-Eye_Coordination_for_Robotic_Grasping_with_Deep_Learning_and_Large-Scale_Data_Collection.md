# Learning Hand-Eye Coordination for Robotic Grasping with Deep Learning and Large-Scale Data Collection

This is a splendid paper that is easy to read and describes another awesome
thing that Deep Learning can do. In short, this paper shows that, if you train
using their method, a robot arm with a gripper can grasp objects when only given
the camera frame. It does NOT need calibration (yay!) or precise dynamics.  All
we see are the camera which contains a view of the gripper in it and then there
is a visual servoing mechanism which will figure out the correct way to move the
robot so that the desired stuff in a bin is grasped.

Specific contributions:

- A **grasp predictor network**, taking in an image of the robotic arm and the
  bin of objects (plus another image of the bin without the arm in view) **AND**
  a robotic motion vector `v_t`. Given the input `(I_0,I_t),v_t`, the network
  predicts the probability that this will lead to a successful grasp.

- A **visual servoing function** which takes in an image and has to figure out
  which motion `v_t` to use. It's easy to remember this baseline: randomly
  generate `v_t` and then use the grasp predictor network to predict which one
  is best, and then use that motion! They improve on this via the CEM. This way
  is nice because by generating `v_t` it's also easy to impose constraints, i.e.
  we can filter out any obviously undesirable movements.

- The training data was collected via 6-14 robots working in parallel,
  automatically grasping objects over several months (!!). What's key is that
  these produce data points `(I_t^i,p_T^i-p_t^i,l_i)` where we have the
  *difference* between two vectors. It was somewhat confusing to me but it now
  makes sense. It was collected over a long time and the network re-fitted
  periodically, which reminds me of fitted Q-iteration.

- Note: it wasn't totally clear to me at first, but the difference `p_T-p_t` is
  indeed the same as `v_t`. Got it? See the color-coded Figure 3 for a
  confirmation.

My main questions mostly have to do with the experimental setup, since I'm not
totally sure how it was set up. For instance, didn't a human have to keep
constantly putting stuff back in the bin? And in addition, what is that `N`
value that appears in the table? It's not the `N` from the CEM, and I thought it
was the number of trials but the paper says "4" somewhere. Finally, it can't be
the number of objects in a bin since that would be a lot and the paper said that
30 grasp attempts was enough to clear the bin (so if `N=40` that doesn't make
sense). Intuition says that it's the number of trials, so hopefully that is the
case.

The other main thing I was wondering about is ... god, their network is SO DEEP.
Did they really need all those layers?!?

Update: quick note, as I investigate whether we use RGB or depth images, I note
that this paper uses RGB images and out-performs some depth-based baselines.
Though, I wonder what would happen if we just trained on depth images?
