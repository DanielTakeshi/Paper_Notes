# Active Perception: Interactive Manipulation for Improving Object Detection

This is a little-known technical report but it may be useful for me.

When doing object detection from robot cameras, it is better to *actively* move
to find better viewpoints for detection (and classification FYI). This can be
either the robot moving itself, or the robot moving the object. I agree, this
makes sense.

I like this quote:

> In a sense, one of the main reasons for the low performance of object
> detection algorithms is that they are designed to work with static images and
> constrained to not being able to interact directly with the environment. We
> recognize objects better not only because our vision algorithm in the brain is
> better but also usually we have the hands to manipulate the environment.

They perform both **manipulation** and **navigation** to get different
viewpoints. The former means the robot moves the object, and the latter means
the robot moves itself.

The most straightforward, non-trivial optimization procedure is to use the one
described in Section III-B. It measures the ratio between information(actions)
and cost(actions). The former is much harder to compute than the latter. They
augment their objective with "views" because that's what ultimately matters. But
then they say the problem is intractable, so they resort to approximation using
mutual information. =) How do we maximize mutual information among views?
(Intuitively, this means we get the most bang for our buck.) For that, use
**Gaussian Processes**. See Section III-C.

Remark: The notation in Equation 5 is really bad. We should use single bars to
represent cardinality, not double bars which are more generally called norms.

Remark: Why does Equation 5 make sense? I think it's because if we maximize
this, it means the set of vies O and V\O depend on each other a lot. Thus, by
choosing our "O" well, we know as much as we can about the remaining V\O
viewpoints.

Practical algorithm: greedily pick the most "valuable" viewpoint one by one,
where the value is determined by mutual information.  However, the mutual
information relies on the assumption that we're dealing with Gaussian processes.
Also, where does the Gaussian process come from? Where are our samples??

Their experiments deal with their **STAIR robot** which finds *cups*, *mugs*,
and *staplers*. They went from 23/35 accuracy to 34/35 accuracy. That's good,
though I can see why this is "only" a technical report now. See Table I for the
actions they used/implemented. Action costs were measured based on time.

If I collect data myself, I can say something like they did, only far more
specific:

> For training, we collected 150 images of cluttered office scenes. We labeled
> the objects of interest and their corresponding views. This labeled data was
> used to learn the parameters of Gaussian Processes in our algorithm.

For benchmarking, I agree, (1) their algorithm, (2) one view, (3) random
strategy. Yes, that's the best way to do it. Ideally, there would be a fourth,
based on the current state of the art. One challenge with this is coming up with
a good-enough benchmark. From Figure 6, they just needed three actions to get
almost 100% accuracy. For a more extensive paper, one might need harder tasks.
Also, the randomization benchmark is pretty bad, it takes about 3x as long as
their method.

I haven't been able to analyze Tables II, III, and IV yet.

Final note: I'm sorry, I just had to note this:

> We used a state-of-the-art sliding-window method for object detection.

Yeah, that's what 2010 was like, the year I graduated from high school ...
