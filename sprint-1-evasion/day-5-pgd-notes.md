# Day 5 — PGD notes

I jumped to implementation before reading the Madry paper carefully — short notes online, then code. Paper-level notes coming in a separate file once I sit with the saddle-point formulation properly. This file is what implementing PGD taught me.

## The one-line realization

PGD is multi-step FGSM with three additions: a smaller per-step size, a projection back into the constraint set after each step, and a random start inside the ε-ball.

```python
# FGSM (day 2):
x_adv = x + eps * sign(grad)

# PGD (day 5):
delta = uniform(-eps, eps)                  # random start
for _ in range(T):
    delta = delta + alpha * sign(grad)      # FGSM-style step
    delta = clamp(delta, -eps, eps)         # project to ε-ball
    delta = clamp(x + delta, 0, 1) - x      # project to valid image
x_adv = x + delta
```

That's the whole algorithm. The single inner step *is* FGSM. PGD is what you get when you stop trusting the linear approximation and re-evaluate the gradient many times.

## Why iterating helps

FGSM takes one ε-sized step in the sign-of-gradient direction. That gradient is a *linear approximation* of the loss valid only locally — once you've moved by ε, the linearization may no longer hold. The gradient at $x_\text{adv}$ might point somewhere completely different from the gradient at $x$.

PGD takes many small α-sized steps (α < ε) and re-computes the gradient at every step. So PGD follows the actual loss surface; FGSM follows one tangent line and hopes. This is why PGD finds stronger adversarial examples than FGSM at the same ε budget — same constraint, better solver.

## Things I'd skim past in someone else's code, but had to think about in mine

Three implementation choices that look like boilerplate but aren't:

**Parameterizing `delta` instead of `x_adv`.** I keep `delta` as the first-class variable and compute `x + delta` only when feeding the model. Cleaner: the constraint $\|\delta\|_\infty \le \epsilon$ becomes a constraint on a single tensor, not a relationship between two. Projection is just `clamp(delta, -eps, eps)`.

**Two projections, not one.** Each step projects onto two sets — the ε-ball *and* the valid image cube $[0,1]$:

```python
delta = torch.clamp(delta, -epsilon, epsilon)         # ε-ball
delta = torch.clamp(x + delta, 0, 1) - x              # valid pixel range
```

The feasible region for the attack is the *intersection* of these two. Forgetting the second one produces "adversarial examples" that are mathematically valid but not real images. The same thing applies to random start: I clamp the random init through both projections before the loop, otherwise step 0 is already infeasible.

**Detach and re-attach each step.** `delta.detach() + alpha * grad.sign()`, then `delta.requires_grad_(True)` again. Without `detach()` the computation graph grows across iterations. This is also the pattern-level point that PGD steps don't backprop through each other — each step takes the gradient at the *current* point as a one-shot signal and uses it.

## What I haven't yet built intuition for

- **Hyperparameters T and α.** I used T=10 and α=ε/4 because that's the standard. No principled reason yet. Want to sweep T ∈ {1, 5, 10, 20, 40} on the same model and see when attack success plateaus.
- **Random start ablation.** I have `random_start=True` as default but I haven't measured how much it actually matters on my model. Madry's claim is that restarts help approximate the true inner max. I'd like to see this empirically: PGD-with-restarts vs. PGD-from-x on adversarially-trained vs. undefended models. Predicted: bigger gap on the defended model.
- **The saddle-point reframe.** I keep seeing PGD-AT described as solving $\min_\theta \mathbb{E}[\max_\delta L(\theta, x+\delta, y)]$. PGD is "the inner max solver." FGSM is "the cheap inner max solver." That reframing — FGSM and PGD as two solvers for the same problem rather than two separate attacks — hasn't fully landed yet. Coming in the Madry paper note.

## Connection to my work

PGD-based adversarial training is the standard robustness baseline outside FL. In federated learning, running PGD-AT on each client is expensive (T forward+backward passes per training example) and changes the gradient signal that gets uploaded — potentially leaking more about local data than vanilla training. This is one reason trust-aware FL methods, including my COMPSAC line, tend to use representation-level signals (clustering, attention over representations) rather than per-client adversarial training.