# Learning to Teach With Dynamic Loss Functions

This is the extension of the "Learning to Teach" paper (ICLR 2018, also see my
blog post) where they use the teacher to teach **loss functions**. Cool!

Unlike the L2T paper, they use *reverse-mode differentiation* for this purpose.
So, I will need to learn how that works ... but already that is probably a good
sign because doing RL for the teacher (as in L2T) should be a last resort! We
should try and get more interpretable methods, rather than simply runnign RL
(and L2T didn't specify details of the RL environment, and they did not release
code...).
