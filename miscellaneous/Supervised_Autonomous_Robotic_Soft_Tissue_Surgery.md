# Supervised Autonomous Robotic Soft Tissue Surgery

Main novelties and contributions:

- The "Smart Tissue Autonomous Robot" (STAR) which has imaging technology which
  can produce a 3D model of the environment.  It seems like they can get rid of
  occlusion by detecting pre-specified markers in the workspace, which seems
  reasonable. Unfortunately we're not going to get a better imaging system than
  that, I'm pretty sure.

  I see, look at Figure 1C, they use custom "NIRF markers" which are used as
  reference points to plan suture locations. If the suturing tissue moves, the
  markers move as well and the planned suturing path is interpolated. They don't
  model the tissue mathematically, incidentally.

- They've extended RAS to soft tissue, whereas prior successes were mostly with
  rigid (i.e., easier) tissue.

- I still don't see how their closed loop works to autonomously correct for
  different suture positions...

Experiments:

- They conduct suturing and anastomosis tasks, arguing that "all have clinical
  relevance."

- They report on the order of O(1) needles that needed to be re-positioned in
  suturing, which should be avoided at all costs.

- Since this is a medical paper instead of a robotics paper, they perform lots
  of benchmarks to measure the quality of suturing. For instance, they need the
  knots to be evenly spaced. The good news is that this paper does a heck lot
  more realistic surgery than what we do in the AUTOLAB.

See also videos and an article here: 

https://www.theverge.com/2016/5/4/11591024/robot-surgery-autonomous-smart-tissue-star-system

It doesn't cite the AUTOLAB's ICRA 2016 paper or the one from UCLA (ICRA 2017)
since it was published beforehand. 

By the way, 'in vivo' definition: "(of a process) performed or taking place in a
living organism." Sure, I guess that applies here to their porcine tissue.
