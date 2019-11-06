# YOLACT: Real-time Instance Segmentation
## Keypoints
* fast, one-stage instance segmentation model
  * as SSD/YOLO in detection
* two parallel tasks
  * non-local prototype mask
    * distributed repr. (cross-category)
  * linear combination per Instance
* general paradigm
  * prototypes + linear combination
  * dictionary learning

## Approaches
* Regionale(reasons)
  * one-stage
  * conv: good at spatially coherent task
  * fc: good at semantic vector(for linear combination)
* Parallel task I: protonet
  * image-sized prototype mask
  * FCN + FPN, then upsample to image size
  * output k channels of 1/4 downsampled
    * use ReLU for purpose to overpower
    * interploate back by bilinear
* Parallel task II: prediction head
  * prediction per anchor:
    * 4 bbox regressor
    * c classifiers
    * k mask coefficient
      * use tanh for purpose to be subtracted(for instance-level distinguishing)
* loss:
  * det. part: SSD
  * seg. part: BCE
  * semantic seg. part: only training phase
* Mask cropping
  * with preserve small obejct
  * divede mask loss during training
* Translation variance
  * prototype is position-aware/translation variant
  *
* Fast NMS
  * relaxation of the supressing relation
    * suppressed detections can also supress others
  * thresholding IoU matrix
  *
* Stable prototype
*
