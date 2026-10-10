# markov_trajectory_mixer-qt

A mixing desk for the denoising trajectory. Every intermediate step as a first-class output; process and recombine them like audio tracks.

## The name

A diffusion generation is a Markov chain: each step depends only on the previous one, all the way from noise to image. Markov, 1906. Old math, correct math, and nobody skimming GitHub reads "AI" in it.

## The idea

Standard sampling discards every intermediate step — only the final image survives. But each step produces a *predicted clean image*: a coarse-to-fine sequence of N increasingly refined predictions. We throw away N-1 of them.

This treats every step's predicted-clean as a track on a mixing desk. The trajectory is the tape; the steps are the tracks. The step-visualizer plugins are multitrack viewers with no faders. This adds the faders.

## The desk

- **Crossover by step range** — early steps carry low frequencies (composition, big shapes); late steps carry high frequencies (texture, detail). Split the trajectory into bands and process them separately. The process is already frequency-separated across time; this works with the grain.
- **EQ per step** — frequency filtering of individual steps' predicted-clean images.
- **Invert + mix (residual bus)** — subtracting one step's prediction from another isolates the *residual*: literally what the model decided at that stage. Parallel processing for denoising.
- **Cross-modulation** — multiply a coarse prediction by a fine one. Nonlinear interference of structure and texture. Gnarly on purpose.
- **Feedback** — take a processed prediction, re-noise it, keep denoising (SDEdit-style). Push too far off-manifold and it artifacts. That's a patch note, not a bug report.
- **Long-exposure mode** — average predicted-clean across steps: a long-exposure photograph of the generation thinking. Softer, dreamier, lower variance.
- **BPM sync** — beat-clocked parameter modulation for animated trajectories. The VJ rig.

## The frame

You're not describing a tweak to image generation. You're describing a new medium where the *performance* is the denoising itself.

## The `-qt` suffix

Native Qt frontend, per the goose-qt convention: the native shell is the correct tool for a workstation. No browser jank.

## Status

Spec stage. Bare bones. One instrument in the righteous dub arc. Companion to annealed_langevin_dynamics-stepsequencer-qt.
