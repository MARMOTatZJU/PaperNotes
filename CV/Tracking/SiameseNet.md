# Deeper and Wider Siamese Networks for Real-Time Visual Tracking
D&W
## Keypoints
* Modern backbone drawback
  * Large receptive field lack localits precision
  * Padding
* New residual module to resolve padding issue
  * Cropping-inside-residual (CIR) unit

## Analysis

* Stride: effect negative
* Receptive field: 60%-80% of the input image
* Padding: negative effect

### Summarized guidlines
* Small stride
* receptive field: 60%-80% of input image
* Avoid padding

## Approaches
### Cropping-Inside Residual
* Bottleneck block w/o downsampling:
  * crop one at border after addition
* Bottleneck block w/t downsampling:
  * crop one at border after addition, then maxpool@s2 instead of s2 in Bottleneck
* Inception / ResNeXt
  * crop after concatenation/addition
### Proposed arch.
* Greater depth => less stage / less downsimpling / less stride

# VITAL: VIsual Tracking via Adversarial Learning
GAN

## Keypoints
* Tracking-by-detection framework
  * Drawing proposal around the target object
* GAN generating masks on input feature map
* Online update

## Problem Analysis
* Higly overlapped prevent capture of appearance variation
* Imbalance of pos./neg. sample

## Approaches
* Mask feature map
* D taking as input masked features
* Get a robust D
* D serving as classifier during test stage
* For learning a datat augmentation
* Focal loss for addressing class imbalance issue
