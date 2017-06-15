# Deep Recurrent Q-Learning for Partially Observable MDPs

General idea: we take standard DQN and replace the first fully connected layer
(which comes after the first 3 convolutional layers IIRC) with an LSTM. This
*Deep Recurrent Q-Network* (DRQN) thus has *memory* and can work with POMDPs,
where information from Atari 2600 games is removed from the agent. This is
important because normal DQN can only have a memory of the past four non-skipped
frames, which is extremely limiting.

**Update**: after re-reading this paper with some more LSTM experience, it now
makes sense to just put one 84x84 frame as input to the LSTM. That's OK. I may
have not had the right intuition before.


## Algorithm and Experiments

The paper goes through the definitions of Q-learning, blah blah blah, DQN, blah
blah blah, DQN has no explicit mechanism for POMDPs (good point they make here),
then the interesting part is about the DRQN architecture:

> To isolate the effects of recurrency, we minimally modify the architecture of
> DQN, replacing only its first fully connected layer with a recurrent LSTM
> layer of the same size.

Yes, this is what I was hoping to see, *minimal* adjustment, to make comparisons
fair. **But the DRQN takes in *one* 84x84 frame. Shouldn't it take four of those
like DQN?!?** UPDATE: Whew, they address why they didn't do this in the appendix:

> Unfortunately, results show that the additional parameters do not lead to
> increased performance on the set of games examined. It is possible that the
> network has too many parameters and is prone to overfitting the training
> experiences it has seen.

All right, but it still seems unfair to not use the exact same input to the
architectures. If the performance of this is *the same* as the one-frame DRQN,
then they probably should have used the four-frame DRQN.

Training DRQNs: sampling the states must be done carefully due to intricacies of
training LSTMs. They sample randomly but have to "zero out" LSTMs. I'm not sure
why that's needed; I better investigate more. They do *not* use policy
gradients, which are probably the standard way we train LSTMs (I think, not
totally sure).

To make it POMDPs: just flicker the screen! =) With probability 1/2, the screen
vanishes, else it's normal. This will make games POMDP since velocity of
particles is more obscured. DQN with *ten* frames can detect velocity, but so
can DRQN with *one* frame, due to the LSTM!

Experiments:

- **Experiment 1**: replicating four-frame DQN's performance with no flickering.
  Yes, performance should be (and is) roughly equal.
- **Experiment 2**: train on *partially* observed data, evaluate on *fully*
  observed data. I'm not sure where they describe this. Figure 3? They never
  justify their assertion that "performance improves when the training is done
  on more observed data."
- **Experiment 3**: train on *fully* observed data, evaluate on *partially*
  observed data. They find that DRQN's performance degrades less ("more
  gracefully"), as expected. See Figure 5 for results. It makes sense to me.
  This is the "opposite direction" of Experiment 2.

Huh, they mention this:

> This observation leads us to conclude that while recurrency is a viable method
> for handling state observations, it confers no systematic benefit compared to
> stacking the observations in the input layer of a convolutional network.

Yeah, maybe recurrency isn't so good? =(


## My Thoughts and Takeaways

I was surprised I missed this paper when I was looking for DQN extensions.
Maybe since it's not from DeepMind it isn't as well-known.  Fortunately, I've
found it now and it seems like a legitimate variant of DQN, and in retrospect,
someone should have thought of this anyway.

The paper makes a good point that even if we take the last four non-skipped
frames, that is *still* a partially observable state, and DQN's performance will
only be as good as how those frames are representative of the game's true state.
**EDIT**: never mind! Of the games reported by DeepMind, actually, *all* of them
(according to the authors of this paper) are NOT POMDPs when we take the last
four frames. This is because any game that has motion means we see the full
game. There are still games that are POMDPs even with four consecutive frames,
but those apparently were not reported or were not supported by the emulator.

I'm still annoyed by how they used one 84x84 frame instead of taking the last
four. How many parameters does an LSTM add to a net anyway? But hey, maybe it
shows that DRQNs should be used instead of DQNs?

I wish I could do something like this. =(

But also, I'm not sure if this is that good compared to just stacking more
frames (say 10 instead of 4).
