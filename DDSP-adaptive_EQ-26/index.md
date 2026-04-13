---
layout: default
---

# **A DDSP Framework for Adaptive Room Equalization**
### Additional material for *29th International Conference on Digital Audio Effects 2026 (DAFx26)*

> **Paper:** [link to PDF]  
> **Code:** [link to repository]  
> **Audio demos:** [link to audio folder or GitHub Pages section]  
> **Contact:** [your email / lab / handle]

---

<!-- Framework block diagram -->
<p align="center">
  <img src="assets/figs/example_EQ_animation.gif" alt="Example adaptive EQ animation" width="92%">
</p>

**Figure 1.** Example of the proposed adaptive room EQ framework. A parametric EQ continuously adapts to "flatten" the frequency response of a time-varying room/soundsystem response. The excitation signal is a music track and the corrections are performed using the iHAM-1 method and a frequency-domain loss.

## Overview

This page supports the paper by providing a longer-form summary, a compact explanation of the proposed framework, visual material, controlled examples of the adaptive behavior, and reproducible audio demonstrations.

The paper presents a modular DDSP-based framework for adaptive room equalization (ARE) that unifies classical adaptive filtering and differentiable audio optimization in a single closed-loop setting. In the manuscript, the framework is organized around four interchangeable components: the equalizer (EQ) structure, the response-estimation method, the loss function, and the optimizer. A key theoretical point is that the classical Fx-LMS algorithm appears as a special case under standard assumptions. The experiments further show that frequency-domain losses are more stable than time-domain losses in the considered nonstationary musical excitation scenarios, and that reliable online response estimation is very important for stable adaptation.


## Extended summary

Adaptive room equalization (ARE) aims to compensate time-varying acoustic coloration so that playback matches a target response even when the sound system response, room conditions or listener position changes. In the paper, this is cast as a closed-loop control problem: audio is processed in frames, the current equalized output is measured, a loss is computed against a target response, and the equalizer parameters are updated online.

The main contribution is a differentiable and modular formulation that keeps the signal chain explicit and flexible. This makes it easy to swap equalizer structures, response estimators, losses, and optimizers without changing the overall loop. The implemented experiments are not intended as a final deployed room-equalization product, but as an open and reproducible basis for exploring the relationship between classical adaptive filtering and DDSP-style optimization. The corresponding *PyTorch* source code is provided in the accompanying repository (LINK) so that the framework can be further inspected and modified.

Empirically, the paper reports that frequency-domain objectives are better suited to the tested musical and time-varying scenarios than time-domain MSE, and that structured parametric equalization can outperform long FIR baselines (Fx-LMS, Fx-FDAF) under the same experimental conditions. The manuscript also reports that frame length, response-estimation quality, and optimizer choice create a meaningful trade-off between responsiveness, stability, and compute cost.


## Limitations and future work

The paper evaluates the framework in controlled simulations based on measured room impulse responses, so the reported results should be interpreted within that scope. We highlight that real-world deployment factors such as crowd noise, loudspeaker/device nonlinearity, converter quantization, thermal drift, and uncontrolled listener movement are not yet covered. Future work should assess robustness in those conditions, as well as in the occurrence of low-information frames and further study real-time deployment with optimized implementations of higher-order update rules and accounting for I/O latency.


## Conclusions

The proposed framework establishes a principled link between classical Fx-LMS and differentiable audio optimization in a closed-loop adaptive room-equalization setting. The conducted tests conclude that frequency-domain losses are more appropriate than time-domain MSE for the tested nonstationary conditions, that accurate online response estimation is essential for stable adaptation, and that the proposed modular implementation provides a flexible basis for future algorithmic development.

---

# Proposed framework

## Conceptual overview

The diagram below should summarize the full closed-loop pipeline: input frame, parametric EQ, loudspeaker-enclosure-microphone (LEM) path, online response estimation, target response, differentiable loss, and optimizer update.

<!-- Framework block diagram -->
<p align="center">
  <img src="assets/figs/Adaptive_EQ_schematic.png" alt="Proposed adaptive room equalization framework" width="92%">
