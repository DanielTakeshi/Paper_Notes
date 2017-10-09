# TSC-DL: Unsupervised Trajectory Segmentation of Multi-Modal Surgical Demonstrations with Deep Learning

This is *another* surgical robotics paper from Ken Goldberg's group. This
paper's main contribution is their unsupervised learning algorithm **Transition
State Clustering with Deep Learning** (TSC-DL) to identify transitions in
demonstrations (from image/video data). They optimize the metric **"Silhouette
Score" (SS)** for cluster tightness, and also to match up with human
annotations, which is quantified with **"Normalized Mutual Information" (NMI)**.
See *Section IV: Evaluation Metrics* for details.

The TSC problem involves multiple tasks which together solve the overall thing
(e.g. surgical suturing) and part of TSC-DL's job is to find the points where
one task (maybe call them sub-tasks?) ends and the next one begins.

- A **transition state x(t)** is defined as one such that **A(t) = A(t+1)**.

They say they get visual features from pre-trained CNNs; they use either AlexNet
or VGG. These features are what help TSC-DL identify transition points!

The abstract says:

> Segmentation of these trajectories into locally-similar contiguous sections
> can facilitate learning from demonstrations, skill assessment, and salvaging
> good segments from otherwise inconsistent demonstrations.

However they do not actually test this. The only tests deal with identifying
segments. It remains to future work to figure out how to *apply* this to real
problems. Look at their follow-up work: SWIRL and DDCO.

Their models are based on stochastic linear dynamics systems, with state spaces
denoted as `x(t)`. Fine print: the states are concatenated across a window of
several time steps.

It's a bit hard for me to get intuition on the featurization techniques as
those methods are dependent on prior work.

The dataset, at least for suturing and needle-passing, is from JHU JIGSAWS. I
think the dataset has 40 demonstrations, of which they used a subset for
suturing and then 28 for needle-passing (please tell us the exact amount
clearly!).
