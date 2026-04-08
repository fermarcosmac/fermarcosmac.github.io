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

## Overview

This page supports the paper by providing a longer-form summary, a compact explanation of the proposed framework, visual material, controlled examples of the adaptive behavior, and reproducible audio demonstrations.

The paper presents a modular DDSP-based framework for adaptive room equalization (ARE) that unifies classical adaptive filtering and differentiable audio optimization in a single closed-loop setting. In the manuscript, the framework is organized around four interchangeable components: the equalizer (EQ) structure, the response-estimation method, the loss function, and the optimizer. A key theoretical point is that the classical Fx-LMS algorithm appears as a special case under standard assumptions. The experiments further show that frequency-domain losses are more stable than time-domain losses in the considered nonstationary musical excitation scenarios, and that reliable online response estimation is very important for stable adaptation.


## Extended summary

Adaptive room equalization (ARE) aims to compensate time-varying acoustic coloration so that playback matches a target response even when the sound system response, room conditions or listener position changes. In the paper, this is cast as a closed-loop control problem: audio is processed in frames, the current equalized output is measured, a loss is computed against a target response, and the equalizer parameters are updated online.

The main contribution is a differentiable and modular formulation that keeps the signal chain explicit and flexible. This makes it easy to swap equalizer structures, response estimators, losses, and optimizers without changing the overall loop. The manuscript highlights that this framework is not intended as a final deployed room-equalization product, but as an open and reproducible basis for exploring the relationship between classical adaptive filtering and DDSP-style optimization. The corresponding *PyTorch* implementation is provided in the accompanying repository (LINK) so that the framework can be further inspected and modified.

Empirically, the paper reports that frequency-domain objectives are better suited to the tested musical and time-varying scenarios than time-domain MSE, and that structured parametric equalization can outperform long FIR baselines (Fx-LMS, Fx-FDAF) under the same experimental conditions. The manuscript also reports that frame length, response-estimation quality, and optimizer choice create a meaningful trade-off between responsiveness, stability, and compute cost.


## Limitations and future work

The paper evaluates the framework in controlled simulations based on measured room impulse responses, so the reported results should be interpreted within that scope. We highlight that real-world deployment factors such as crowd noise, nonlinear loudspeaker and microphone behavior, converter quantization, thermal drift, and uncontrolled listener movement are not yet covered. Future work should study robustness in those conditions, as well as low-information frames and more optimized implementations of higher-order update rules.


## Conclusions

The proposed framework establishes a principled link between classical Fx-LMS and differentiable audio optimization in a closed-loop adaptive room-equalization setting. The manuscript concludes that frequency-domain losses are more appropriate than time-domain MSE for the tested nonstationary conditions, that accurate online response estimation is essential for stable adaptation, and that the proposed modular implementation provides a flexible basis for future algorithmic development.

---

# Proposed framework

## Conceptual overview

The diagram below should summarize the full closed-loop pipeline: input frame, parametric EQ, loudspeaker-enclosure-microphone path, online response estimation, target response, differentiable loss, and optimizer update.

<!-- Replace with your schematic figure -->
<p align="center">
  <img src="assets/figs/Adaptive_EQ_schematic.png" alt="Proposed adaptive room equalization framework" width="92%">
</p>

**Figure 1.** Block diagram of the proposed adaptive room equalization system. The LEM block stands for loudspeaker-enclosure-microphone, although the linear response of other elements in the sound system (e.g., amplifiers, transmission lines, crossover filters) is also included in $\mathbf{s}_k$.

The manuscript frames ARE as a frame-wise closed-loop controller. The input signal is segmented into non-overlapping frames, passed through a parametric equalizer, propagated through the room/system response, measured, compared to a target response, and used to update the equalizer parameters once per frame. The paper also emphasizes the frame-length trade-off between update rate and spectral resolution, with 8192 samples chosen as the best compromise in the ablation study.

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
- frame length (samples): $N$
- number of biquads of parametric EQ: $M$
- global gain parameter: $G$