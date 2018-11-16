# One Model to Learn Them All

OK, a somewhat clickbaity title but the TL;DR of the paper is the following:

> Can we create a unified deep learning model to solve tasks across multiple domains?

They use different *modalities* in the network, which process the input and then
later, they can share parameters.

This model doesn't do as well as the state of the art (at least, as of the
summer 2017 publication date) but I don't think it's fair to expect state of the
art performance.

I wonder how much of a connection we can use with this and Policy Distillation?
Well, in PD, they used one model for multiple games, and the games are among
similar modalities. I wonder if we can do one modality as Atari, the other
modality as Retro games?
