# **A DDSP Framework for Adaptive Room Equalization**
### Additional material for *International Conference on Digital Audio Effects (DAFx) 2026*

> **Paper:** [link to PDF]  
> **Code:** [link to repository]  
> **Audio demos:** [link to audio folder or GitHub Pages section]  
> **Contact:** [your email / lab / handle]

---

## Overview

This page supports the paper by providing a longer-form summary, a compact explanation of the proposed framework, visual material, controlled examples of the adaptive behavior, and reproducible audio demonstrations. $`\sqrt{3x-1}+(1+x)^2`$ \\sqrt{3x-1}+(1+x)^2`\\

The paper presents a modular DDSP-based framework for adaptive room equalization (ARE) that unifies classical adaptive filtering and differentiable audio optimization in a single closed-loop setting. In the manuscript, the framework is organized around four interchangeable components: the equalizer structure, the response-estimation method, the loss function, and the optimizer. A key theoretical point is that Fx-LMS appears as a special case under standard assumptions. The experiments further show that frequency-domain losses are more stable than time-domain losses in the considered nonstationary music scenarios, and that reliable online response estimation is important for stable adaptation. fileciteturn1file0 fileciteturn1file3 fileciteturn1file16

---

## Extended summary

Adaptive room equalization aims to compensate time-varying acoustic coloration so that playback matches a target response even when the room or listener position changes. In the paper, this is cast as a closed-loop control problem: audio is processed in frames, the current equalized output is measured, a loss is computed against a target response, and the equalizer parameters are updated online.

The main contribution is a differentiable and modular formulation that keeps the signal chain explicit and flexible. This makes it easy to swap equalizer structures, response estimators, losses, and optimizers without changing the overall loop. The manuscript highlights that this framework is not intended as a final deployed room-equalization product, but as an open and reproducible basis for exploring the relationship between classical adaptive filtering and DDSP-style optimization. fileciteturn1file0 fileciteturn1file17

Empirically, the paper reports that frequency-domain objectives are better suited to the tested musical and time-varying scenarios than time-domain MSE, and that structured parametric equalization can outperform long FIR baselines under the same experimental conditions. The manuscript also reports that frame length, response-estimation quality, and optimizer choice create a meaningful trade-off between responsiveness, stability, and compute cost. fileciteturn1file5 fileciteturn1file10 fileciteturn1file15

---

## Limitations and future work

The paper evaluates the framework in controlled simulations based on measured room impulse responses, so the reported results should be interpreted within that scope. The manuscript explicitly notes that real-world deployment factors such as crowd noise, nonlinear loudspeaker and microphone behavior, converter quantization, thermal drift, and uncontrolled listener movement are not yet covered. Future work should study robustness in those conditions, as well as low-information frames and more optimized implementations of higher-order update rules. fileciteturn1file4

---

## Conclusions

The proposed framework establishes a principled link between classical Fx-LMS and differentiable audio optimization in a closed-loop adaptive room-equalization setting. The manuscript concludes that frequency-domain losses are more appropriate than time-domain MSE for the tested nonstationary conditions, that accurate online response estimation is essential for stable adaptation, and that the proposed modular implementation provides a flexible basis for future algorithmic development. fileciteturn1file16

---

# Proposed framework

## Conceptual overview

The diagram below should summarize the full closed-loop pipeline: input frame, parametric EQ, loudspeaker-enclosure-microphone path, online response estimation, target response, differentiable loss, and optimizer update.

<!-- Replace with your schematic figure -->
<p align="center">
  <img src="assets/figures/framework_placeholder.png" alt="Proposed adaptive room equalization framework" width="92%">
</p>

**Figure X.** [Replace this caption with the final caption for the framework schematic.]

The manuscript frames ARE as a frame-wise closed-loop controller. The input signal is segmented into non-overlapping frames, passed through a parametric equalizer, propagated through the room/system response, measured, compared to a target response, and used to update the equalizer parameters once per frame. The paper also emphasizes the frame-length trade-off between update rate and spectral resolution, with 8192 samples chosen as the best compromise in the ablation study. fileciteturn1file17 fileciteturn1file1

## Notation

Use this section to define the symbols exactly as they appear in the manuscript.

- $`u_k`$: input frame at time index $k$
- $x_k$: equalizer output
- $s_k$: time-varying room / LEM response
- $y_k$: measured output at the capture device
- $y_k^\*$: target output
- $\bar{\theta}_k$: equalizer parameter vector
- $H_{\mathrm{EQ}}(e^{j\omega}; \bar{\theta})$: parametric equalizer frequency response
- $H^\*(e^{j\omega})$: target response
- $L(\cdot)$: differentiable loss
- $\Delta \bar{\theta}_k$: parameter update

You can add any paper-specific conventions here, for example:
- frame length $N$
- number of biquads $M$
- global gain $G$
- optimizer hyperparameters
- estimator smoothing factors

## Core equations

Use the equations below as a template. Replace names, symbols, or formatting to match the final manuscript version.

### Closed-loop parameter update

$$
\bar{\theta}_{k+1} = \bar{\theta}_k + \Delta \bar{\theta}_k
$$

### Parametric equalizer

For a cascade of biquad sections:

$$
H_k(z; \theta_m) =
\frac{b_{0,m} + b_{1,m}z^{-1} + b_{2,m}z^{-2}}
     {a_{0,m} + a_{1,m}z^{-1} + a_{2,m}z^{-2}}
$$

and the full equalizer response:

$$
H_{\mathrm{EQ}}(e^{j\omega}; \bar{\theta}) = G \prod_{m=1}^{M} H_m(e^{j\omega}; \theta_m)
$$

### Example frequency-domain loss

$$
\mathrm{FD\text{-}MSE}(y_k, H^\*) =
\frac{1}{N}\sum_{n=1}^{N}
\left(
\left| \frac{Y_k(e^{j\omega_n})}{U_k(e^{j\omega_n})} \right|
-
\left|H^\*(e^{j\omega_n})\right|
\right)^2
$$

### Example optimizer updates

Add the exact update rule(s) you want to show here.

$$
\bar{\theta}_{k+1} = \bar{\theta}_k - \eta_k \nabla_{\bar{\theta}} L(y_k, y_k^\*; \bar{\theta}_k)
$$

$$
\bar{\theta}_{k+1} = \bar{\theta}_k - \left[\nabla_{\bar{\theta}}^2 L(y_k, y_k^\*; \bar{\theta}_k)\right]^{-1}
\nabla_{\bar{\theta}} L(y_k, y_k^\*; \bar{\theta}_k)
$$

> Add any derivation notes, assumptions, and implementation details here.  
> If you want to include the Fx-LMS special case, place it in a short derivation block here.

---

# Visual explanation of the room setup

## Room layout and response illustration

Paste the room-layout animation, interactive schematic, or HTML visualization here.

<!-- Replace with your HTML animation block -->
<div class="demo-box">
  <p><strong>Placeholder:</strong> insert room layout animation here.</p>
  <p>[HTML / SVG / canvas animation goes here]</p>
</div>

<!-- Replace with another visualization if needed -->
<div class="demo-box">
  <p><strong>Placeholder:</strong> insert measured / estimated room-response animation here.</p>
</div>

Add a short caption underneath explaining what the animation shows, how the room changes, and how the response estimate tracks those changes.

---

# Control-performance examples

## Example executions

Use this section for representative trajectories, animated plots, or short clips that illustrate the behavior of the controller.

<!-- Example figure -->
<p align="center">
  <img src="assets/figures/control_example_placeholder.png" alt="Example control performance" width="92%">
</p>

**Figure Y.** [Replace with a caption describing the control example.]

You can include:
- a time trace of the validation metric,
- a comparison of optimizers,
- a before/after frequency-response plot,
- or an animation of the equalizer state over time.

### Suggested narrative

Describe one or two representative scenarios:
- a stable case showing smooth convergence,
- a difficult case showing adaptation during a response change,
- a failure mode, if relevant, to be transparent about the method’s limitations.

---

# Systematic results

## Summary figures and tables

This subsection is meant to reproduce the paper’s main result plots and tables in a compact, browsable form.

### Main result figures

<!-- Replace with figures from the paper -->
<p align="center">
  <img src="assets/figures/results_main_placeholder.png" alt="Main results figure" width="92%">
</p>

<p align="center">
  <img src="assets/figures/results_comparison_placeholder.png" alt="Comparison figure" width="92%">
</p>

### Main result table

<!-- Replace with your final table -->
| Scenario | Optimizer | Metric 1 | Metric 2 | Metric 3 | Notes |
|---|---:|---:|---:|---:|---|
| [Scenario A] | [SGD] | [ ] | [ ] | [ ] | [ ] |
| [Scenario A] | [Adam] | [ ] | [ ] | [ ] | [ ] |
| [Scenario A] | [Newton] | [ ] | [ ] | [ ] | [ ] |
| [Scenario A] | [iHAM-1] | [ ] | [ ] | [ ] | [ ] |
| [Scenario A] | [iHAM-3] | [ ] | [ ] | [ ] | [ ] |

Add more rows for each song, transition time, or evaluation condition.

---

## Reproducible audio demonstrations

This section implements a per-song interactive player where each row contains multiple synchronized versions of the same excerpt. The idea is:

1. press play for the song,
2. switch between versions while it is playing,
3. keep the same playback position when switching,
4. compare optimizers without losing temporal alignment.

The code below uses one hidden `<audio>` element per version and keeps all versions aligned by transferring the current playback time from the active version to the newly selected one.

### Audio demo structure

Duplicate one block per song and replace the file paths.

<div class="audio-demo-list">

  <div class="audio-song" data-song="song-01">
    <div class="audio-song-header">
      <div>
        <strong>Song 01:</strong> [Track title]
        <div class="audio-song-subtitle">[Genre / duration / notes]</div>
      </div>
      <div class="audio-song-status">
        Active version: <span data-active-version>Input</span>
      </div>
    </div>

    <div class="audio-controls">
      <button type="button" class="song-play-toggle">Play / Pause</button>

      <div class="version-buttons" role="group" aria-label="Audio versions">
        <button type="button" data-version="input">Input</button>
        <button type="button" data-version="desired">Desired</button>
        <button type="button" data-version="noeq">No EQ</button>
        <button type="button" data-version="sgd">SGD</button>
        <button type="button" data-version="adam">Adam</button>
        <button type="button" data-version="newton">Newton</button>
        <button type="button" data-version="iham1">iHAM-1</button>
        <button type="button" data-version="iham3">iHAM-3</button>
      </div>
    </div>

    <div class="audio-meta">
      <span>Tip: switch versions while audio is playing to hear the change at the same time stamp.</span>
    </div>

    <audio data-version="input" src="assets/audio/song-01/input.wav" preload="auto"></audio>
    <audio data-version="desired" src="assets/audio/song-01/desired.wav" preload="auto"></audio>
    <audio data-version="noeq" src="assets/audio/song-01/noeq.wav" preload="auto"></audio>
    <audio data-version="sgd" src="assets/audio/song-01/sgd.wav" preload="auto"></audio>
    <audio data-version="adam" src="assets/audio/song-01/adam.wav" preload="auto"></audio>
    <audio data-version="newton" src="assets/audio/song-01/newton.wav" preload="auto"></audio>
    <audio data-version="iham1" src="assets/audio/song-01/iham1.wav" preload="auto"></audio>
    <audio data-version="iham3" src="assets/audio/song-01/iham3.wav" preload="auto"></audio>
  </div>

  <div class="audio-song" data-song="song-02">
    <div class="audio-song-header">
      <div>
        <strong>Song 02:</strong> [Track title]
        <div class="audio-song-subtitle">[Genre / duration / notes]</div>
      </div>
      <div class="audio-song-status">
        Active version: <span data-active-version>Input</span>
      </div>
    </div>

    <div class="audio-controls">
      <button type="button" class="song-play-toggle">Play / Pause</button>
      <div class="version-buttons" role="group" aria-label="Audio versions">
        <button type="button" data-version="input">Input</button>
        <button type="button" data-version="desired">Desired</button>
        <button type="button" data-version="noeq">No EQ</button>
        <button type="button" data-version="sgd">SGD</button>
        <button type="button" data-version="adam">Adam</button>
        <button type="button" data-version="newton">Newton</button>
        <button type="button" data-version="iham1">iHAM-1</button>
        <button type="button" data-version="iham3">iHAM-3</button>
      </div>
    </div>

    <div class="audio-meta">
      <span>Tip: duplicate this block for every song in your evaluation set.</span>
    </div>

    <audio data-version="input" src="assets/audio/song-02/input.wav" preload="auto"></audio>
    <audio data-version="desired" src="assets/audio/song-02/desired.wav" preload="auto"></audio>
    <audio data-version="noeq" src="assets/audio/song-02/noeq.wav" preload="auto"></audio>
    <audio data-version="sgd" src="assets/audio/song-02/sgd.wav" preload="auto"></audio>
    <audio data-version="adam" src="assets/audio/song-02/adam.wav" preload="auto"></audio>
    <audio data-version="newton" src="assets/audio/song-02/newton.wav" preload="auto"></audio>
    <audio data-version="iham1" src="assets/audio/song-02/iham1.wav" preload="auto"></audio>
    <audio data-version="iham3" src="assets/audio/song-02/iham3.wav" preload="auto"></audio>
  </div>

</div>

### Suggested CSS

Paste this into your site stylesheet or inside a `<style>` block if your GitHub Pages setup allows it.

```html
<style>
.audio-demo-list {
  display: grid;
  gap: 1rem;
  margin: 1.25rem 0;
}

