# Automated Curriculum Learning for Neural Networks

The details are eluding me a bit, but the general gist is to use a multi-armed
bandit to generate a stochastic curricula, based on the amount the network
learns from each data sample (the more it learns, the higher the reward).
