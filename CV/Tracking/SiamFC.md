# Fully-Convlutional Siamese Networks for Object Tracking
SiamFC
## Keypoints
* Similarity Learning via ConvNet
* Offline trained in pairs
* Siamese Architecture
* Scoring-based, no regressor

## Approaches
### Deep similarity Learning
Learning $f(z,x)$, where
$\begin{cases}
  z, \text{examplar}
\\
  x, \text{candidate}
\end{cases}$<br>
SiamFC: $f(z,x)=\varphi(z)\star\varphi(x)+b\cdot\mathbb{1}$ (cross-correlation)
#### Siamese fashion
embedding(ConvNet) + metrics(cross-correlation) => score map D<br>
Peak in the score map => u<br>
Diplacement = k*||u-c|| to target bbox <br>
metrics derived in the embedding space
##### Restrictions
* Convnet be no padding, following the strict translation invariance

### Training
Retrieving pairs from dataset: examplar image & search image (both centered)<br>
Image extended with the mean of color per channel (RGB)<br>
Positive examples: distance within R <br>
Negative examples: distance surpassing R<br>

# Triplet Loss in Siamese Network for Object Tracking
## Keypoints
* Introduce Triplet Loss to  
  * Consideration of triplet relation: examplar / positive instance / negative instance
  * More training pairs
    * Exemplar-instance => Examplar-Positive instance-Negative instance
    * M+N => M*N
  * Stronger feedback

Nota: patch: HxWxC, feature map

## Approaches
* Similarity: $\mathcal { V }=f ( z , x ) = \phi ( z ) * \phi ( x ) + b$
* Probability: $\operatorname { prob } \left( v p _ { i } , v n _ { j } \right) = \frac { e ^ { v p _ { i } } } { e ^ { v p _ { i } } + e ^ { v n _ { j } } }$
* Triplet loss: $L _ { t } \left( \mathcal { V } _ { p } , \mathcal { V } _ { n } \right) = - \frac { 1 } { M N } \sum _ { i } ^ { M } \sum _ { j } ^ { N } \log p r o b \left( v p _ { i } , v n _ { j } \right)$

# ATOM: Accurate Tracking by Overlap Maximization
ATOM
## Keypoitns
* IoU prediction module trained offline
* Classification module trained online

## Comments
More computational complexity (online update) and lower performance on VOT2018 than SiamRPN++


# End-to-end representation learning for Correlation Filter based Tracking
SiamFC + CF

## Keypoints
* SiamCF, CF
* Interpretation of CF learner

## Approach
* CF added on the examplar image $x$<br>
$h _ { \rho , s , b } \left( x ^ { \prime } , z ^ { \prime } \right) = s \omega \left( f _ { \rho } \left( x ^ { \prime } \right) \right) \star f _ { \rho } \left( z ^ { \prime } \right) + b$
* Closed form solution: <br>
$\left\{ \begin{aligned} k & = \frac { 1 } { n } ( x \star x ) + \lambda \delta \\ k * \alpha & = \frac { 1 } { n } y \\ w & = \alpha \star x \end{aligned} \right . \Rightarrow \left\{ \begin{array} { l } { \hat { k } = \frac { 1 } { n } \left( \widehat { x } ^ { * } \circ \widehat { x } \right) + \lambda \mathbb { 1 } } \\ { \widehat { \alpha } = \frac { 1 } { n } \widehat { k } ^ { - 1 } \circ \widehat { y } } \\ { \widehat { w } = \widehat { \alpha } ^ { * } \circ \widehat { x } } \end{array} \right.$
* Differentiable: <br>
Results in Fourier domain:<br>
$\left\{\begin{aligned}
\widehat { \nabla _ { \alpha } \ell } &= \widehat { x } \circ \left( \widehat { \nabla _ { w } \ell } \right) ^ { * } \\
\widehat { \nabla _ { y } \ell } &= \frac { 1 } { n } \widehat { k } ^ { - * } \circ \widehat { \nabla _ { \alpha } \ell } \\
\widehat { \nabla _ { k } \ell } &= - \widehat { k } ^ { - * } \circ \widehat { \alpha } ^ { * } \circ \widehat { \nabla _ { \alpha } \ell } \\
\widetilde { \nabla _ { x } \ell } &= \widehat { \alpha } \circ \widehat { \nabla _ { w } \ell } + \frac { 2 } { n } \widehat { x } \circ \operatorname { Re } \left\{ \widehat { \nabla _ { k } \ell } \right\}
\end{aligned} \right.$

# A Towfold SIamese Network for Real-Time Object Tracking
SA-Siam
## Keypoints
* Semantic branch / apperance branch
* Channel attention from target activation on the semantic branch

## Approaches
* Heterogenous branches
  * Apperance branch: similarity learning:
  $h_{a}(z, X)=\operatorname{corr}\left(f_{a}(z), f_{a}(X)\right)$
  * Semantic branch: using backbone trained via image classification problem:
  $h_{s}\left(z^{s}, X\right)=\operatorname{corr}\left(g\left(\xi \cdot f_{s}(z)\right), g\left(f_{s}(X)\right)\right)$
* attention
  * Channel weights $\xi$ calculated via target heatmap
* Score map aggregation at test time: $h\left(z^{s}, X\right)=\lambda h_{a}(z, X)+(1-\lambda) h_{s}\left(z^{s}, X\right)$
  * Grid division(3x3) + Maxpool(grid-wise) + MPL (9 neurons)

# Towards a Better Match in Siamese Network Based Visual Object Tracker
## Keypoints
* Scale&rotation
* Candidate patch

## Approaches
* Rotated candidate search patch
  * Only change one property per frame
* Spatial mask for extreme aspect ratio
  * Exclude background influence
* Moving average upadate template
