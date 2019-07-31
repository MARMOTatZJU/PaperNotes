# Acquisition of Localization Confidence for Accurate Object Detection
IoU-Net
## Keypoints
* IoU prediction module
  * absence of "localization confidence" in former detectors
* IoU refinement module
  * optimization-base
## Approaches
* IoU branch
  * head of FC of the ROI after precise ROI pooling
* IoU guided NMS
  * retrieve bbox with highest IoU confidence at the beginning of every iter
  * while suppressing overlapped bbox, replace the choosed bbox's original cls. score with any encountered higher cls. score
    * confidence clustering
    * correction of the confidence score
* IoU refinement
  * optimization-based
  * maximization of predicted IoU
* Precise RoI pooling
  * $\frac{\int_{y_{1}}^{y_{2}} \int_{x_{1}}^{x_{2}} f(x, y) d x d y}{\left(x_{2}-x_{1}\right) \times\left(y_{2}-y_{1}\right)}$
    * for bin $\{(x_1,y_1), (x_2,y_2)\}$
    * continous, differentiable
