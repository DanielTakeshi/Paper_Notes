# Cloth Manipulation Using Random-Forest-Based Controller Parametrization

**TODO: notes in progress**.

Develop a mapping: `visual features ==> control action`, where the mapping is
parameterized as a random forest. I wonder how Deep Learning stacks up --- and
they actually claim that random forests are *superior* to CNNs, and nearest
neighbors search for that matter. Well, I know from some personal experience ...
maybe I should tell the authors about the paper we wrote. The task might not be
complicated enough for a CNN. Data is collected and the training is done online,
to repeatedly train the random forest. (You can do the same thing with any
machine learning model, though.) The data come from images; they do not assume
that the robot can actually see the configuration `c`, a 3D mesh of cloth. This
is a very reasonable assumption.

Part of their motivation for using random forests, I believe, is that they can
dynamically construct the RF by making it bigger and bigger (i.e., deeper) while
reducing overfitting, and if the *architecture* converges with enough data, then
at the point of convergence we know we have enough data.

In the context of reducing the dimension of images and extracting the relevant
parts (i.e., features), they say:

> Recently, deep neural networks have also been used as general-purpose feature
> extractors.  They have also been used to manipulate low-DOF articulated bodies
> [5] and in DOM applications [19, 20]. For simplicity, our random-forest uses
> HOW-features as inputs.

(I.e., this paper uses the same features that wer

There is an advantage to simplicity, and I can agree. The tradeoff is that
networks *should* outperform RFs given more complexity and more data. I can't
see how RFs can keep up.

Regarding problem formulation, Equations (3) and (4), I am interested in
pursuing (3).

They mention ArcSim, from James O'Brien's group, in 2012. They use Gazebo for
robot kinematics. Is it easy to combine the two? It seems like their method for
*training* will not work reliably in the physical world, they train in the
simulator. They were able to transfer, though.


