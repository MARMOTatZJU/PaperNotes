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
* Scale assignment
  * 0, 64, 128, 256, 512,  $\infty$ for P3 - P7
* Center-ness branch:
  * $\text { centerness }^{*}=\sqrt{\frac{\min \left(l^{*}, r^{*}\right)}{\max \left(l^{*}, r^{*}\right)} \times \frac{\min \left(t^{*}, b^{*}\right)}{\max \left(t^{*}, b^{*}\right)}}$
  * Soft threshold

# Objects as Keypoints
Detection as keypoints
## Keypoints
* Inefficiency of enumeration of location&classification
* Inefficiency of post-processing
* Generalizable: 3D detection, Human pose estimation
* Efficient grouping strategy
## Approaches
* Backbone
  * Deep Layer Aggregation
* Head
  * Keypoint estimation:
    * gt attribution:
    $\begin{aligned}&
    Y_{x y c}=\exp \left(-\frac{\left(x-\tilde{p}_{x}\right)^{2}+\left(y-\tilde{p}_{y}\right)^{2}}{2 \sigma_{p}^{2}}\right)
    \\&
    L_{k}=\frac{1}{N} \sum_{x y c} \left\{\begin{array}{ll}{\left(1-\hat{Y}_{x y c}\right)^{\alpha} \log \left(\hat{Y}_{x y c}\right)} & {\text { if } Y_{x y c}=1} \\ {\left(1-Y_{x y c}\right)^{\beta}\left(\hat{Y}_{x y c}\right)^{\alpha}} {\log \left(1-\hat{Y}_{x y c}\right)} & {\text { otherwise }} \end{array}\right.
    \end{aligned}$
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


# FoveaBox: Beyond Anchor-based Object Detector
## keypoints
* Without predefined shape settings
## Approaches
* Scale assignment
  * $\left[S_{l} / \eta^{2}, S_{l} \cdot \eta^{2}\right]$
* Fovea area
  * Only use the __center region__(shrunked gt bbox) of gt bbox for classification&Regression:
  $\begin{aligned} x_{1}^{\prime \prime} &=c_{x}^{\prime}-0.5\left(x_{2}^{\prime}-x_{1}^{\prime}\right) \sigma_{1} \\ y_{1}^{\prime \prime} &=c_{y}^{\prime}-0.5\left(y_{2}^{\prime}-y_{1}^{\prime}\right) \sigma_{1} \\ x_{2}^{\prime \prime} &=c_{x}^{\prime}+0.5\left(x_{2}^{\prime}-x_{1}^{\prime}\right) \sigma_{1} \\ y_{2}^{\prime \prime} &=c_{y}^{\prime}+0.5\left(y_{2}^{\prime}-y_{1}^{\prime}\right) \sigma_{1} \end{aligned}$

# DuBox: No-Prior Box Objection Detection via Residual Dual Scale Detectors

## Keypoints
* traditional paradigm: detector + anchor
  * Detectors of too much scales
    * Independence between diff. scales
  * detect with too much anchors
* two scales detector

## Approaches
* Hooks: "anchor points" on the output feature map
* Classification:
  * Circle range
    * $\begin{aligned}&
    \left(i-\left(x_{2}+x_{1}\right) / 2\right)^{2}+\left(j-\left(y_{2}+y_{1}\right) / 2\right)^{2} \leqslant r^{2}
    \\&
    r=\sqrt{\left(x_{2}-x_{1}\right)^{2}+\left(y_{2}-y_{1}\right)^{2}} / p
    \end{aligned}$
    * p: predifend for controlling the numbers large/small objects
* Regression
  * Densebox-like encoding/decoding
    * Encoding: $\begin{array}{c}{\Delta w_{1}=i-x_{1}, \Delta w_{2}=x_{2}-i} \\ {\Delta h_{1}=j-y_{1}, \Delta h_{2}=y_{2}-j}\end{array}$
    * Deconding: $\begin{aligned} x_{1} &=\left(i-P r_{\Delta w_{1}}\right) s, x_{2}=\left(i+P r_{\Delta w_{2}}\right) s \\ y_{1} &=\left(j-P r_{\Delta h_{1}}\right) s, y_{2}=\left(j+P r_{\Delta h_{2}}\right) s \end{aligned}$
* Residual dual scale detector
  * high-level (32/64) det. learning residual of low-level (8/16) det.
  * Mixing of feature maps from diff. levels
    * deconv./addition used for fm alignment
    * 8s+16s, 32s+64s
  * Refine module
    * Spatial/channel attention
  * Bridge module
    * low level -> high level
  * IoU strapped cls. losses

    $\begin{aligned}&
     L_{c l s} =-\sum_{i, j \in \Theta} C E\left(\boldsymbol{P r}_{c l s}^{i, j}, T_{c l s}^{i, j}\right) \sigma\left(\boldsymbol{P} r_{b b o x}^{i, j}, G t_{b b o x}^{i, j}\right) -\sum_{i, j \notin \Theta} C E\left(\boldsymbol{P} \boldsymbol{r}_{c l s}^{i, j}, T_{c l s}^{i, j}\right)
     \\&
     \sigma\left(P r_{b b o x}^{i, j}, G t_{b b o x}^{i, j}\right)=\left\{\begin{array}{ll}{1} & {\text { if } \operatorname{IoU}\left(P r_{b b o x}^{i, j}, G t_{b b o x}^{i, j}\right)>\epsilon} \\ {0} & {\text { elsewise. }}\end{array}\right. \text{(IoU gate)}
     \end{aligned}$
   * Differentiaion for diff. scales
     * Diff. positive range:
       * detector 1(small obj.): $p=9$ & $r=\min (r, 3)$
       * detector 2(large obj.): $p=10$
     * Diff. scale weight
       * $L_{\text {detector}_{1}}=\sum_{i, j \in \Theta} \lambda_{b b o x}^{i, j} L_{b b o x}^{i, j}+\lambda_{c l s} L_{c l s}$ where $\lambda_{b b o x}^{i, j}$: bbox occupy 0.3+ of the orig. img.
