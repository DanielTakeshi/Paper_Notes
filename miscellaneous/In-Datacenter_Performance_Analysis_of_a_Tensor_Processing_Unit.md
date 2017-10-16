# In-Datacenter Performance Analysis of a Tensor Processing Unit

I'm interested in this paper because of the Tensor Processing Unit (TPUs), which
might dramatically accelerate Deep Learning computations, and any further
improvements in speed at this point are enormously appreciated. I think of TPUs
as replacements for CPUs or GPUs, or at least replacements only for the matrix
multiplication parts for neural networks. They're domain specific hardware to
Deep Learning.

The paper claims that the TPU is 15x-30x faster than contemporary GPUs and
CPUs!! Now obviously we have to be careful about how exactly to interpret it,
but this sounds huge.

This only seems to apply for the inferential stage (i.e., prediction) of DNNs,
and not the training, which is a bit of a bummer but for Google, the inferential
part is more important since it's what the user would see or experience in real
time. Their priority shifted in 2013 when people were using their speech
recognition DNNs.

See Figure 1 for the block diagram. The most important part to know about is the
**Matrix Multiply Unit**.

It's going to be an exciting year. :-) Apparently, the TPU caused NVIDIA to
start responding with a hybrid architecture, so I will look forward to seeing
what these heavyweights have to say.
