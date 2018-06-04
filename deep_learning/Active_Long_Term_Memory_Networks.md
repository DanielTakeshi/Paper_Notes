# Active Long Term Memory Networks

This is another paper in the long line of work about continued, lifelong
learning in neural networks. How does one design network that are able to
maintain previously learned knowledge between input and output while acquiring
new knowledge?

They seem to take a lot of inspiration from neuroscience, which gives the paper
a more unique feel to it. That said, reading this, I don't quite see much that I
think can help the problem of catastrophic forgetting? Results are a bit limited
and I think the authors have moved on to other stuff, such as their Born Again
Neural Networks paper at ICML 2018. I also don't quite understand their
architecture, and I wish there was a diagram. They have a *Neocortex* network
and a *Hioopcampus* network and seems like the former is trained during
"development," the latter during "maturity."

Experiments are based on images (classification?) based on different viewpoints.

[See this discussion][1]

[1]:https://www.reddit.com/r/MachineLearning/comments/4ppam7/160602355_active_long_term_memory_networks/
