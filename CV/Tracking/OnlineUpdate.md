# ATOM
# Keypoints
  * Target state estimation
    * Predict IoU
  * Classifier entirely trained online
    * Online optimization
# Approaches
  * Target state estimation
    * IoU-net with reference(target) image information modulated
    * Multiple initial images for ensemble
  * Online classification
    * Classifier: $f(x ; w)=\phi_{2}\left(w_{2} * \phi_{1}\left(w_{1} * x\right)\right)$, x: backbone feature
      * Gaussian label for supervision
      * Loss: $L(w)=\sum_{j=1}^{m} \gamma_{j}\left\|f\left(x_{j} ; w\right)-y_{j}\right\|^{2}+\sum_{k} \lambda_{k}\left\|w_{k}\right\|^{2}$
      * One-order approx. (SDP problem):
      $\begin{aligned}
      & \tilde{L}_{w}(\Delta w) \approx L(w+\Delta w)
      \\
      &\tilde{L}_{w}(\Delta w)=\Delta w^{\mathrm{T}} J_{w}^{\mathrm{T}} J_{w} \Delta w+2 \Delta w^{\mathrm{T}} J_{w}^{\mathrm{T}} r_{w}+r_{w}^{\mathrm{T}} r_{w}
      \end{aligned}$
      * optimized via Conjugate Gradient(CG) Descent
    * Hard Sample Mining




# Learning Discriminative Model Prediction for Tracking
DiMP
ATOM improved
## Keypoints
* Modelisation for target/background
* Online updating
  * Target model (weights) prediction
  * Discriminative learning losses

## Approaches
* Overall pipeline:
  * classfication: predictor predict the W for scoring
    * training set: for W Prediction
    * testing set: for tracking with the W predicted on training set
  * regression: IoU-net
* Discriminative loss (for predictor):
  $L(f)=\frac{1}{ | S_{\text { train }} |} \sum_{(x, c) \in S_{\text { trisin }}}\|r(x * f, c)\|^{2}+\|\lambda f\|^{2}$
  * Residual between score/gt:
    $\begin{aligned}&
    r(s, c)=v_{c} \cdot\left(m_{c} s+\left(1-m_{c}\right) \max (0, s)-y_{c}\right)
    \\&
    s=x * f \text{ (target confidence score)}
    \end{aligned}$
    * Learnable parameter:
      * label confidence score $y_c$
      * spatial weight $v_c$
      * target mask $m_c$
  * learned discriminative loss, with triangular based function
    $\begin{aligned}&
    y_{c}(t)=\sum_{k=0}^{N-1} \phi_{k}^{y} \rho_{k}(\|t-c\|)
    \\&
    \rho_{k}(d)=\left\{\begin{array}{ll}{\max \left(0,1-\frac{|d-k \Delta|}{\Delta}\right),} & {k<N-1}
    \\
    {\max \left(0, \min \left(1,1+\frac{d-k \Delta}{\Delta}\right)\right),} & {k=N-1}\end{array}\right.
    \\
    \end{aligned}$

    * Fit a certain function

# Tracking Holistic Object Representations

* msr
