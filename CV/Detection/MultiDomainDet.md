# Towards Universal Object Detection by Domain Attention
Domain-attentive blocks
## Keypoints
* Cross-domain detection not pratical
* Lack of domain adaptation measures
* Channel attention for domain adaptation


## Approaches
* DA Blocks
  * SE adapter bank:
  $\mathbf{X}_{U S E}=\left[\mathbf{X}_{S E}^{1}, \mathbf{X}_{S E}^{2}, \ldots, \mathbf{X}_{S E}^{N}\right] \in \mathbb{R}^{C \times N}$
  SE denotes squeeze-excitation
  * Domain-Attentative Attention for soft assignment:
  $\begin{aligned}
  \mathbf{S}_{D A}&=\mathbf{F}_{D A}(\mathbf{X})=\operatorname{softmax}\left(\mathbf{W}_{D A} \mathbf{F}_{a v g}(\mathbf{X})\right)
  \\
  \mathbf{X}_{D A}&=\mathbf{X}_{U S E} \mathbf{S}_{D A} \in \mathbb{R}^{C \times 1}
  \end{aligned}$
