# Momentum Contrast for Unsupervised Visual Representation Learning
MoCo,Kaiming

## Keypoints
* __Contrastive learning__: training an encoder for a __dictionary look-up__ task
  * _large_ & _consistent_ dictionary
  * reduce gap between supervised&Unsupervised pretraining

## Approaches
* Contrastive learning
  * formulation:
    * query&key: q & $\{k_0, k_1, ....\}$
    * embedding:
  * contrastive loss $\mathcal{L}_{q}=-\log \frac{\exp \left(q \cdot k_{+} / \tau\right)}{\sum_{i=0}^{K} \exp \left(q \cdot k_{i} / \tau\right)}$
* Motivation
  * "good features: learned by a _large dictionary_ covering a rich set of _negative samples_
  * encoder for dictionary keys: consistent during evolution
* Momentum contrast
  * Dictionary: queue of mini-batches
  * Query Encoder Param. Updating: $\theta_{\mathrm{k}} \leftarrow m \theta_{\mathrm{k}}+(1-m) \theta_{\mathrm{q}}$
    * Updateing $f_k$ using $f_q$
    * $f_q$ param ($\theta_q$) updated by BP
    * avoid BP w.r.t. whole samples in dict.
    * slowly evolving key encoder matter (experiments show)
* pretext task (pair generation)
  * same image: positive pair
    * same image under different data augmentation
    * (video: frame as data augmentation?)
  * different image: negative pair
* technical details:
  *
  * Shuffling BN:
    * use multi-GPUs
    * for keys, shuffling samples order & shuffle back after encoding/embedding
    * avoid query to use same statistics as query

## Reference
* Contrastive learning
  * Dimensionality reduction by learning an invariant mapping
* Related works
  * Unsupervised feature learning via non-parametric instance discrimination
  * Unsupervised learning of visual representations using videos
* Technical details
  * Shuffling BN: Data-efficient image recognition with contrastive predictive coding




# Unsupervised Feature Learning via Non-Parametric Instance Discrimination
## Motivation
Can we learn a
good feature representation that captures apparent similarity
among instances, instead of classes, by merely asking
the feature to be discriminative of individual instances?

## Intro
Learn appearance similarity


## Approaches
* Proxiamal regularization
  * make embedding vary slower
  * $-\log h\left(i, \mathbf{v}_{i}^{(t-1)}\right)+\lambda\left\|\mathbf{v}_{i}^{(t)}-\mathbf{v}_{i}^{(t-1)}\right\|_{2}^{2}$
