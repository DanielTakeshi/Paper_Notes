# A Vision-Guided Multi-Robot Cooperation Framework for  Learning-by-Demonstration and Task Reproduction

**What is the paper contribution/novelty?**

- Doing LfDs with humans performing the demonstrations with both hands.

- Using robust tracking systems (e.g., a kalman filter to track the data) for
  better visual servoing.

It's still not clear to me, though ... however, the combination of LfD and
visual servoing seems fairly common, I remember Sanjay's done that with the
dVRK.  They use a Kalman filter for this, but again that's very common so where
is the novelty?

Maybe their main novelty is the "bimanual" task where we have two robot arms. 

They also focus on using tools, so move the tool's tip by moving the
end-effector's top, under the assumption that the tool is held rigidly. That's
another important thing I can do.

Their main experimental/task contribution is a bimanual sewing task.


**Why does their method work?**

- Unfortunately I'm not totally sure ... it's a bit confusing but I'll re-read
  the paper.
