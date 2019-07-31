# High Performance Visual Tracking with Siamese Region Proposal Networks
SiamRPN
## Keypoints
* Offline trained in pairs
* SiameseNet + RPN
* One-shot det. + trival conv. during inference phase
  * Meta-learning (learning to learn)
* Two branch
  * Classifier
  * Regressor

## Approaches
### Training phase
* Feature exxtraction part same as SiamFC
* Examplar image: $\varphi(z)$ =[ConvNet]=> $\begin{cases}[\varphi(z)]_{cls}\text{(4x4x2kx256)} \\ [\varphi(z)]_{reg}\text{(16x16x256)}  \end{cases}$  <br>
Search image: $\varphi(x)$ =[ConvNet]=> $\begin{cases}[\varphi(x)]_{cls}\text{(4x4x4kx256)}
\\ [\varphi(x)]_{reg}\text{(16x16x256)}  \end{cases}$
* Classification & Regression via Convolution opr. (along H & W axis )<br>
Anchor map:
$\begin{aligned} A _ { w \times h \times 2 k } ^ { c l s } & = [ \varphi ( x ) ] _ { c l s } \star [ \varphi ( z ) ] _ { c l s } \\ A _ { w \times h \times 4 k } ^ { r e g } & = [ \varphi ( x ) ] _ { r e g } \star [ \varphi ( z ) ] _ { r e g } \end{aligned}$
* Positive / negative pairs
  * Two thresholds: $th_{hi}$ and $th_{lo}$


### Prediction phase
* One-shot detection interpreted as Meta-Learning
* Proposal selection
  * Only select anchors within the center square
  * Cosine window on cls score
  * Add temporal penalty (supress rapid change):
  $\text{Penalty}= e ^ { k * \max \left( \frac { r } { r ^ { \prime } } , \frac { r ^ { \prime } } { r } \right) * \max \left( \frac { s } { s ^ { \prime } } , \frac { s ^ { \prime } } { s } \right) }$
  * Cropping method: given target bbox shaped of (w, h), crop size A: $(w+p)\times(h+p)=A^2$ with padding $p=\frac{w+h}{2}$
  * NMS after re-ranking with new scores to get the final bbox of target

# Distractor-aware Siamese Networks for Visual Object Tracking
DaSiamRPN
## Keypoints
* Distractors: semantic backgrounds
* Sampling strategy
* Local-to-global search (long-term tracking)

## Problems
* Semantic backgrounds learned rather than instance-level info. via conventional method
  * Imbalance btw semantic & non-semantic in background
* Online update within Siamese framework

## Distractor aware Training
### Analysis of issues
* Diverse cat. of pos. pairs favorable for generalization:

## Approaches
### Training pair selection （neg pair
* Positive pairs generated from detection sets through data augmentation
* Negative pairs from semantic background (same/different category)
### NMS selecting Distractor
* Distractor set:
$D : = \{\forall d _ { i } \in D , f ( z , d _ { i } ) > h \cap d _ { i } \neq z _ { t } \}$
<!-- * Objective in consideration of distractors:<br> -->
Original version: $q = \underset { p _ { k } \in \mathcal { P } } { \operatorname { argmax } } f \left( z , p _ { k } \right) - \frac { \hat { \alpha } \sum _ { i = 1 } ^ { n } \alpha _ { i } f \left( d _ { i } , p _ { k } \right) } { \sum _ { i = 1 } ^ { n } \alpha _ { i } }$<br>
Final version: $q _ { T + 1 } = \underset { p _ { k } \in \mathcal { P } } { \operatorname { argmax } } \left( \frac { \sum _ { t = 1 } ^ { T } \beta _ { t } \varphi \left( z _ { t } \right) } { \sum _ { t = 1 } ^ { T } \beta _ { t } } - \frac { \sum _ { t = 1 } ^ { T } \beta _ { t } \hat { \alpha } \sum _ { i = 1 } ^ { n } \alpha _ { i } \varphi \left( d _ { i , t } \right) } { \sum _ { t = 1 } ^ { T } \beta _ { t } \sum _ { i = 1 } ^ { n } \alpha _ { i } } \right) \star \varphi \left( p _ { k } \right)$
  * weighted by distractor $\alpha$, sparsely regularized
    * Constant value (=1) according to the author's setting
  * weighted by time $\beta$
    * $\beta_t = \sum _ { i = 0 } ^ { t - 1 } \left( \frac { \eta } { 1 - \eta } \right) ^ { i }$ with $\eta=0.01$
  * accelerated thanks to bilinear property of operation $f(z,x)$
### Local-to-global
* Detection score indicate the objectness
* Search region increase step by step

# SiamRPN++: Evolution of Siamese Visual Tracking with Very Deep NEtwoorks
SiamRPN++
## Keypoints
* Effect of violation of strict translation invariance
* Introduction of ResNet by sampling strategy
* Layer wise feature aggregation structure (multiple layer features)
* Depthwise correlation structure

## Analysis
### Padding restrictions
* Strict translation restrictions for backbone
* Strong spatial bias towards central point on the feature map if backbone with Padding
* Random translation (spatial aware sampling) in the data aug alleviate this bias

## Approaches
* Translation data augmentation
* Depthwise correlation
* Layer-wise aggregation
  * Same spatial resolution
  * At the level of cls/reg outputs
* Finetuning of the backbone

# Siamese Cascaded Region Proposal Networks for Real-Time Visual Tracking
C-RPN
## Keypoints
* Cascaded RPN
  * Feature Transfering Block (FTB)
* Explore low-level spatial features (feature-pyramid like)
* Multiple level regression, better precision

## Approaches
* Cascades:
$\begin{aligned} \Phi _ { l } ( \mathbf { z } ) & = \mathrm { FTB } \left( \Phi _ { l - 1 } ( \mathbf { z } ) , \varphi _ { l } ( \mathbf { z } ) \right) \\ \Phi _ { l } ( \mathbf { x } ) & = \operatorname { FTB } \left( \Phi _ { l - 1 } ( \mathbf { x } ) , \varphi _ { l } ( \mathbf { x } ) \right) \end{aligned}$<br>
with $\begin{cases}
\Phi _ { l }: \text{fused features}
\\
\varphi _ { l }: \text{feature from siamese net}
\end{cases}$
* Multiple stage classifiers & regressors (Faster-RCNN type)
* FTB: fuse feature of multiple-levels (like feature pyramid, bottom-up/top-down)
  * Deconv. the fm from former state for spatial dimension alignement with fm from siamese net
  * Elem-add
  * Interp. fused feature map to unify the spatial resolution (for RPN)

# SPM-Tracker: Series-Parallel Matching for Real-Time Visual Object Tracking
## Keypoints
* Robustness for Coarse Matching
* Discrimination for Fine Matching

## Approaches
* CM
  * 
* FM
  * ROI align to crop feature
  * Relation network for fine match
