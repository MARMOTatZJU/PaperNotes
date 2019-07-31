# DARTS: DIFFERENTIABLE ARCHITECTURE SEARCH
## Keypoints
* Relaxation of search space
* Optimize structure with mixing probability
* Induction of arch. based on probability
## Approach
* Node: hidden state
* Take two previous hidden _layers_' outputs
* Operation Relaxation:
  * Operation set:
    * Conv / MaxPool / Zero(no connection)
  * Parametrized by weights $\alpha$
    * Encode the arch.
  * $\overline{o}^{(i, j)}(x)=\sum_{o \in \mathcal{O}} \frac{\exp \left(\alpha_{o}^{(i, j)}\right)}{\sum_{o^{\prime} \in \mathcal{O}} \exp \left(\alpha_{o^{\prime}}^{(i, j)}\right)} o(x)$
* Task formulation
  * Bilevel optimization problem
  * $\begin{array}{ll}{\min _{\alpha}} & {\mathcal{L}_{v a l}\left(w^{*}(\alpha), \alpha\right)} \\ {\text { s.t. }} & {w^{*}(\alpha)=\operatorname{argmin}_{w} \mathcal{L}_{t r a i n}(w, \alpha)}\end{array}$
    * $w$ -> training
    * $\alpha$ -> validation
  * Objective approx.: $\begin{aligned} & \nabla_{\alpha} \mathcal{L}_{v a l}\left(w^{*}(\alpha), \alpha\right) \\
  \approx & \nabla_{\alpha} \mathcal{L}_{v a l}\left(w-\xi \nabla_{w} \mathcal{L}_{\operatorname{train}}(w, \alpha), \alpha\right) \\
  \text{(chain rule)}= & \nabla_{\alpha} \mathcal{L}_{v a l}\left(w^{\prime}, \alpha\right)
  -\xi \nabla_{\alpha, w}^{2} \mathcal{L}_{\operatorname{train}}(w, \alpha) \nabla_{w^{\prime}} \mathcal{L}_{v a l}\left(w^{\prime}, \alpha\right) \\
  \text{(finite difference)}\approx & \nabla_{\alpha} \mathcal{L}_{v a l}\left(w^{\prime}, \alpha\right) - \xi \frac{\nabla_{\alpha} \mathcal{L}_{t r a i n}\left(w^{+}, \alpha\right)-\nabla_{\alpha} \mathcal{L}_{t r a i n}\left(w^{-}, \alpha\right)}{2 \epsilon}
  \end{aligned}$
* Arch. deriving
  * Choose top-k highest probability operations
    * k=2 for CNN, k=1 for training

# PROXYLESSNAS: DIRECT NEURAL ARCHITECTURE SEARCH ON TARGET TASK AND HARDWARE
## Keypoints
## Approaches
* Overparametrized networks
  * Edge: set of operations
  * Arch. param: Operation weights + Binary gate
  * $\mathcal{N}\left(e=m_{\mathcal{O}}^{1}, \cdots, e_{n}=m_{\mathcal{O}}^{n}\right), \mathcal{O}=\left\{o_{i}\right\}\text{(operation set)}$
    * One-shot: $m_{\mathcal{O}}^{\text { One-Shot }}(x)=\sum_{i=1}^{N} o_{i}(x)$
    * DARTS: $m_{\mathcal{O}}^{\mathrm{DARTS}}(x)=\sum_{i=1}^{N} p_{i} o_{i}(x)=\sum_{i=1}^{N} \frac{\exp \left(\alpha_{i}\right)}{\sum_{j} \exp \left(\alpha_{j}\right)} o_{i}(x)$
  * BinaryConnect
    * Save memory
    * Gate: $g=\text { binarize }\left(p_{1}, \cdots, p_{N}\right)=\left\{\begin{array}{ll}{[1,0, \cdots, 0]} & {\text { with probability } p_{1}} \\ {} & {\dots} \\ {[0,0, \cdots, 1]} & {\text { with probability } p_{N}}\end{array}\right.$
    * "Routing": $m_{\mathcal{O}}^{\text { Binary }}(x)=\sum_{i=1}^{N} g_{i} o_{i}(x)=\left\{\begin{array}{ll}{o_{1}(x)} & {\text { with probability } p_{1}} \\ {\cdots} & {} \\ {o_{N}(x)} & {\text { with probability } p_{N}}\end{array}\right.$
    * Updating: $\frac{\partial L}{\partial \alpha_{i}}=\sum_{j=1}^{N} \frac{\partial L}{\partial p_{j}} \frac{\partial p_{j}}{\partial \alpha_{i}} \approx \sum_{j=1}^{N} \frac{\partial L}{\partial g_{j}} \frac{\partial p_{j}}{\partial \alpha_{i}}=\sum_{j=1}^{N} \frac{\partial L}{\partial g_{j}} \frac{\partial\left(\frac{\exp \left(\alpha_{j}\right)}{\sum_{k} \exp \left(\alpha_{k}\right)}\right)}{\partial \alpha_{i}}=\sum_{j=1}^{N} \frac{\partial L}{\partial g_{j}} p_{j}\left(\delta_{i j}-p_{i}\right)$
      * Only two paths (N -> 2)
  * REINFORCE-based approach
* Differentiable latency
  * Latency modeling: $\mathbb{E}\left[\text { latency }_{i}\right]=\sum_{j} p_{j}^{i} \times F\left(o_{j}^{i}\right)$
  * Latency optim.: $\partial \mathbb{E}\left[\text { latency }_{i}\right] / \partial p_{j}^{i}=F\left(o_{j}^{i}\right)$
  * Latency prediction
    * Train model to predict
    * Feature engineering:
      * type of the operator
      * input and output feature map size
      * other attributes like kernel size, stride for convolution and expansion ratio
    * Training samples drawn from search space
    * Label tested on Google Pixel 1 with TensorFlow-lite
* Final objective: $\text {Loss}=\operatorname{Loss}_{C E}+\lambda_{1}\|w\|_{2}^{2}+\lambda_{2} \mathbb{E}[\text { latency }]$
