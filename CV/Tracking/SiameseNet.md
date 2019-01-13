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
Diplacement = k*||u-c||<br>

### Training
Retrieving pairs from dataset: examplar image & search image (both centered)<br>
Image extended with the mean of color per channel (RGB)<br>
Positive examples: distance within R <br>
Negative examples: distance surpassing R<br>

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
### Training pair selection
* Positive pairs generated from detection sets through data augmentation
* Negative pairs from semantic background (same/different category)
### NMS selecting Distractor
* Distractor set:
$D : = \{\forall d _ { i } \in D , f ( z , d _ { i } ) > h \cap d _ { i } \neq z _ { t } \}$
* Objective in consideration of distractors:
$q _ { T + 1 } = \underset { p _ { k } \in \mathcal { P } } { \operatorname { argmax } } \left( \frac { \sum _ { t = 1 } ^ { T } \beta _ { t } \varphi \left( z _ { t } \right) } { \sum _ { t = 1 } ^ { T } \beta _ { t } } - \frac { \sum _ { t = 1 } ^ { T } \beta _ { t } \hat { \alpha } \sum _ { i = 1 } ^ { n } \alpha _ { i } \varphi \left( d _ { i , t } \right) } { \sum _ { t = 1 } ^ { T } \beta _ { t } \sum _ { i = 1 } ^ { n } \alpha _ { i } } \right) \star \varphi \left( p _ { k } \right)$
  * weighted by distractor $\alpha$, sparsely regularized
    * Constant value (=1) according to the author's setting
  * weighted by time $\beta$
    * $\beta_t = \sum _ { i = 0 } ^ { t - 1 } \left( \frac { \eta } { 1 - \eta } \right) ^ { i }$ with $\eta=0.01$
  * accelerated thanks to bilinear property of operation $f(z,x)$
### Local-to-global
* Detection score indicate the objectness
* Search region increase step by step
