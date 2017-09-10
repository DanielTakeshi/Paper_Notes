# \#Exploration: A Study of Count-Based Exploration for Deep Reinforcement Learning

Note: I read the one under review at ICLR. I'm hoping to read the newest version
later.

General idea: try to generalize count-based exploration (i.e. which keep track of states through hash codes) to high dimensional state spaces through hash codes. They also identify what makes a good hash code for the deep reinforcement learning setting. This simple idea leads to surprisingly good performance on Atari 2600 games.

## Introductory Stuff

Count-based exploration is when in RL, we explicitly count the states as they happen, i.e. in standard Q-learning, we keep track of the (s,a) pairings we've encountered (through a table) to learn the Q(s,a) function. This is not feasible in the high dimensional setting, which is why we use linear or (more commonly now) neural network function approximation. Their intriguing idea is to mimic this function approximation by instead discretizing the state space with a hash table, then using (s,a) as the hash table keys. They can further fine-tune the hash table/function so that it appropriately balances generalization across states. They claim:

> The main strength of the presented approach is that it is fast, flexible and complementary to most existing RL algorithms.

This is a bit vague but I guess it's nice, I'm liking it so far.

Notation:

- Use the standard finite, discounted horizon MDP with rewards, good.
- Use \phi to map from states to an integer value (i.e. the hash value).
- R^+(s,a) = \beta / \sqrt{n(\phi(s))} is an exploration bonus to the reward, with \beta a coefficient and n(...) indicating the counts (**after** applying the hash function). They do NOT use actions as input to the 'n' function. By the way, their algorithm can use "any RL algorithm" normally, it's just that the R(s,a) = R(s,a) + R^+(s,a). In the experiments, they use TRPO.

As anyone who has taken CS 61B at Berkeley knows, the clear challenge is to make the hash function map similar states to similar codes and distinct states to different codes. For this, they use **Locality-Sensitive Hashing** (LSH), see next section of notes.


## Algorithm

See Algorithm 1 in the paper. They use a previous method for hashing states, "SimHash," which relies on a (k,d)-dimensional matrix A. Increasing k means increasing granularity. Even before that, though, they use a preprocessing function since applying directly on (say) raw pixels is not effective. ([I wrote a little bit about preprocessing][1].) They propose two ways of processing:

- [Hand-Designed] **BASS** (Basic Abstraction of the ScreenShots). A basic method which is specific to Atari games and partitions pixels based on intensity.
- [Learning-Based] **Learned Embedding**. Instead, try to learn domain-dependent features so that we can extract the most useful features. For this, they use **autoendoders**. When am I ever going to get comfortable with these? They emphasize the need to round values to 0/1 to turn the states into binary codes. By the way, their autoencoder uses sigmoids, and they *deliberately want the saturating behavior of sigmoids*.

Note that both methods require states to be in **binary codes**, so states must be a vector of all 0s and 1s.


## Experiments

First question:

> Can count-based exploration through hashing improve performance significantly across different domains? How does the proposed method compare to the current state of the art in exploration for deep RL?

They test using RLLab and Atari 2600 games. They use different preprocessing for images, answering the second question:

> What is the impact of state preprocessing on the overall performance of image inputs?

Another question:

> What factors contribute to good performance, e.g., what is the appropriate level of granularity of the hash function?

They use TRPO for all experimental runs.

Figure 2: seriously? Why did they use these awful colors? I cannot tell the difference between VIME and SimHash even after zooming in quite a bit. There's a similar issue with Figure 3. By the way, Figure 1 is unnecessary.

Also, is the TRPO baseline really that bad (in Section 3.1)? I might want to check out that RLLab paper if they have results there ...

Section 3.2: they use six Atari 2600 games, and compare with a lot of algorithms: double DQN, double DQN w/pseudo-counts, dueling networks, A3C+, Gorila, and DQN Pop-Art. Wow. PS: the double DQN w/pseudo-counts does well on Montezuma's Revenge! I don't actually know all of these extensions. Maybe I need to record them together in a blog post.

Section 3.3: the granularity parameter k obviously needs to be tuned appropriately.

Section 3.4 investigates Montezuma's Revenge in more detail. They must have done a lot of experimentation and "just trying things out" as Andrew Ng and Ken Goldberg would advise. They get *high scores*, 5661, but it's not state of the art, according to them. What *is* the state of the art?

The most related work is a NIPS 2016 paper by Bellemare et al., which I should probably check out at some point.


## My Thoughts and Takeaways

Nice simple idea, interesting results, figures unreadable. It will be interesting to see if it gets accepted into ICLR 2017.

I guess another takeaway is to ask myself if I can use hash tables in some way for my work. They're pretty common.


[1]:https://danieltakeshi.github.io/2016/11/25/frame-skipping-and-preprocessing-for-deep-q-networks-on-atari-2600-games/
