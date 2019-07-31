# Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks
MAML
## Keypoints
* Intuition: internal representations exist


## Approaches
* Meta-learning algorithm
  * training task optim.: $\theta_{i}^{\prime}=\theta-\alpha \nabla_{\theta} \mathcal{L}_{\mathcal{T}_{i}}\left(f_{\theta}\right)$
    * $\alpha$: training optim. step
  * test task optim.: $\theta \leftarrow \theta-\beta \nabla_{\theta} \sum_{\mathcal{T}_{t} \sim p(\mathcal{T})} \mathcal{L}_{\mathcal{T}_{i}}\left(f_{\theta_{i}^{\prime}}\right)$
  * $\beta$: test optim. step