.audio-song {
  border: 1px solid #ddd;
  border-radius: 14px;
  padding: 1rem;
  background: #fafafa;
}

.audio-song-header {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
  align-items: baseline;
  flex-wrap: wrap;
  margin-bottom: 0.75rem;
}

.audio-song-subtitle {
  margin-top: 0.25rem;
  font-size: 0.95rem;
  opacity: 0.8;
}

.audio-song-status {
  font-size: 0.95rem;
  white-space: nowrap;
}

.audio-controls {
  display: grid;
  gap: 0.75rem;
}

.song-play-toggle,
.version-buttons button {
  border: 1px solid #bbb;
  border-radius: 999px;
  padding: 0.45rem 0.8rem;
  background: white;
  cursor: pointer;
}

.version-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.45rem;
}

.version-buttons button.is-active {
  border-color: #222;
  font-weight: 600;
  background: #efefef;
}

.audio-meta {
  margin-top: 0.75rem;
  font-size: 0.9rem;
  opacity: 0.8;
}

.audio-song audio {
  display: none;
}
</style>
```

### Suggested JavaScript

This script keeps the selected version aligned to the current playback time when switching, so comparing optimizers feels immediate and time-consistent.

```html
<script>
document.addEventListener("DOMContentLoaded", () => {
  const songBlocks = document.querySelectorAll(".audio-song");

  function getAudioMap(songEl) {
    const map = new Map();
    songEl.querySelectorAll("audio[data-version]").forEach((audio) => {
      map.set(audio.dataset.version, audio);
    });
    return map;
  }

  function getActiveVersion(songEl) {
    return songEl.dataset.activeVersion || "input";
  }

  function setActiveVersionUI(songEl, version) {
    songEl.dataset.activeVersion = version;

    const label = songEl.querySelector("[data-active-version]");
    if (label) {
      const niceName = {
        input: "Input",
        desired: "Desired",
        noeq: "No EQ",
        sgd: "SGD",
        adam: "Adam",
        newton: "Newton",
        iham1: "iHAM-1",
        iham3: "iHAM-3",
      }[version] || version;
      label.textContent = niceName;
    }

    songEl.querySelectorAll(".version-buttons button").forEach((btn) => {
      btn.classList.toggle("is-active", btn.dataset.version === version);
    });
  }

  function switchVersion(songEl, nextVersion, shouldContinuePlaying) {
    const audioMap = getAudioMap(songEl);
    const currentVersion = getActiveVersion(songEl);
    const currentAudio = audioMap.get(currentVersion);
    const nextAudio = audioMap.get(nextVersion);

    if (!nextAudio) return;

    const t = currentAudio ? currentAudio.currentTime || 0 : 0;

    if (currentAudio && currentAudio !== nextAudio) {
      currentAudio.pause();
    }

    nextAudio.currentTime = t;
    setActiveVersionUI(songEl, nextVersion);

    if (shouldContinuePlaying) {
      const playPromise = nextAudio.play();
      if (playPromise && typeof playPromise.catch === "function") {
        playPromise.catch(() => {});
      }
    }
  }

  songBlocks.forEach((songEl) => {
    const audioMap = getAudioMap(songEl);
    const playBtn = songEl.querySelector(".song-play-toggle");

    // Initialize with the first declared version or input.
    const initialVersion = audioMap.has("input")
      ? "input"
      : (audioMap.keys().next().value || "input");

    songEl.dataset.activeVersion = initialVersion;
    setActiveVersionUI(songEl, initialVersion);

    playBtn?.addEventListener("click", () => {
      const activeVersion = getActiveVersion(songEl);
      const activeAudio = audioMap.get(activeVersion);
      if (!activeAudio) return;

      if (activeAudio.paused) {
        const playPromise = activeAudio.play();
        if (playPromise && typeof playPromise.catch === "function") {
          playPromise.catch(() => {});
        }
      } else {
        activeAudio.pause();
      }
    });

    songEl.querySelectorAll(".version-buttons button").forEach((btn) => {
      btn.addEventListener("click", () => {
        const activeVersion = getActiveVersion(songEl);
        const activeAudio = audioMap.get(activeVersion);
        const shouldContinuePlaying = activeAudio ? !activeAudio.paused : false;
        switchVersion(songEl, btn.dataset.version, shouldContinuePlaying);
      });
    });

    // Keep the active version label consistent if the user plays another version directly.
    audioMap.forEach((audio, version) => {
      audio.addEventListener("play", () => {
        setActiveVersionUI(songEl, version);
      });
    });
  });
});
</script>
```

### Audio file naming convention

Use a stable folder structure so the template stays easy to maintain:

```text
assets/
  audio/
    song-01/
      input.wav
      desired.wav
      noeq.wav
      sgd.wav
      adam.wav
      newton.wav
      iham1.wav
      iham3.wav
    song-02/
      input.wav
      desired.wav
      noeq.wav
      sgd.wav
      adam.wav
      newton.wav
      iham1.wav
      iham3.wav
```

---

# How to cite
TODO

---

Thank you for reading. This page accompanies the paper with additional explanations, figures, and audio examples so that the experiments and control behavior can be inspected interactively.

