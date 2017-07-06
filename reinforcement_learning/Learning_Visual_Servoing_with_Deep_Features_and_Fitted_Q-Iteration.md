# Learning Visual Servoing with Deep Features and Fitted Q-Iteration

Main ideas:

- Given observations from a camera, how should we teach a robot where to move to
  in response? (That's visual servoing.) Classical algorithms require access to
  "good features" or knowledge of system dynamics, but they improve on this
  (performance, generalization, etc.) using Deep Learning techniques, to try and
  get the benefits of recent Computer Vision advances for the problem of
  servoing. More specifically, they *learn* "predictive" features, *learn*
  system dynamics, and use reinforcement learning techniques.

- Uses a **bilienar predictive model** to estimate predictive features.

- Uses **sample-efficient fitted Q-iteration** which assigns higher weight to
  the important features. I'm not too familiar with (neural?) fitted
  Q-iteration, but it might be [worth reading more about further][1].

There's lots of great information here: http://rll.berkeley.edu/visual_servoing/
