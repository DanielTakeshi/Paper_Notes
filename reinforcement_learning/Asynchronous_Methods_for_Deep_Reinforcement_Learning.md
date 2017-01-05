# Asynchronous Methods for Deep Reinforcement Learning

General idea: 

Remember that, though the best known aspect of this paper is the **A3C algorithm**, this paper is really about *four* algorithms, all of which are asynchronous variants of other algorithms.


## Introductory Material

TODO


## The Algorithm

TODO


## Experiments

TODO


## My Thoughts and Takeaways

At the time it was published (ICML 2016, though it was on arXiv for a few months earlier), I believe it had the best results on the Atari 2600 games. It's a bit tough to say because that requires going through the whole set of games and eyeballing the numbers but I trust the paper's claims.

This is probably one of my favorite papers that I've read. I find it hard to read research papers, but this one was actually not that bad for me, especially the 8-page non-supplementary material content. I can understand and follow the n-step methods and SARSA. It also doesn't seem hard to replicate a simpler version of asynchronous methods for DQN which we created for the CS 294-129 class at Berkeley. It's not truly parallel but it simulates that by running different game environments sequentially (*if* it were easy to write parallel code we would just do that). The harder part might be A3C itself, though.

Great work!!

[IN PROGRESS] I am going to implement these algorithms using TensorFlow. Or, more accurately, just "base it" off of Denny Britz's code.
