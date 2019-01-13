# Focal Loss
Paper: [Focal Loss for Dense Object Detection](https://arxiv.org/pdf/1708.02002.pdf)<br>
## Target problem & Analysis
* Class imbalance
  * Pos./Neg. example ratio
  * Non-trival loss values of easy cases

## Proposed approaches
### Focal loss
$\rm{FL}(p_t)=s-(1-p_t)^{\gamma}\log(p_t)$, where:
- $p_t$: confidence of g.t. class (obj./bg.),
$p_t=
\begin{cases}
p,\text{if positive sample}
\\
1-p,\text{if negative sample}
\end{cases}$
### Retina net
* Retina_Net = Feature_Pyramid_Net + Focal_Loss
  * Backbone = ResNet + FPN
