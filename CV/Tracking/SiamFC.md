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
Diplacement = k*||u-c|| to target bbox <br>
metrics derived in the embedding space
##### Restrictions
* Convnet be no padding, following the strict translation invariance

### Training
Retrieving pairs from dataset: examplar image & search image (both centered)<br>
Image extended with the mean of color per channel (RGB)<br>
Positive examples: distance within R <br>
Negative examples: distance surpassing R<br>

# Triplet Loss in Siamese Network for Object Tracking
## Keypoints
* Introduce Triplet Loss to  
  * Consideration of triplet relation: examplar / positive instance / negative instance
  * More training pairs
    * Exemplar-instance => Examplar-Positive instance-Negative instance
    * M+N => M*N
  * Stronger feedback

Nota: patch: HxWxC, feature map

## Approaches
* Similarity: $\mathcal { V }=f ( z , x ) = \phi ( z ) * \phi ( x ) + b$
* Probability: $\operatorname { prob } \left( v p _ { i } , v n _ { j } \right) = \frac { e ^ { v p _ { i } } } { e ^ { v p _ { i } } + e ^ { v n _ { j } } }$
* Triplet loss: $L _ { t } \left( \mathcal { V } _ { p } , \mathcal { V } _ { n } \right) = - \frac { 1 } { M N } \sum _ { i } ^ { M } \sum _ { j } ^ { N } \log p r o b \left( v p _ { i } , v n _ { j } \right)$
