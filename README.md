# Least Action & Path Integrals: Independent Summer Study at IISc

[github.com/Tushar-Hegde/LeastAction](https://github.com/Tushar-Hegde/LeastAction/LeastAction.ipynb)
[Colab notebook](https://colab.research.google.com/drive/1ycYvf4DuS0ek8jvvazjp-EafQA8qo7Pg?usp=sharing)
[Review Presentation](https://canva.link/jcv0xfedcyo2em2)
---

## 1. Summary

I spent the summer learning the path-integral formulation of quantum mechanics and the classical principle of least action, and pairing that theory with a small computational experiment: using gradient descent to find the trajectory of a particle by minimizing (and trying to maximize) the discretized action.

The optimization code was from an existing open-source PyTorch implementation (Sam Greydanus's tutorial, linked in the references) and modified it to run my own experiments.

During my study, several of the ideas I tried turned out to already exist in the literature, and several didn't work. However during the course of this I learnt the physics, tools and had some experience with how research is.

---

## 2. Physics background I studied

Worked through mainly from Feynman & Hibbs and Sakurai & Napolitano + three arXiv papers (see References).

**Foundations**
- Probability distributions in quantum mechanics
- Exclusive and interfering alternatives
- Quantum-mechanical amplitude

**Classical mechanics side**
- The classical action and Lagrangian mechanics: Maupertuis's action, the action postulate, derivation of the Euler–Lagrange equation

**Path-integral formulation**
- The sum over paths, the path integral, and its classical limit
- The free-particle path integral; momentum and energy
- Gaussian functions and integrals: pure quadratic, with a linear term, with imaginary exponent
- Events in succession (composition of amplitudes)
- Diffraction through a 1D slit
- Derivation/confirmation of the uncertainty principle in 1D
- Interaction of two particles through a potential field
- How the path integral propagates the wave function
- A first pass at the Schrödinger formulation: formalism, operators, and observation

**Computational tools picked up along the way**
- Python scientific stack: NumPy, SciPy, Awkward Array, and other libraries
- PyTorch and Keras
- ML concepts: linear regression, a little on neural networks and transformers

---

## 3. The computational project

### The idea
The principle of least action says the path a classical particle actually takes between two fixed endpoints is a stationary point of the action

S = ∫ L dt = ∫ (T − V) dt

Discretize time into N steps, treat the particle's position at each interior step as a free parameter (endpoints held fixed), and S becomes an ordinary differentiable function of those parameters. Autograd can then compute dS/dx at every point, so gradient descent can "relax" a guessed path towards the true trajectory.

### What I did
I took the existing PyTorch implementation and modified it to experiment:

- **Changed the physical system** by swapping in different Lagrangians: a free particle, a harmonic oscillator, and a more specific potential (hoping to get multistable states) , among others.
- **Tuned the optimizer.** I varied the number of iterations and the step size, and studied the balance between them. In my runs, small steps could stall convergence, while large steps gave inaccurate or noisy final paths (the review slides include plots of both well-converged and visibly noisy trajectories). I tuned largely by trial and error.
- **Pushed past minimization.** I flipped the sign of the objective to extremize the action to a maximum or an inflection point, not just a minimum.

### What I tried that already existed
Ideas I came up with and worked through, then learned had been done already:

- Computing the probability of a particle being at position *x* at time *t*, given its previous velocity
- Applying that to an existing probability distribution
- Using a single Lagrangian for a system of multiple particles
- A few smaller things along similar lines

These were still worth doing, since re-deriving them independently was a good check that I understood the material.

### What didn't work
- **Arrival probability over a time window:** trying to compute the probability that a particle arrives at point *x* at any time within a window. (Was not able to normalise over time)
- **Maximizing the action to get a stable path:** gradient ascent on the action does not converge to something physically meaningful. I got my particly to oscillate to infinity and back basically.
- **A "maximum slope" criterion:** instead of extremizing, I tried using a maximum-slope condition to find regions where the action changes very little. This didn't yield useful paths.
- Some other attempts I no longer remember in detail.

---

## 4. What I took away

- **All interpretations are equally good if they agree with experiment.** Seeing the same physics through Lagrangian, path-integral, and Schrödinger formulations made this concrete.
- **In research there is always something more to do or learn.** Every answer opened a new question, and failed experiments were still informative.
- The experience has shaped how I think about science and research, which is my main interest and will shape my future direction.

---

## 5. What I'd do next

- Write a from-scratch implementation so the whole pipeline is mine, rather than a modified tutorial.
- Compare the gradient-descent trajectory quantitatively against the analytic Euler–Lagrange solution (e.g., error vs. step size and iteration count) instead of judging by eye.
- Try a better-conditioned optimizer (e.g., L-BFGS or Adam with scheduling) to remove the step-size trial and error.
- Move from the classical action to a Monte Carlo evaluation of the actual path integral for a simple potential.

---

## 6. References

- R. P. Feynman and A. R. Hibbs, *Quantum Mechanics and Path Integrals*
- J. J. Sakurai and J. Napolitano, *Modern Quantum Mechanics*
- arXiv:1308.2022 · arXiv:1712.08508 · arXiv:2011.08188
- Base implementation and tutorial: <https://greydanus.github.io/2023/03/05/ncf-tutorial/>
- My modified code (Colab): <https://colab.research.google.com/drive/1ycYvf4DuS0ek8jvvazjp-EafQA8qo7Pg?usp=sharing>
