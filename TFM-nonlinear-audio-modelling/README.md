# DEVELOPMENT OF A SYSTEM FOR CHARACTERIZATION AND COMPENSATION OF THE RESPONSE IN NEXT-GENERATION AUDIO

## Master of Science in Signal Theory and Communications: Master's Thesis

#### Author: Fernando Marcos Macías
#### Supervisor: José Luis Blanco Murillo

# Abstract: 

This thesis presents a comprehensive methodology for the characterization and compensation of nonlinear audio systems, with a focus on the generalized Hammerstein model. Utilizing MATLAB, we developed an analytic identification *pipeline* that effectively models nonlinear behaviour in audio devices. It includes modules to automate measurement, estimation of basic parameters of the structure (i.e., nonlinearity order and system memory), identification, synthesis, and computation of an inverse system approximation. The pipeline's performance was validated on synthetic systems matching the generalized Hammerstein structure, demonstrating consistent and accurate identification across varied responses. Furthermore, the method was applied to a real-world microphone pre-amplifier, successfully characterizing its nonlinearities under different gain settings, consistent with the Volterra-Wiener model's predictions. The synthesis results match those expected for the generalized Hammerstein approximation to a complex nonlinear system. The inversion results remain far from optimal, but insight into its practical implementation was attained and is provided in hopes of being leveraged in future studies.

In addition to the analytical approach, a statistical learning framework was implemented utilizing the Pytorch library, unveiling its strengths and weaknesses with respect to our analytical *pipeline*. Notably, we studied the influence of leveraging the Hilbert envelope within the optimizable loss function, which yields promising results, although these remain suboptimal. This research underscores the importance of verifying model assumptions and exploring hybrid methodologies, combining advanced signal processing techniques with machine learning, to enhance the accuracy and robustness of nonlinear system identification and compensation.


# Results:

### Synthesis: simulated systems

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/3.2.0/es5/tex-mml-chtml.js">
</script>

| System | Signal | SNR (dB) | \\(x(t)\\)   | \\(y(t)\\) | \\(\hat{y}(t)\\): *pipeline* | \\(\hat{y}(t)\\): *learning* |
|--------|--------|----------|--------------|------------|------------------------------|------------------------------|
| BPLP | ESS | 0 | <audio controls style="width: 80px;"><source src="web_audios/ESS_BPLP_0_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_BPLP_0_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_BPLP_0_yhat.wav" type="audio/wav"></audio> | - |
| BPLP | ESS | \\(\infty\\) | <audio controls style="width: 80px;"><source src="web_audios/ESS_BPLP_Inf_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_BPLP_Inf_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_BPLP_Inf_yhat.wav" type="audio/wav"></audio> | - |
| BPLP | Guitar | 0 | <audio controls style="width: 80px;"><source src="web_audios/guit_BPLP_0_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/guit_BPLP_0_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/guit_BPLP_0_yhat.wav" type="audio/wav"></audio> | - |
| BPLP | Guitar | \\(\infty\\) | <audio controls style="width: 80px;"><source src="web_audios/guit_BPLP_Inf_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/guit_BPLP_Inf_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/guit_BPLP_Inf_yhat.wav" type="audio/wav"></audio> | - |
| WB | ESS | 0 | <audio controls style="width: 80px;"><source src="web_audios/ESS_WB_0_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_WB_0_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_WB_0_yhat.wav" type="audio/wav"></audio> | - |
| WB | ESS | \\(\infty\\) | <audio controls style="width: 80px;"><source src="web_audios/ESS_WB_Inf_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_WB_Inf_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_WB_Inf_yhat.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/ESS_WB_Inf_yhat_LV.wav" type="audio/wav"></audio> |
| WB | Guitar | 0 | <audio controls style="width: 80px;"><source src="web_audios/guit_WB_0_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/guit_WB_0_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/guit_WB_0_yhat.wav" type="audio/wav"></audio> | - |
| WB | Guitar | \\(\infty\\) | <audio controls style="width: 80px;"><source src="web_audios/guit_WB_Inf_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/guit_WB_Inf_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/guit_WB_Inf_yhat.wav" type="audio/wav"></audio> | - |



### Synthesis: SHURE FP11

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/3.2.0/es5/tex-mml-chtml.js">
</script>

| System | Signal | \\(x(t)\\) | \\(y(t)\\) | \\(\hat{y}(t)\\): *pipeline* |
|--------|--------|------------|------------|------------------------------|
|FP11-66 | ESS | <audio controls style="width: 80px;"><source src="web_audios/FP11_66_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/FP11_66_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/FP11_66_yhat.wav" type="audio/wav"></audio> |
|FP11-72 | ESS | <audio controls style="width: 80px;"><source src="web_audios/FP11_72_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/FP11_72_y.wav" type="audio/wav"></audio>  | <audio controls style="width: 80px;"><source src="web_audios/FP11_72_yhat.wav" type="audio/wav"></audio> |
|FP11-78 | ESS | <audio controls style="width: 80px;"><source src="web_audios/FP11_78_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/FP11_78_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/FP11_78_yhat.wav" type="audio/wav"></audio> |
|FP11-84 | ESS | <audio controls style="width: 80px;"><source src="web_audios/FP11_84_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/FP11_84_y.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/FP11_84_yhat.wav" type="audio/wav"></audio> |


### Inversion: WB System

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/3.2.0/es5/tex-mml-chtml.js">
</script>

| Order | \\(x(t)\\) | \\(\hat{x}(t)\\)|
|-------|------------|-----------------|
| 1 | <audio controls style="width: 80px;"><source src="web_audios/WB_inv_1_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/WB_inv_1_xhat.wav" type="audio/wav"></audio> |
| 2 | <audio controls style="width: 80px;"><source src="web_audios/WB_inv_2_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/WB_inv_2_xhat.wav" type="audio/wav"></audio> |
| 3 | <audio controls style="width: 80px;"><source src="web_audios/WB_inv_3_x.wav" type="audio/wav"></audio> | <audio controls style="width: 80px;"><source src="web_audios/WB_inv_3_xhat.wav" type="audio/wav"></audio> |


