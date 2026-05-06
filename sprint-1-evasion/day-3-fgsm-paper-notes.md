# Day 3 — FGSM aha moments

Two things clicked while implementing FGSM that didn't click from reading the paper alone.

## Aha #1: sign() and the `+` are physical, not symbolic

I was familiar with FGSM before this sprint — I had read about it, I knew the equation. But I had never actually implemented it. Coding it was different.

Two things became concrete only after typing them into PyTorch:

**Why `sign()`.** On paper it's a one-line operation. In code, when I printed the raw gradient and then `sign(gradient)`, I saw the gradient values were tiny and varied wildly across pixels — some pixels would barely move under a fixed budget, others would dominate. The `sign()` step throws that variance away and pushes every pixel by exactly $\epsilon$ in the loss-increasing direction. That's when the "use the budget, not the gradient" framing from day 1 stopped being a sentence I had written and became something I had seen.

**Why `+` and not `-`.** Trivial in retrospect, but I had to stop and think about it the first time. Training does $x \leftarrow x - \eta \nabla_\theta J$ to *decrease* loss. FGSM does $x \leftarrow x + \epsilon \cdot \text{sign}(\nabla_x J)$ to *increase* loss. Same machinery, opposite direction. Gradient descent on the input, but adversarial — the model is fixed, the input moves. Once I wrote the line of code I won't forget the sign of the sign again.

## Aha #2: CIFAR-10 was more vulnerable than MNIST, and that's the linearity hypothesis showing up

I expected MNIST to be easier to attack. The intuition was lazy: MNIST images are simpler, models hit 99%+ accuracy, so surely a small perturbation flips them. CIFAR-10 has more complex features, so it should be more "robust" in some hand-wavy sense.

Wrong. At the same $\epsilon$, CIFAR-10 collapsed harder than MNIST.

After sitting with it, I think this is the linearity hypothesis from the paper actually showing up in my own results. MNIST is 28×28×1 = 784 dimensions. CIFAR-10 is 32×32×3 = 3072 dimensions, almost 4× more. Goodfellow's argument is that adversarial examples come from $w^\top \eta$ summed over input dimensions — same per-pixel $\epsilon$, more dimensions, larger total dot product, larger pre-activation shift, easier flip. So vulnerability should *grow with input dimensionality*, which is what I saw.

This is the moment the linearity hypothesis went from "interesting argument I read" to "thing I just empirically observed in my own code." That's a different kind of belief than the day-1 belief.

