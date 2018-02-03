# A Note on the Evaluation of Generative Models

This is a relatively easy to read paper which mainly consists of going through
five different evaluation metrics for generative models, and showing caveats to
each.

The authors conclude that one must be, well, very careful when evaluating
generative models.

- Average log likelihood: the default one, basically maximizing p(x | theta) or
  minimizing its negative (I usually think of minimizing the negative since
  that's what we put in TensorFlow lol). But be careful because images are
  technically discrete as they're stored with 8-bit integers, so we should
  inject a small amount of noise if we view the numbers as continuous (and thus
  modeling them as densities).

- Samples and log likelihood: they argue that log likelihood and visual
  appearance of samples are independent.

- Samples and applications: interesting demonstration that (mixture) *models*
  can generate poor samples 99% of the time, but for class labels, the
  *posterior over the label* is another mixture model, but one where all the
  mass lies in the other 1% of the original model which generates good samples.
  (TODO: will have to look at this again...)

- Nearest neighbors: ah, this I could tell in advance. And yes there are many
  problems with using Euclidean distance as a metric of similarity.

- Parzen windows: I think I studied these before but I don't remember, and quite
  frankly these seem very specialized to computer vision.

I thought this paper was interesting, though some of the details weren't easy to
process since I don't do research in this area.
