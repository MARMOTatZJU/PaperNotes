# Learning Correspondence from the Cycle-consistency of Time
## Keypoints
- Correspondence (inter-frame)
- Cycle consistency (backward-forward)

## Approaches
- Tracker $\mathcal{T}$: given patch@t & Image@s, output patch@s
  -  $x_{s}^{I} \times x_{t}^{p} \mapsto x_{s}^{p}$
  -  differentiable
  -  Affinity function(yield feature) -> Localizer -> Sampler
      -  STN-like

-  $\mathcal{L}=\sum_{i=1}^{k} \mathcal{L}_{s i m}^{i}+\lambda \mathcal{L}_{s k i p}^{i}+\lambda \mathcal{L}_{l o n g}^{i}$

    - $\mathcal{L}_{l o n g}^{i}=l_{\theta}\left(x_{t}^{p}, \mathcal{T}^{(i)}\left(x_{t-i+1}^{I}, \mathcal{T}^{(-i)}\left(x_{t-1}^{I}, x_{t}^{p}\right)\right)\right)$
      - cycle-consistency

    - $\mathcal{L}_{s k i p}^{i}=l_{\theta}\left(x_{t}^{p}, \mathcal{T}\left(x_{t}^{I}, \mathcal{T}\left(x_{t-i}^{I}, x_{t}^{p}\right)\right)\right)$
      - simpler than cycle consistency

    - $\mathcal{L}_{s i m}^{i}=-\left\langle x_{t}^{p}, \mathcal{T}\left(x_{t-i}^{I}, x_{t}^{p}\right)\right\rangle$
      - Neative Frobenius Inner Product, inlier loss
      - Normal supervision

    - Alignement objective: $l_{\theta}\left(x_{*}^{p}, \hat{x}_{t}^{p}\right)=\frac{1}{n} \sum_{i=1}^{n}\left\|M\left(\theta_{x_{*}^{p}}\right)_{i}-M\left(\theta_{\hat{x}_{t}^{p}}\right)_{i}\right\|_{2}^{2}$
