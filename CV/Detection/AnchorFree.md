# FCOS: Fully Convolutional One-Stage Object Detection
FCN+Anchor Free
## Keypoints
Motivations:
* Anchor free issues
  * low recall
  * overlapped obj. ambiguity
## Approaches
* Regression target:
    $\begin{aligned} l^{*} &=x-x_{0}^{(i)}, t^{*}=y-y_{0}^{(i)} \\ r^{*} &=x_{1}^{(i)}-x, b^{*}=y_{1}^{(i)}-y \end{aligned}$
* FPN(Retina net): multi-level to solve low recall issue & scale issues
  * attribute different scale by positive sample scale range: set as negative sample those
$\begin{aligned}&
\max \left(l^{*}, t^{*}, r^{*}, b^{*}\right)>m_{i}
\\&
\max \left(l^{*}, t^{*}, r^{*}, b^{*}\right)< m_{i-1}
\end{aligned}$
* Center-ness branch:
  * $\text { centerness }^{*}=\sqrt{\frac{\min \left(l^{*}, r^{*}\right)}{\max \left(l^{*}, r^{*}\right)} \times \frac{\min \left(t^{*}, b^{*}\right)}{\max \left(t^{*}, b^{*}\right)}}$
  * Soft threshold

# Objects as Keypoints
Detection as keypoints
## Keypoints
* Inefficiency of enumeration of location&classification
* Inefficiency of post-processing
* Generalizable: 3D detection, Human pose estimation
## Approaches
* Backbone
  * Deep Layer Aggregation
* Loss
  * Classification: Logistics regresion (soft)
    * Gaussian label
      * Center point as positive
      * Others regarded as soft negative
  * Offset&size regression: L1
* Post-proc
  * Peak value point within 3x3 field:
    * topk-100
    * implementable with maxpool 3x3
    * serve as NMS alternative
