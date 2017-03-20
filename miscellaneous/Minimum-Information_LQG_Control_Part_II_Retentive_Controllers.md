# Minimum-Information LQG Control Part II: Retentive Controllers

This is the continuation of the [first paper][1], both of which were accepted to
the same conference (CDC 2016). They are able to somehow reduce the problem
setting here to that of Part I (the memoryless controller). The definition of a
retentive controller (i.e. agent):

> When the controller is retentive (memory-utilizing), it does maintain an
> internal memory state which can have information on more than the most recent
> observation. 

As expected, to measure the amount of memory held, and to measure information
overall, use *information theory*. The guiding principle here is to *measure the
information complexity of the internal controller*. Remember this and repeat it
to myself.

**Main contribution**: the design of a controller which is optimal under
constraints on memory and sensor channels. Their other paper didn't consider
memory constraints.


## Main Ideas and Intuition

Figures 1 and 2 make sense. However, for Figure 1, there isn't the analogue in
the first paper (Part I) about adding in MMSEs. Why is that?

Section II: already know.


## Technical Details

I remember needing forward and backward passes to compute probabilities of
hidden states for Hidden Markov Models and Kalman Filters. Is something similar
going on here with their three algorithms?


[1]:https://github.com/DanielTakeshi/Paper_Notes/blob/master/miscellaneous/Minimum-Information_LQG_Control_Part_I_Memoryless_Controllers.md
