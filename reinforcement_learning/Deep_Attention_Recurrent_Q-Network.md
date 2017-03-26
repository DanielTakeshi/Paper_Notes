# Deep Attention Recurrent Q-Network

This looks pretty interesting. They augment normal DQN with attention models
(and recurrence!) to get **DARQN**. They cite the paper about [Deep Recurrent
Q-Learning][1] so their real contribution is the attention mechanism.  They have
an LSTM which is trained to focus on certain regions of pixel space, so this
adds the benefit of *interpretable* learning. The paper also mentions
computational speed-ups.

They use both soft attention and hard attention. With (hard) attention, the
weights of the neural network are adjusted so that attention locations which
result in higher reward are encouraged.

**Network Architecture**: the CNN part is similar to DQN, except that the input
is 84x84x1, so there is *no frame stacking*. The output of the CNN part consists
of a 256x7x7 set of feature maps, so the attention network (the next part)
takes in 49x256 input (49 vectors, each of length 256).

**Results**: rather surprisingly, DQN performs best on Breakout by far. That's
surprising, their new algorithms can't get anywhere near its performance?? For
some of the other games, their algorithms are better, but Table 1 is a mixed
bag of results.

Figures 4 and 5 are nice.

The results are pretty mixed. I'm surprised that they haven't tried improving
it, but maybe the papers which cite this one (see Google Scholar) do this.

[1]:https://github.com/DanielTakeshi/Paper_Notes/blob/master/reinforcement_learning/Deep_Recurrent_Q-Learning_for_Partially_Observable_MDPs.md
