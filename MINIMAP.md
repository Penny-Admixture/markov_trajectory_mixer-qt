# MINIMAP — markov_trajectory_mixer-qt (hand this + the relevant source files to any model)

## What this is
A mixing desk for the denoising trajectory of a diffusion model. Every
intermediate step as a first-class output; process and recombine them
like audio tracks. A Qt music tool, not a game. One instrument in the
righteous dub arc. Companion to
annealed_langevin_dynamics-stepsequencer-qt (the step sequencer
programs the trajectory; this desk mixes it).

The name is old math and camouflage at once: a diffusion generation is
a Markov chain — each step depends only on the previous one, all the
way from noise to image (Markov, 1906). Correct math, and nobody
skimming GitHub reads "AI" in it.

## Current state (as found on main) — SPEC STAGE, NO CODE
- **The repo contains exactly one file: `README.md`.** No src/, no
  build files, no run.bat, no GOALS.md, no docs/. There is nothing to
  build or run. The README is the whole project so far: "Spec stage.
  Bare bones."
- No build number exists. Do not claim one.
- The first real work is Build 1 (the skeleton app), following the
  house Qt pattern of the sibling projects.

## The idea (from the README — the spec, not implemented code)
Standard sampling discards every intermediate step — only the final
image survives. But each step produces a *predicted clean image*: a
coarse-to-fine sequence of N increasingly refined predictions. We throw
away N-1 of them.

This treats every step's predicted-clean as a track on a mixing desk.
The trajectory is the tape; the steps are the tracks. The
step-visualizer plugins are multitrack viewers with no faders. This
adds the faders.

The frame: you're not describing a tweak to image generation. You're
describing a new medium where the *performance* is the denoising
itself.

## The desk (from the README — the spec)
- **Crossover by step range** — early steps carry low frequencies
  (composition, big shapes); late steps carry high frequencies
  (texture, detail). Split the trajectory into bands and process
  them separately. The process is already frequency-separated across
  time; this works with the grain.
- **EQ per step** — frequency filtering of individual steps'
  predicted-clean images.
- **Invert + mix (residual bus)** — subtracting one step's prediction
  from another isolates the *residual*: literally what the model
  decided at that stage. Parallel processing for denoising.
- **Cross-modulation** — multiply a coarse prediction by a fine one.
  Nonlinear interference of structure and texture. Gnarly on purpose.
- **Feedback** — take a processed prediction, re-noise it, keep
  denoising (SDEdit-style). Push too far off-manifold and it
  artifacts. That's a patch note, not a bug report.
- **Long-exposure mode** — average predicted-clean across steps: a
  long-exposure photograph of the generation thinking. Softer,
  dreamier, lower variance.
- **BPM sync** — beat-clocked parameter modulation for animated
  trajectories. The VJ rig.

## Layout
```
README.md                The whole project: name, idea, the desk
                         (crossover, EQ, residual bus,
                         cross-modulation, feedback, long-exposure,
                         BPM sync), the frame, -qt rationale,
                         status (spec stage)
```

## The `-qt` suffix
Native Qt frontend, per the goose-qt convention: the native shell is
the correct tool for a workstation. No browser jank. (When Build 1
lands, the intended stack is Qt — the sibling Qt projects use C++17 /
Qt6 / CMake + MinGW on Penny's PC; verify against the actual first
build rather than assuming.)

## Build and run — NOT YET POSSIBLE (no code)
When Build 1 lands, the house ABI applies: `git pull`, then
`.\run.bat` (run.bat always builds; see the sibling projects'
run.bats for the pattern).

## Gotchas
- **No code, no build, no version** — everything in this repo is one
  README. The next agent's job is Build 1, not a minimap update.
- The name is camouflage by design (see above) — do not "clarify" it
  into something that reads as AI on a skim.
- The desk's seven features (crossover, EQ, residual bus,
  cross-modulation, feedback, long-exposure, BPM sync) are the spec's
  load-bearing list; the first build should implement them, not a
  reduced set.
- Feedback artifacts are a patch note, not a bug report — pushing
  off-manifold is expected behaviour of the instrument, per the README.
