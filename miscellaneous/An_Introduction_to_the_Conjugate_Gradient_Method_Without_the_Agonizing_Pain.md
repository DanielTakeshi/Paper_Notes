# An Introduction to the Conjugate Gradient Method Without the Agonizing Pain

I've known of this paper for several years, but until I learned more about the
Trust Region Policy Optimization (TRPO) algorithm, I didn't feel the need to
know about the conjugate gradient method. TRPO uses CG so that it can avoid
computing a matrix and instead implicitly compute it via a bunch of
matrix-vector products, which are vectors since (n,n).dot(n,1) results in a
tensor of shape (n,1).


## Idea and Intuition of Conjugate Gradient

The CG method solves Ax=b. This means finding x given A and b. It is *iterative*
so it involves multiple steps.


## Technical Details

TODO


## Final Thoughts

I quite enjoy reading Shewchuk's writing, when it isn't in standard academic
form. I wish more papers like this existed for other technical algorithms.

Progress:

- Read Chapters 1 through 3, skimmed Chapters 4 and 5 (didn't do math yet).
- No exercises attempted.

Things I especially like:


- Figure 5. It shows visualizations of quadratic forms which depend on the
  definiteness quality of A. The other terms involving b and c just shift the
  contour around.

- Equation 8. It shows why x = A^{-1}b is the minimizer of CG, assuming
  invertibility (though this is implied because in the general setting of
  quadratic forms that the paper considers, A is symmetric positive-definite).
