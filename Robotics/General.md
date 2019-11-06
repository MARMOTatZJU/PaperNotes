# Review of Deep Learning Methods in Robotic Grasp Detection



## Background

Kinematic models
* open loop
* cons
  * complex analytical approach to form the solution
  * deriving based on models: require many data
  * dynamic actuators: impossible to model physics

Empirical method
* learn from demonstration
* cons
  * limited to demonstrated tasks

generalised robotics task
* Unstructured nature: challenging

Overview (sub-systems):
* __Grasp detection__: To detect grasp poses from images of the objects in their image plane coordinates
  * key entry point for any robotic grasping research
* __Grasp planning__: To map the detected image plane coordinates to the world coordinates
* __Control__: To determine the inverse kinematics solution of the previous sub-system



## Grasp detection
different definitions for successful grasp configurations

Overall:
detection accuracy: 90%+
detection success rate: 70%+

### Grasp representation

* Point repr.: $g=(x,y,[z])$
  * where to grasp
* Rectangle repr.(most commonly used)
  * Oriented rectangle repr.: $G=(x,y,z,\alpha,\beta,\gamma,l)$
    * 7 DoF
    * grasping point: $(x,y,z)$
    * grasping orientation: $(\alpha,\beta,\gamma)$
    * gripper opening width: $(l,)$
    * _Efficient grasping from RGBD images: Learning using a new rectangle representation_ by Jiang et al.
  * Simplified oriented rectangle repr.:
    * 5 DoF
    * boudning-box + orientation
    * _Deep Learning for Detecting Robotic Grasps_ by Lenz et al. [to be explored]
    * _Real-time grasp detection using convolutional neural networks_ by Redmon et al.
    * _Robotic grasp detection using deep convolutional neural networks_ by Kruma et al.
  * depth info. controlled in specific cases
* Location + orientation: $(x,y,[z],\theta)$
  * remove dimension $(h,w))
  * _Supersizing self-supervision: Learning to grasp from 50K tries and 700 robot hours_, xy\theta
  * _More Than a Feeling: Learning to Grasp and Regrasp using Vision and Touch_, xyz\theta
* Pixel-wise grasp representations
  * mask activation -> anthropomorphic grasping points
  * grasping points for index & thumb
  * _Associating grasp configurations with hierarchical features in convolutional neural networks_
* Dense captioning
  * using textual description


### Grasp detection method
* Analytical method
  * _Robotic grasping and contact: A review_ by Bicchi
  * assume that parameters are known
    * impossile fofr generalised solution
* Empirical method
  * data-driven
  * rely on
    * data of successful results(trials?)
    * simulation on real systems
  * Categories:
    * 2-stage: grasp detection + grasp planning
    * fully e2e: visuomotor control / image-to-action
* Category
  * Robotic Grasping
    * Grasp Detection-based(detection+planning)
      * structured output: grasp repr.
      * grasp robustness(predicted by function)
    * Visuomotor control, image-to-action
      * NVIDIA Isaac
#### Sliding window Methods
_Deep Learning for Detecting Robotic Grasps_ by Lenz et al.
* categories
  * iterative scanning
    * image patch contains potential grasp or not
  * reference rectangle method
    * adapted from Region Proposal Network
    * location detected by sliding window
* Cons
  * repetitive method, unsuitable for real-time grasp detection

#### One-shot Detection Methods
_Real-time grasp detection using convolutional neural networks_ by Redmond
* structured grasp output
* transfer Learning
* outperform sliding window
* assume at least one graspable object
  * opposed to iterative scanning
* orientation repr.
  * $(\sin 2 \theta, \cos 2 \theta)$
  * two dimensions

* orientation
  * classify orientation degree
  * classify anchor with diff. orientation
#### Robustness function
* grasp probability
* useful in case of partial information

_Learning to Plan Robust Grasps with Synthetic Point Clouds and Analytic Grasp Metrics_ by Mahler
* Dex-Net 2.0
  * dataset with cloud points
* CNN predicting robustness
  * Grasp Quality CNN (GQ-CNN)

#### Visuomotor control policy

_Learning Deep Policies for Robot Bin Picking by Simulating Robust Grasping Sequences_ by Mahler
* transfer learning
* based on _Learning to Plan Robust Grasps with Synthetic Point Clouds and Analytic Grasp Metrics_ by Mahler

_Towards Vision-Based Deep Reinforcement Learning for Robotic Motion Control._ by Zhang
* RL, DQN

#### Summary
* Structured: one-shot detection achieved SotA results

## Training Data

chanllenges
* need for large volume of training dataset
  * generate / use simulated data
* lack of domain specific data

### multi-modal
RGB-D
tactile sensing

### Pre-compiled datasets
Cornell Grasp dataset
Washington RGB-D dataset
### Collected datasets
_Supersizing self-supervision: Learning to grasp from 50K tries and 700 robot hours_ by Pinto
* predict centre points for grasps from a policy learned using reinforcement learning
* Mixture of Gaussians (MOG) background subtraction that identified graspable regions

_Learning hand-eye coordination for robotic grasping with deep learning and large-scale data collection_ by Levine et al.
* arm forest

### Domain Adaptation & Simulated Data
* grasp wrench space (GWS) analyses
* Dex-Net 2.0
  * analytic grasp quality metrics


## CNN for Grasp Detection
Analytical approaches
* impossiple to have generalised grasping model using just analytical data

### Methods
_Real-time grasp detection using convolutional neural networks_ by Redmon
* one-shot detection

_Real World, Real-Time Robotic Grasping with Convolutional Neural Networks_ by Watson
* follow Redmond

_Robotic grasp detection using deep convolutional neural networks_ by Kumra
* two branch for RGB & Depth

_Deep Learning for Detecting Robotic Grasps_ by Lenz
* cascaded CNN
* sliding window

### Evaluation

Rectangle metric (by Redmon)
* Difference between the grasp angles to be less than 30
* Jacquard index between the two grasps to be less than 25%

_Real World, Real-Time Robotic Grasping with Convolutional Neural Networks_ by Watson
* online evaluation

## Conclusion
* Sliding window: was most popular
  * superseded by one-shot det.
* One-shot detection: current SotA

## Future Work
* Large-scale datasets
  * _data simulation_
  * collection of data via _Reinforcement Learning_
    * time-consuming
* Visuomotor control policies via Reinforcement Learning
  * limited due to _precautionary steps_ to prevent damage
  * training RL alog. in _simulated environment_

Nota.
one-shot在这篇文章里应该为single-shot
