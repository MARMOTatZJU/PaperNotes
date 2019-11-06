# Going Deeper with Convolutions
GoogLeNet v1
## Keypoints
* Deep: depth / width
* Introducing sparsity in conv/fc
  * Motivation:
    * Arora : _Provable bounds for learning some deep representations_
    * Hebbian principle
* Filter-level sparsity & layer-level density
  * e.g. clustering sparse matrices into relatively dense submatrices
    * _On two-dimensional sparse matrix partitioning: Models, methods, and a recipe._
* _there will be a smaller number of more spatially
spread out clusters that can be covered by convolutions over larger patches, and there will be a decreasing number of
patches over larger and larger regions_
## Implementation
  * Inception module
    * Using dense component(matrix multiplication, i.e. conv/fc) to approximate local sparse structure
    * Dim. reducion before large kernel
    * used in deeper layer
  * Auxilary classifier
*

# Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift
GoogLeNet v2
## Keypoints
* Covariate shift issue
  * layer input need to be continue,
  * but this not hold naturally
  * Shimodaira: _improving predictive inference under covariate shift by weighting the log-likelihood function._
* Gradient vanishing
  * large output(distribution not hold) make non-linearity layer saturate
## Approaches
* Batch normalization: $\widehat{x}^{(k)}=\frac{x^{(k)}-\mathrm{E}\left[x^{(k)}\right]}{\sqrt{\operatorname{Var}\left[x^{(k)}\right]}}$
* Element-wise affine: $y^{(k)}=\gamma^{(k)} \widehat{x}^{(k)}+\beta^{(k)}$
* Algorithm:
  $\begin{aligned} \mu_{\mathcal{B}} & \leftarrow \frac{1}{m} \sum_{i=1}^{m} x_{i} \\ \sigma_{\mathcal{B}}^{2} & \leftarrow \frac{1}{m} \sum_{i=1}^{m}\left(x_{i}-\mu_{\mathcal{B}}\right)^{2} \\ \widehat{x}_{i} & \leftarrow \frac{x_{i}-\mu_{\mathcal{B}}}{\sqrt{\sigma_{\mathcal{B}}^{2}+\epsilon}} \\ y_{i} & \leftarrow \gamma \widehat{x}_{i}+\beta \equiv \mathrm{B} \mathrm{N}_{\gamma, \beta}\left(x_{i}\right) \end{aligned}$


# Deep Layer Aggregation
DLA
## Keypoints
* __Aggregation__ matters
  * avoid barely seeking for deeper arch.

## Approaches
* Stage(by resolution)/Block/Layer
  * $x_i$: stage/block feature
  * $N$: aggregation node
* Iterative Deep Aggregation(IDA)
  * $\begin{equation}
  I\left(\mathrm{x}_{1}, \ldots, \mathrm{x}_{n}\right)=\left\{\begin{array}{ll}{\mathrm{x}_{1}} & {\text { if } n=1} \\ {I\left(N\left(\mathrm{x}_{1}, \mathrm{x}_{2}\right), \ldots, \mathrm{x}_{n}\right)} & {\text { otherwise }}\end{array}\right.
  \end{equation}$
* Hierarchical Deep Aggregation(HDA)
  * $\begin{equation}
  \begin{aligned} T_{n}(\mathbf{x})=N( & R_{n-1}^{n}(\mathbf{x}), R_{n-2}^{n}(\mathbf{x}), \ldots, \\ R_{1}^{n}(\mathbf{x}), L_{1}^{n}(\mathbf{x}), &\left.L_{2}^{n}(\mathbf{x})\right) \end{aligned}
  \end{equation}$
  * $\begin{equation}
  \begin{aligned} L_{2}^{n}(\mathrm{x}) &=B\left(L_{1}^{n}(\mathrm{x})\right), \quad L_{1}^{n}(\mathrm{x})=B\left(R_{1}^{n}(\mathrm{x})\right) \\ R_{m}^{n}(\mathrm{x}) &=\left\{\begin{array}{ll}{T_{m}(\mathrm{x})} & {\text { if } m=n-1} \\ {T_{m}\left(R_{m+1}^{n}(\mathrm{x})\right)} & {\text { otherwise }}\end{array}\right.\end{aligned}
  \end{equation}$
  * feature feed back to backbone
  * aggregation node merge
* Aggregation
  * basic: $\begin{equation}
  N\left(\mathrm{x}_{1}, \ldots, \mathrm{x}_{n}\right)=\sigma\left(\text { BatchNorm }\left(\sum_{i} W_{i} \mathrm{x}_{i}+\mathrm{b}\right)\right)
  \end{equation}$
  * residual: $\begin{equation}
  N\left(\mathrm{x}_{1}, \ldots, \mathrm{x}_{n}\right)=\sigma\left(\text { BatchNorm }\left(\sum_{i} W_{i} \mathrm{x}_{i}+\mathrm{b}\right)+\mathrm{x}_{n}\right)
  \end{equation}$
    * with $x_n$ as skip connection
    * good for high level hierarchy, bad for shallw level hierarchy
* Overall designs
  * IDA over HDA
* Interpolation/upsample
  * IDA
