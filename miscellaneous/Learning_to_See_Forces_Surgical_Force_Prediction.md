# Learning to See Forces: Surgical Force Prediction with RGB-Point Cloud Temporal Convolutional Networks

Another force-related paper, this time we can use deep learning and sequences of
images to try and predict them, rather than using complicated or hand-built
sensors ("Sensor Substitution") or non-DL based video methods ("Visual
Measurement of Suture Strain") from the past.

> Since it is often challenging to devise a hardware solution with accurate
> force feedback, we propose the use of “visual cues” to infer forces from
> tissue deformation. Endoscopic video is a passive sensor that is freely
> available, in the sense that any minimally-invasive procedure already utilizes
> it. To this end, we employ deep learning to infer forces from video as an
> attractive low-cost and accurate alternative to typically complex and
> expensive hardware solutions.

Sure, the above text (from the abstract) makes sense.

Their proposed solution is a vision-based temporal CNN which predicts surgical
forces. A few details:

- They assume a sequence (or a "window") of images before and after a specific
  time t.

- Input to the temporal CNN: a latent encoding (i.e., features obtained after
  passing images through a pre-trained net like VGG) of the images at each time
  step in the window under consideration.

- How are these latent features created anyway? They use a pre-trained VGG and a
  pre-trained PointNet, to combine features from RGB, point cloud, and depth
  images. This must happen for all the images in the window, and then the
  combination of those is the input to the temporal CNN (see previous bullet
  point).

- Label: a single scalar, indicating the force (in Newtons) at time t. Actually
  they could use (x,y,z) for forces but seems like they only care about `z`? The
  loss function is mean square error.

- For training data, they need to generate it themselves, for obvious reasons.
  They get roughly 60K and 45K data points for their two studies. That's
  impressive! How long did that take?

- They argue from the results that their network can accurately predict smaller
  forces, but perhaps not larger ones. Is that expected, since the larger forces
  are outside the usual range of labels as observed in the training data?
  UPDATE: ha ha, as soon as I read that, they say the same thing I was thinking
  ...

Wow, they use a real pig liver. Impressive!
