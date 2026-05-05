# Day 1 — FGSM paper notes

**Paper:** Goodfellow, Shlens, Szegedy. *Explaining and Harnessing Adversarial Examples.* ICLR 2015. [arXiv:1412.6572](https://arxiv.org/abs/1412.6572)

## One-line summary
A tiny, structured perturbation — invisible to humans — can flip a neural network's prediction with high confidence. FGSM is the simplest way to construct one.

## FGSM in three lines

$$x_{\text{adv}} = x + \epsilon \cdot \text{sign}(\nabla_x J(\theta, x, y))$$

- $x$: input
- $\nabla_x J$: gradient of the loss w.r.t. the input (where moving $x$ increases the loss fastest)
- $\epsilon$: per-pixel budget under an $L_\infty$ constraint
- $\text{sign}(\cdot)$: keep only direction, throw away magnitude

## Why sign() and not the raw gradient?
Took me a minute. The attack lives under an $L_\infty$ budget — each pixel can change by at most $\epsilon$. The gradient tells you direction, but its magnitudes vary wildly per pixel: some pixels would barely move, others would explode. By taking the sign, every pixel uses its full $\epsilon$ budget in the loss-increasing direction. So FGSM is the **optimal one-step perturbation under a linear approximation of the loss**, given an $L_\infty$ ball. It's not "use the gradient" — it's "use the budget."

## The linearity hypothesis
The thing that actually made me update.

The pre-2014 explanation for adversarial examples was that NNs are *too non-linear* — they have weird overfitting pockets where tiny moves cross decision boundaries. Goodfellow flips it: the problem is that NNs are *too linear* in high-dimensional input space. A small per-pixel change $\eta$ produces a change of $w^\top \eta$ in pre-activation. Even if each pixel moves by tiny $\epsilon$, summed over thousands of dimensions you get a huge dot product. So adversarial examples aren't bugs in the non-linear regions — they're features of high-dimensional linear behavior.

Why this matters: it predicts that even *linear* models have adversarial examples (they do), and that adversarial examples should *transfer* across architectures (they do). The old non-linearity story explained neither.

## What surprised me
- FGSM was the first adversarial attack I implemented myself. Seeing a perturbation that's literally invisible to me flip the model's prediction with high confidence was a different feeling than reading about it. Made the threat model concrete.
- CIFAR-10 (3-channel) felt noticeably more vulnerable than MNIST (1-channel grayscale) at the same $\epsilon$. I think this lines up with the linearity argument — more input dimensions = larger $w^\top \eta$ for the same per-pixel budget — but I want to verify this empirically in the next few days.

## Still unclear
- Is $\epsilon = 0.007$ on MNIST in the paper a calibrated number (e.g. below JPEG quantization, below 8-bit pixel resolution) or just a value that happened to work? Worth checking.
- The paper says adversarial training with FGSM helps but PGD-trained models are stronger. Why exactly does iterative attack training generalize better than one-step? Coming back to this in the PGD note.

## Connection to my work
FGSM is test-time evasion: attacker has white-box access at inference. My CARLA backdoor work is train-time poisoning: attacker corrupts the training data and the model carries the vulnerability into deployment. Same adversary mindset, different access window. Worth keeping the threat-model distinction sharp as I move through the sprint.