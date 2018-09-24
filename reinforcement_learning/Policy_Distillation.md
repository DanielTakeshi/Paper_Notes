# Policy Distillation

https://danieltakeshi.github.io/2018/06/08/papers-that-have-cited-pd/

Easy-to-read paper on an important topic, using distillation (teacher ->
student) to train students, for better regularization.

As expected, the student is trained to match the q-values of the targets. The
authors use three loss functions: MSE on q-values (bad), NLL on the action w/max
q-value (also bad), and KL divergence on the softmax(q-values/T) with sharpened
distributions, as they use T=0.01 as the temperature. The KL divergence one
works the best.

They show experiments that identical students do better than their teachers,
which is the same claim argued in "Born Again Neural Networks" for ICML 2018.
The BANN paper really should have mentioned that this paper *already* did the
same identical architecture setting, and for a harder, *sequential* setting than
CIFAR-10 classification?? (BANN cites this paper, but doesn't go in depth on
identical architectures.)

Other experiments involve multi-game DQN (which I have some experience with)
showing that a single student (w/separate output "branches" for different games)
can learn from 10 separately-trained teacher networks each with their own Atari
game, and that this out-performs just training one large DQN (w/separate game
output branches as usual) on all the games together.  They also show brief
evidence for the benefits of *online policy distillation*.
