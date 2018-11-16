# Stochastic Adversarial Video Prediction

A video prediction paper. Given a series of frames, try to predict the next few
ones. It's hard due to multimodality (naive solutions will just average pixels
together, leading to blurriness), the super-exponential set of possibilities.

Key idea: combine benefits of VAEs and GANs, while avoiding downsides
(blurriness for the former, mode collapse for the latter). Sounds cool, I wonder
if there is a nice way to quantify the quality of the video. Of course this is
the major challenge of any generative model. And with video prediction we don't
know if the results were hand-selected or whatever. Figure 3 shows that there
are some interesting differences among the methods where the generator will
predict frames that *blur out* some of the objects touched by the robot arm.

Their method is applied to two datasets, one from the CoRL 2017 paper, and
another from an older one which I'm not familiar with.

I have not gone through these other papers in detail, but it would be nice to
really understand the difference between this paper and the one from Finn (NIPS
2016) and Ebert (CoRL 2017).
