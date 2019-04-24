# Convolution with even-sized kernels and symmetric padding
C2sp/C4sp
## Issues
- Edge effects
- Information quantity represented by mean L1 norm

## Approaches
- Even-sized kernel: $\mathcal{R}=\{(1-\kappa, 1-\kappa),(1-\kappa, 2-\kappa), \ldots,(\kappa, \kappa)\}$
- Evenly shift by channel: $\mathcal{R}_{+}=\{\mathcal{R}(1,1), \mathcal{R}(-1,1), \mathcal{R}(1,-1), \mathcal{R}(-1,-1)\}$
  - Different padding $\in\mathcal{R}_{+}$ evenly attributed to each channels
  - s.t. $\sum_{i=1}^{c_{i}} \sum_{\delta \in \mathcal{R}_{+}} \delta=(0,0)$
  - Avoid accumulation of spatial bias


# Deformable Convolutional Networks
Deformable
## Keypoints
- Address fixed geometric structure of CNN issue
- Deformable conv.: learned 2D offset to grid sampling
- Deformable ROI pooling: learned 2D offset to regular bin
- Share with STN/Deformable part net.

## Approaches
- Deformable conv
  - Conv regarded as sampling+summation(reduce)
    - Sampling: $\mathcal{R}=\{(-\kappa, -\kappa),(1-\kappa, 2-\kappa), \ldots,(\kappa, \kappa)\}$
    - Summation: $\mathbf{y}\left(\mathbf{p}_{0}\right)=\sum_{\mathbf{p}_{n} \in \mathcal{R}} \mathbf{w}\left(\mathbf{p}_{n}\right) \cdot \mathbf{x}\left(\mathbf{p}_{0}+\mathbf{p}_{n}\right)$
  - Deformable: 2D offset $\Delta \mathbf{p}_{n}$ introduced
    -  $\mathbf{y}\left(\mathbf{p}_{0}\right)=\sum_{\mathbf{p} n \in \mathcal{R}} \mathbf{w}\left(\mathbf{p}_{n}\right) \cdot \mathbf{x}\left(\mathbf{p}_{0}+\mathbf{p}_{n}+\Delta \mathbf{p}_{n}\right)$
    -  Interpolation of  $(\mathbf{p}_{0}+\mathbf{p}_{n}+\Delta \mathbf{p}_{n})$
        -  $\mathbf{x}(\mathbf{p})=\sum_{\mathbf{q}} G(\mathbf{q}, \mathbf{p}) \cdot \mathbf{x}(\mathbf{q})$,
        -  $G(\mathbf{q}, \mathbf{p})=g\left(q_{x}, p_{x}\right) \cdot g\left(q_{y}, p_{y}\right)$
    -  $\Delta \mathbf{p}_{n}$ calculated by one conv of same settings (kernel shape, dilation, etc.)
        - output feature map: same as feature output
        - output channel: 2N
- Deformable ROI pooling
  - Original: $\mathbf{y}(i, j)=\sum_{\mathbf{p} \in b i n(i, j)} \mathbf{x}\left(\mathbf{p}_{0}+\mathbf{p}\right) / n_{i j}$, with bin(i,j)
    - $\left\lfloor i \frac{w}{k}\right\rfloor \leq p_{x}<\left\lceil(i+1) \frac{w}{k}\right\rceil$
    - $\left\lfloor j \frac{h}{k}\right\rfloor \leq p_{y}<\left\lceil(j+1) \frac{h}{k}\right\rceil$
  - Deformable: $\mathbf{y}(i, j)=\sum_{\mathbf{p} \in \operatorname{lin}(i, j)} \mathbf{x}\left(\mathbf{p}_{0}+\mathbf{p}+\Delta \mathbf{p}_{i j}\right) / n_{i j}$

# Deformable ConvNets v2: More Deformable, Better Results
## Keypoints
- Address irrevalent focusing (extension out of RoI) issue
- Visual support region
- Error bounded saliency region
-

## Approaches
- More deformable conv stacked
  - applied to all conv3x3 in conv3-5 of Resnet-50
- Modulated deformable conv: $y(p)=\sum_{k=1}^{K} w_{k} \cdot x\left(p+p_{k}+\Delta p_{k}\right) \cdot \Delta m_{k}$,  Modulated deformable ROI pooling: $y(k)=\sum_{j=1}^{n_{k}} x\left(p_{k} | j+\Delta p_{k}\right) \cdot \Delta m_{k} / n_{k}$
  - Modulation term: $\Delta m_{k}$, output via seperate conv (also responsible for outputing offsets)
- Mimic learning: faster R-CNN mimic R-CNN (focus better on cropped patch)
  -  Applied to positive ROI, as negative ROI need mor content information
  - $L_{\mathrm{mimic}}=\sum_{b \in \Omega}\left[1-\cos \left(f_{\mathrm{RCNN}}(b), f_{\mathrm{FRCNN}}(b)\right)\right]$
  - mimic loss + R-CNN classification loss
