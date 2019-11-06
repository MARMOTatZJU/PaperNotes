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
Improved ATOM
## Keypoints
* Modelisation for target/background
* Online updating
  * Target model (weights) prediction
  * Discriminative learning losses
  * Efficient optimization

## Approaches
* target model as conv weights
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

  $\begin{aligned}
  y_{c}(t)&=\sum_{k=0}^{N-1} \phi_{k}^{y} \rho_{k}(\|t-c\|)
  \\
  \rho_{k}(d)&=\{
  \begin{array}{ll}
  \max \left(0,1-\frac{|d-k \Delta|}{\Delta}\right), & {k<N-1}
  \\
  \max (0, \min \left(1,1+\frac{d-k \Delta}{\Delta}\right), &{k=N-1}
  \end{array}
  \end{aligned}$
  <!---->
    * Fit a certain function
* Training
  * similar to meta-learning
  * $\left(M_{\text { train }}, M_{\text { test }}\right)$ with $M=\left\{\left(I_{j}, b_{j}\right)\right\}_{j=1}^{N_{\text { frames }}}$
  * $L_{\mathrm{tot}}=\beta L_{\mathrm{cls}}+L_{\mathrm{bb}}$

# Tracking Holistic Object Representations
* Keypoints
  * template matching method
    * choice of template matters
  * Need for fitting in dynamic environ.
    * rotation / illumination / occlusion / motion blur / etc.
  * Holistic object representations
    * store diverse template
    * diversity measurement
* Approaches
  * Condition to become a template
    * a crop should only be stored as a template if it contains additional information about the object compared to the already
* LTM(long time module)
  * accumulated templates: set of feature candidates
    * similarity: $f_{1} \star f_{2}$
    * gramm matrix:
      $G\left(f_{1}, \cdots, f_{n}\right)=\left[\begin{array}{cccc}{f_{1} \star f_{1}} & {f_{1} \star f_{2}} & {\cdots} & {f_{1} \star f_{n}} \\ {\vdots} & {\vdots} & {\ddots} & {\vdots} \\ {f_{n} \star f_{1}} & {f_{n} \star f_{2}} & {\cdots} & {f_{n} \star f_{n}}\end{array}\right]$
    * objective: maximize feature group volume $\max _{f_{1}, f_{2}, \ldots, f_{n}} \Gamma\left(f_{1}, \ldots, f_{n}\right) \propto \max _{f_{1}, f_{2}, \ldots, f_{n}}\left|G\left(f_{1}, f_{2}, \ldots, f_{n}\right)\right|$
      * representing tracked object's manifold in this embedded representation
    * set a lower bound for similarity between candidate & given ground-truth bbox
      * basic form $f_{c} \star f_{1}>\ell \cdot G_{11}$
      * two strategies for robustness
        * addaptive: $f_{c} \star f_{1}>\ell \cdot G_{11}-\gamma$, where $\gamma$ is diversity from STM
        * ensemble: $f_{c} \star f_{1 : n}>\ell \cdot \operatorname{diag}(G)$
* STM(short time module)
  * first-in-first-out (no selection)
  * diversity measurement: $\gamma=1-\frac{2}{N(N+1) G_{s t, m a x}} \sum_{i<j}^{N} G_{s t, i j}$
* Inference strategy
  * Modulation: layer attention
  * ST-LT switch: IoU(STM, LTM)
    * higher: choose STM
    * lower: choose LTM
  * extract frame every 10 frames