</p>

**Figure 2.** Block diagram of the proposed adaptive room equalization system. The LEM block stands for loudspeaker-enclosure-microphone, although the linear response of other elements in the sound system (e.g., amplifiers, transmission lines, crossover filters) is also included in $\mathbf{s}_k$.

This framework casts the ARE task as a frame-wise closed-loop controller. The input signal is segmented into non-overlapping frames, passed through a parametric equalizer, propagated through the room/soundsystem response, measured, compared to a target response, and used to update the equalizer parameters once per frame. The frame-length rises as a fundamental trade-off in our work, balancing between update rate and spectral resolution. In an ablation study, the size of 8192 samples is chosen as the best compromise and is used throughout. All of the experiments can be re-run with different step sizes in the repository (LINK).

## Notation

The following notation is used throughout the paper

- $u_k$: input frame at time index $k$
- $x_k$: equalizer output
- $s_k$: time-varying room / LEM response
- $y_k$: measured output at the capture device
- $y_k^\*$: target output
- $\bar{\theta}_k$: equalizer parameter vector
- $H_{\mathrm{EQ}}(e^{j\omega}; \bar{\theta})$: parametric equalizer frequency response
- $H^\*(e^{j\omega})$: target response
- $L(\cdot)$: differentiable loss
- $\Delta \bar{\theta}_k$: parameter update
- $N$: frame length (samples)
- $M$: number of biquads of parametric EQ
- $G$: global EQ gain (dB)


## Time-varying acoustic scenarios
<!-- Scenarios animations -->
<p align="center">
  <img src="assets/figs/moving_listener.gif" alt="Moving listener position scenario" width="100%">
</p>

**Figure 3.** Animation depicting the simulated moving listener scenario. With a fixed speaker position, the position of the listener is changed smoothly via interpolation of the measured frequency responses (SoundCam dataset - conference room). This results in large yet realistic structural changes in the acoustic response. Because of this, this is treated in the analysis as the wrost case scenario---when also dealing with (nonstationary) musical excitation signals.

<p align="center">
  <img src="assets/figs/moving_person.gif" alt="Moving listener position scenario" width="100%">
</p>

**Figure 4.** Animation depicting the simulated moving person scenario. With a fixed loudspeaker and listener position, the position of a person within the room is varied smoothly via interpolation. This results in slight changes in the acoustic response, although representative of the real effect of a moving human within the conference room (SoundCam dataset).

## Core equations

Hereafter a summary of the core equations used throughout the paper is presented.

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

$$
\bar{\theta}_{k+1} = \bar{\theta}_k - \eta_k \nabla_{\bar{\theta}} L(y_k, y_k^\*; \bar{\theta}_k)
$$

$$
\bar{\theta}_{k+1} = \bar{\theta}_k - \left[\nabla_{\bar{\theta}}^2 L(y_k, y_k^\*; \bar{\theta}_k)\right]^{-1}
\nabla_{\bar{\theta}} L(y_k, y_k^\*; \bar{\theta}_k)
$$

## Audio demo

The table below compares FD-MSE equalized versions of the same song. Rows correspond to transition time ($tt$), and columns correspond to optimizer. Within each row, switching optimizer keeps the same playback time position to enable seamless subjective comparison.

