# Learning by Observation for Surgical Subtasks: Multilateral Cutting of 3D Viscoelastic and 2D Orthotropic Tissue Phantoms

**Main Idea**: Let's design the tasks of debridement and pattern cutting as
finite state machines (i.e. a set of rules with if-then steps, etc) based on
human demonstrators, and then automate those tasks with dvrk.

**Main Points**:

- The "finite state machine" thing is really a set of rules to follow, if we do
  this then do that, otherwise return to this, etc. The "learning by
  observation" terminology refers to using human demonstrators to update their
  finite state machine. The "machine" depends on precise parameters which have
  to be manually adjusted as needed.

- **Debridement**: split into about five subtasks (not including detection). The
  detection part uses OpenCV and hue-saturation-value separation for detection
  of those parts to remove. Repeatability (i.e. success rate) of 96% in 50 trials.

- **Pattern Cutting**: split into about ten subtasks. More OpenCV stuff to
  detect contours.  Repeatability (i.e. success rate) of 70% in 20 trials.

**Thoughts**: I think the whole Finite-State Machine thing may be overly formal
for their purposes, particularly with the debridement stuff. But maybe this is
standard for such papers. (Also, Fig. 5 about the software stack is really
unhelpful, not that informative.) Regarding the experimental results, I just
wish there's a better way to describe it in a paper rather than regurgitating
statistics derived from their tables. I would prefer more high-level conclusions
from the tables. They do have a video, though, and the result looks really
awesome there!! A lot of the paper is also about simply describing the rules for
each step, which definitely took a lot of time and tweaking with OpenCV.

(Note: this paper was a finalist for the best medical robotics paper at ICRA 2015.)
