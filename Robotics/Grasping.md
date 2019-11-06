# Real-Time Grasp Detection Using Convolutional Neural Networks
## Keypoints
* single-stage regression

## Approaches
*
* orientation repr.: $(\sin 2 \theta, \cos 2 \theta)$
* Assumption:
  * image contain only one obejct/grasp
* Training
  * change grasps of objects to avoid overfitting
  *
* MultiGrasp
  * NxN grid
  * assuming at most one grasp per grid cell
* Rectangle metric
  * The grasp angle is within 30 of the ground truth grasp.
  * The Jaccard index of the predicted grasp and the ground truth is greater than 25 percent


# Real World, Real-Time Robotic Grasping with Convolutional Neural Networks
## Keypoitns
* Physical implementation


# Robotic Grasp Detection using Deep Convolutional Neural Networks
## Keypoints
* Two branches for diff. modality

# Learning Hand-Eye Coordination for Robotic Grasping with Deep Learning and Large-Scale Data Collection
Arm forest

# S4G: Amodal Single-view Single-Shot SE(3) Grasp Detection in Cluttered Scenes
## Keypoints
* Single-shot proposal network
  * Pointnet++
* Synthetic data
  * YCB / Amazon
*

## Approaches
* Grasp configuration
  * $c = (h, s_h)$
    * $h \in \mathbb{SE}(3)$
    * $h_s \in \mathbb{R}$
* Data synthesis
  * YCB
  * format:
    * $\mathbb{SE}(3)$ pose
    * Grasp qualiry scoring
      * antipodal / collision / occupancy / robustness
      * $s_{\mathbf{h}}=\min _{j}\left[s_{\mathbf{h}_{j}}^{a} s_{\mathbf{h}_{j}}^{o} s_{\mathbf{h}_{j}}^{c}\right], \quad \mathbf{h}_{j}=\exp (\hat{\xi}) \mathbf{h}$
  * Graphics based generation
    * MuJoCo engine / V-HACD
* Single-shot grasp generation
  * Scene Cloud point: Pointnet++
    * Point Set Features
    * Set Feature Back Propagation
      * inverse distance interpolation / skip links
    * prediction output:
      * 6-DoF grasp pose
      * grasp quality score
    * one prediction per cloud point
  * 6D representation of 3D rotation matrix
    * rotation loss (3 DoF)
      * L2 regression
      * consider symetric solution
    * translation loss (3 DoF)
      * L2 regression
    * quality
      * classification, CE loss
  * NMS based on local maximum of robustness score $h_s$
    * give executable grasp $\mathcal{H}$
  * random sampling based on grasp quality score