<style>
  .audio-demo-wrap {
    overflow-x: auto;
    margin: 1rem 0 1.4rem;
  }

  .audio-demo-table {
    width: 100%;
    border-collapse: collapse;
    min-width: 760px;
    font-size: 0.96rem;
  }

  .audio-demo-table th,
  .audio-demo-table td {
    border: 1px solid #d7dde6;
    padding: 0.6rem 0.55rem;
    text-align: center;
    vertical-align: middle;
  }

  .audio-demo-table th {
    background: #f3f6fa;
    font-weight: 600;
  }

  .audio-demo-row.is-active {
    background: #f8fcf8;
  }

  .tt-label {
    font-weight: 600;
    white-space: nowrap;
  }

  .demo-play-btn,
  .demo-ctrl-btn {
    border: 1px solid #b9c5d6;
    background: #ffffff;
    color: #243447;
    border-radius: 8px;
    padding: 0.35rem 0.55rem;
    font-size: 0.9rem;
    cursor: pointer;
    line-height: 1.2;
  }

  .demo-play-btn:hover,
  .demo-ctrl-btn:hover {
    background: #edf2f8;
  }

  .demo-play-btn.is-selected {
    border-color: #7d9bbd;
    background: #e7eff9;
  }

  .demo-play-btn.is-playing {
    border-color: #2f855a;
    background: #e6f6eb;
    color: #14532d;
    box-shadow: inset 0 0 0 1px rgba(47, 133, 90, 0.2);
    font-weight: 600;
  }

  .demo-controls {
    display: flex;
    gap: 0.35rem;
    justify-content: center;
  }

  .demo-status {
    text-align: left;
    margin-top: 0.6rem;
    color: #4b5563;
    font-size: 0.92rem;
  }

  .demo-status strong {
    color: #1f2937;
  }
</style>

<div class="audio-demo-wrap">
  <table class="audio-demo-table" aria-label="Audio demo comparison by transition time and optimizer">
    <thead>
      <tr>
        <th>Transition time ($tt$)</th>
        <th>SGD</th>
        <th>Adam</th>
        <th>Newton</th>
        <th>GHAM-1</th>
        <th>GHAM-3</th>
        <th>Controls</th>
      </tr>
    </thead>
    <tbody>
      <tr class="audio-demo-row" data-row-label="tt = 1.0 s">
        <td class="tt-label">1.0 s</td>
        <td><button class="demo-play-btn" data-label="SGD" data-src="assets/audio/example/EQ_SGD_FD_MSE_tt1p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="Adam" data-src="assets/audio/example/EQ_Adam_FD_MSE_tt1p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="Newton" data-src="assets/audio/example/EQ_Newton_FD_MSE_tt1p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="GHAM-1" data-src="assets/audio/example/EQ_GHAM_1_FD_MSE_tt1p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="GHAM-3" data-src="assets/audio/example/EQ_GHAM_3_FD_MSE_tt1p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td>
          <div class="demo-controls">
            <button class="demo-ctrl-btn demo-pause-btn" type="button">Pause</button>
            <button class="demo-ctrl-btn demo-stop-btn" type="button">Stop</button>
          </div>
        </td>
        <td hidden>
          <audio preload="metadata"></audio>
        </td>
      </tr>

      <tr class="audio-demo-row" data-row-label="tt = 15.0 s">
        <td class="tt-label">15.0 s</td>
        <td><button class="demo-play-btn" data-label="SGD" data-src="assets/audio/example/EQ_SGD_FD_MSE_tt15p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="Adam" data-src="assets/audio/example/EQ_Adam_FD_MSE_tt15p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="Newton" data-src="assets/audio/example/EQ_Newton_FD_MSE_tt15p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="GHAM-1" data-src="assets/audio/example/EQ_GHAM_1_FD_MSE_tt15p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="GHAM-3" data-src="assets/audio/example/EQ_GHAM_3_FD_MSE_tt15p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td>
          <div class="demo-controls">
            <button class="demo-ctrl-btn demo-pause-btn" type="button">Pause</button>
            <button class="demo-ctrl-btn demo-stop-btn" type="button">Stop</button>
          </div>
        </td>
        <td hidden>
          <audio preload="metadata"></audio>
        </td>
      </tr>

      <tr class="audio-demo-row" data-row-label="tt = 30.0 s">
        <td class="tt-label">30.0 s</td>
        <td><button class="demo-play-btn" data-label="SGD" data-src="assets/audio/example/EQ_SGD_FD_MSE_tt30p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="Adam" data-src="assets/audio/example/EQ_Adam_FD_MSE_tt30p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="Newton" data-src="assets/audio/example/EQ_Newton_FD_MSE_tt30p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="GHAM-1" data-src="assets/audio/example/EQ_GHAM_1_FD_MSE_tt30p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td><button class="demo-play-btn" data-label="GHAM-3" data-src="assets/audio/example/EQ_GHAM_3_FD_MSE_tt30p0_FilthyBird_IdLikeToKnow_MIX.wav">Play</button></td>
        <td>
          <div class="demo-controls">
            <button class="demo-ctrl-btn demo-pause-btn" type="button">Pause</button>
            <button class="demo-ctrl-btn demo-stop-btn" type="button">Stop</button>
          </div>
        </td>
        <td hidden>
          <audio preload="metadata"></audio>
        </td>
      </tr>
    </tbody>
  </table>

  <p class="demo-status" id="audio-demo-status">
    <strong>Status:</strong> Stopped.
  </p>
