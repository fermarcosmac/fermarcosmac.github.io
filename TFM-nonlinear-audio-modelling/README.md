# DEVELOPMENT OF A SYSTEM FOR CHARACTERIZATION AND COMPENSATION OF THE RESPONSE IN NEXT-GENERATION AUDIO

## Master of Science in Signal Theory and Communications: Master's Thesis

#### Author: Fernando Marcos Macías
#### Supervisor: José Luis Blanco Murillo

### Abstract: 

This thesis presents a comprehensive methodology for the characterization and compensation of nonlinear audio systems, with a focus on the generalized Hammerstein model. Utilizing MATLAB, we developed an analytic identification *pipeline* that effectively models nonlinear behaviour in audio devices. It includes modules to automate measurement, estimation of basic parameters of the structure (i.e., nonlinearity order and system memory), identification, synthesis, and computation of an inverse system approximation. The pipeline's performance was validated on synthetic systems matching the generalized Hammerstein structure, demonstrating consistent and accurate identification across varied responses. Furthermore, the method was applied to a real-world microphone pre-amplifier, successfully characterizing its nonlinearities under different gain settings, consistent with the Volterra-Wiener model's predictions. The synthesis results match those expected for the generalized Hammerstein approximation to a complex nonlinear system. The inversion results remain far from optimal, but insight into its practical implementation was attained and is provided in hopes of being leveraged in future studies.

In addition to the analytical approach, a statistical learning framework was implemented utilizing the Pytorch library, unveiling its strengths and weaknesses with respect to our analytical *pipeline*. Notably, we studied the influence of leveraging the Hilbert envelope within the optimizable loss function, which yields promising results, although these remain suboptimal. This research underscores the importance of verifying model assumptions and exploring hybrid methodologies, combining advanced signal processing techniques with machine learning, to enhance the accuracy and robustness of nonlinear system identification and compensation.
