# Multilateral Surgical Pattern Cutting in 2D Orthotropic Gauze with Deep Reinforcement Learning Policies for Tensioning

(That's a mouthful of a title.) The main idea in this paper is to design policies for surgical cutting robots that rely on *tensioning*. The surgery task uses two tools, (1) a grasping tool and (2) a surgical cutting tool (i.e. scissors), and the robot's objective is to use those tools to successfully cut a pattern (typically a circle) on a small sheet of surgical gauze. I know from watching the lab videos that the gauze is suspended at the four corners.

Intiution: it is challenging to cut through the gauze to accurately match the target pattern on it. One way to improve this is to use *tensioning*, where the grasping tool can grab part of the gauze to make cutting easier. Analogy: think of cutting a soft fabric. Just cutting it normally like we would for paper might not work since the scissors don't have the appropriate force to slice through.

So that's the tool/setting, but how does the robot *perform* the cutting? We use ... **deep reinforcement learning**! The deep RL algorithm uses **Trust Region Policy Optimization** (I'll have a paper summary and blog post on that "soon," I promise) to learn *tensioning* specifically. AKA this is not Learning from Demonstrations. TRPO is the default for continuous control tasks, such as what we have here (the RL learns a policy for the robot *controls*). In addition, as it is costly to collect data on this, the robot needs to use a **finite element fabric simulator** to simulate datapoints; performance therefore depends on the accuracy of this simulator. It's described in Section IV but there's not much I can say/critique about it given my lack of simulating experience and limited physics knowledge. The simulator models the fabric using a rectangular mesh of vertices (i.e. grid).

More on the cutting: the scissors actually follow a *pre-planned* route, while the tensioning is done by providing the robot with tensioning directions, which must be what the learned policy will do. The **reward** is the symmetric difference between desired contour (provided as input) and actual cut contour (this makes sense). It's also the evaluation metric so this means the reward is perfectly ideal.


## The Policy/Algorithm:

They call it **Tensioning Policy Search**, see Algorithm 1. Here are its main points:

- Given a closed/open curve (non-intersecting, fortunately), it has to *segment* the curves into pieces. This might seem weird but it's due to a limitation in the dVRK robot since it can't cut continuously without having arms collide. I get it. See Figure 5. The segmentation is going to find them by "local minima and maxima of the curves" as the paper says, but I don't know what they mean by that.
- They sample 8-12 "fudicial" points. These are the boundaries between the segments (see previous point).
- Not totally clear on the actions formulation in the MDP. But otherwise it's clear.
- Not clear on the neural network architecture, but it's fully connected.
- It has to plan a cutting trajectory *before* the tensioning. I think this is specified in their `heuristic_search` method (Section VI-B) but is dVRK really allowed to switch directions and skip cuts, to go back to them later? Perhaps that has to do with the limitations of itself (see Figure 5 again).
- Then it can select grasp points for tensions. The grasp point candidates are selected *by sampling* ... OK I think that makes sense.

Then here's what's interesting: we have a set segmented curves and sampled candidates. We find policies for each of these. Then at the very end, we take whichever one works best. *Random search* basically, because the operations are not differentiable. Maybe that could change? ;)


## Experiments

Three major ones:

- Simulation, 17 open/closed curves, compare with three others: (1) no tension, (2) fixed, and (3) analytic.
- Simulation again, this time only for their algorithm but gauge noise robustness.
- Then 4 curves on a *real* **da Vinci Research Kit (DVRK)**. It's supposed to be relatively complicated to use. :(

Lots of detail on these! Table I shows extensive results for the first of the experiments above, and fortunately, also has images of the curves. Yay! There are a few failure cases but I suppose there is not enough room in the paper to describe it.

The robustness: yeah, first and second make sense, former decreases resolution of the grid, and the latter adds noise to a bunch of stuff (always an option). The modeling error is a bit more mysterious.

Real robot: not much to say here, the section is a bit shorter and still shows their algorithm is better **BUT** it's better in 3 out of 4. That's a very small sample size, unfortunately. Also, they can't perform two of the baselines.


## My Thoughts and Takeaways

There's lots of depth in the experiments, but not so much breadth. The real robot examples are impressive and ahs clear applications. The downside is that there are many assumptions one has to make, and reproducibility is difficult to impossible due to the highly-specialized nature of this research.

The cutting is also planned out beforehand. Is it possible to learn the cuts as we go, along with the tensioning? Or to learn about any other thing they did here using brute force? Can we make something differentiable?

I suggested trying out CNNs when I first reviewed this.