</div>

<script>
  (function () {
    var rows = Array.prototype.slice.call(document.querySelectorAll('.audio-demo-row'));
    var statusEl = document.getElementById('audio-demo-status');

    function clearRowVisual(row) {
      row.classList.remove('is-active');
      row.querySelectorAll('.demo-play-btn').forEach(function (btn) {
        btn.classList.remove('is-playing');
      });
    }

    function stopRow(row) {
      var audio = row.querySelector('audio');
      audio.pause();
      audio.currentTime = 0;
      clearRowVisual(row);
    }

    function stopOtherRows(activeRow) {
      rows.forEach(function (row) {
        if (row !== activeRow) {
          stopRow(row);
        }
      });
    }

    function setStatus(text) {
      statusEl.innerHTML = '<strong>Status:</strong> ' + text;
    }

    function markSelected(row, selectedBtn, isPlaying) {
      row.querySelectorAll('.demo-play-btn').forEach(function (btn) {
        btn.classList.remove('is-selected');
        btn.classList.remove('is-playing');
      });
      selectedBtn.classList.add('is-selected');
      if (isPlaying) {
        selectedBtn.classList.add('is-playing');
        row.classList.add('is-active');
      } else {
        row.classList.remove('is-active');
      }
    }

    function switchAndPlay(row, btn) {
      var audio = row.querySelector('audio');
      var src = btn.getAttribute('data-src');
      var label = btn.getAttribute('data-label');
      var rowLabel = row.getAttribute('data-row-label');
      var keepTime = audio.currentTime || 0;
      var sameSource = audio.getAttribute('src') === src;

      stopOtherRows(row);

      markSelected(row, btn, true);

      if (sameSource) {
        audio.play().catch(function () {});
        setStatus('Playing ' + label + ' at ' + rowLabel + '.');
        return;
      }

      audio.pause();
      audio.setAttribute('src', src);

      var finalizeSwitch = function () {
        if (isFinite(audio.duration) && audio.duration > 0) {
          audio.currentTime = Math.min(keepTime, Math.max(0, audio.duration - 0.05));
        }
        audio.play().catch(function () {});
        setStatus('Playing ' + label + ' at ' + rowLabel + '.');
      };

      if (audio.readyState >= 1) {
        finalizeSwitch();
      } else {
        audio.addEventListener('loadedmetadata', finalizeSwitch, { once: true });
      }
    }

    rows.forEach(function (row) {
      var audio = row.querySelector('audio');
      var pauseBtn = row.querySelector('.demo-pause-btn');
      var stopBtn = row.querySelector('.demo-stop-btn');

      row.querySelectorAll('.demo-play-btn').forEach(function (btn) {
        btn.addEventListener('click', function () {
          switchAndPlay(row, btn);
        });
      });

      pauseBtn.addEventListener('click', function () {
        audio.pause();
        row.classList.remove('is-active');
        setStatus('Paused at ' + row.getAttribute('data-row-label') + '.');
      });

      stopBtn.addEventListener('click', function () {
        stopRow(row);
        setStatus('Stopped at ' + row.getAttribute('data-row-label') + '.');
      });

      audio.addEventListener('ended', function () {
        clearRowVisual(row);
        setStatus('Playback ended at ' + row.getAttribute('data-row-label') + '.');
      });
    });
  })();
</script>