# Learning Robust Bed Making using Deep Imitation Learning with Dart

Argh, I had a good summary here once but deleted it ... ?!?

Well, the point is this:

- Pre-define the bed-making task as much as possible to make it open-loop.
- But for the **sheet grapsing** part, use pre-trained YOLO network for this. Do
  LfD on just that thing. 
- Do data augmentation by flipping images, etc.
- Do data variation via DART to reduce covariate shift.
- Then performance is better as measured by sheet coverage.

The more I read papers like these, the more I understand that it's critical to
start with a good problem that's interesting enough when solved, but is also
doable and isn't too heavily hand-engineered.
