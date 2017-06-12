# TSC-DL: Unsupervised Trajectory Segmentation of Multi-Modal Surgical Demonstrations with Deep Learning

This is *another* surgical robotics paper from Ken Goldberg's group. This
paper's main contribution is their algorithm **Transition State Clustering with
Deep Learning** (TSC-DL). It looks like they're trying to optimize the metric
"Silhouette Score" and also to match up with human annotations. They can
quantify this difference with "Normalized Mutual Information."

The TSC problem, I believe, involves multiple tasks which together solve the
overall thing (e.g. surgical suturing) and part of TSC-DL's job is to find the
points where one task (maybe call them sub-tasks?) ends and the next one begins.
Actually, that might be the *main/only* task, judging from Figure 1?

- A **transition state x(t)** is defined as one such that **A(t) = A(t+1)**.

They say they get visual features from pre-trained CNNs; they use either AlexNet
or VGG.
