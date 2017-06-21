# Deterministic Policy Gradient Algorithms

**Main idea**: this paper is considered the *precursor* to the **Deep
Deterministic Policy Gradient** paper (from DeepMind, published at ICLR 2016)
since it doesn't focus on neural network function approximators.

**Important questions**: 

- What's the difference between DPG and VPG, "vanilla" policy gradients? 

- Is this an actor-critic method? Is this off-policy?

- What are the main contributions?

**My answers**:

- The difference seems to be whether the policy is stochastic. In VPG, the
  policy is stochastic because in the discrete case, we put a softmax over
  actions, and in the continuous case, we output the mean of a Gaussian and then
  sample from that. So either way, the action is not determined from the policy.

  Additional thoughts/questions:
 
  - DDPG is considered the successor to DPG, but DDPG can also be applied to
    stochastic policies during training, right)

  - But what about this: in VPG, we can train with a stochastic policy. I agree,
    but then what about actual test-time deployment? For that, shouldn't the
    mean of the Gaussian be simply the action? Should we be sampling from a
    Gaussian during test time as well? I am not sure ... because the only reason
    why we have that Gaussian at the beginning is so that we get a continuous
    function which gives us a gradient defined almost everywhere.

- Yes! They answer that in the abstract, actually. 

  - The reason why it needs to be off policy is a bit subtle: a deterministic
    policy intuitively means the set of actions we pick is less diverse (covers
    less of the action space) than if we had stochastic policies. For instance,
    always picking the mean of a Gaussian means that we may never get to the
    "extreme regions" in a Gaussian and those may actually be pretty good, we
    just won't transition there until a long time has passed.

  - This is kind of like why Q-Learning has been useful, it's off-policy and
    follows a noisy version of a deterministic policy, that of taking the
    reward-maximizing action in a given state.

- Main contribution is showing how to compute the gradient of a policy
  parameterized by \theta *when that policy is deterministic*, i.e. given state
  s we know exactly the action a.
  
  - They argue convincingly that this is useful because, as we know, policy
    gradients have high variance, and for stochastic policies, as they become
    more deterministic, their variance actually increases since it scales with
    1/\sigma^2 where \sigma^2 is the variance.

**Experiments**: Unfortunately, it's hard to get intuition of these since they
are specialized problems. For now just assume the experiments in the DDPG paper
since MuJoCo has become standardized ... but we really need harder RL problems.
Maybe that DeepMind Labyrinth works?

**Final Thought**: There are lots of classical RL papers referenced here, such
as the 1992 paper which introduced REINFORCE. I better read those! Also, I might
consider going through the math here ... because this set of notes is extremely
high level. 
