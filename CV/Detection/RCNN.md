# Double-Head RCNN: Rethinking Classification and Localization for Object Detection
## Keypoints
* fc-head for classification
* cnov-head for regression
* complementary(unfocused) tasks

## Approaches
* Head structure
  * common head: conv7x7:256
  * fc-head: fc:1024 -> fc:1024 -> class
  * conv-head: conv7x7:1024 -> conv7x71024: -> box avgpool:2014
* Overall objective: $\begin{equation}
\mathcal{L}=\omega^{f c} \mathcal{L}^{f c}+\omega^{c o n v} \mathcal{L}^{c o n v}+\mathcal{L}^{r p n}
\end{equation}$
* Complementary loss
  * fc-head loss: $\begin{equation}
  \mathcal{L}^{f c}=\lambda^{f c} L_{c l s}^{f c}+\left(1-\lambda^{f c}\right) L_{r e g}^{f c}
  \end{equation}$
  * conv-head loss: $\begin{equation}
  \mathcal{L}^{c o n v}=\left(1-\lambda^{c o n v}\right) L_{c l s}^{c o n v}+\lambda^{c o n v} L_{r e g}^{c o n v}
  \end{equation}$
* Complementary Fusion of classifiers
  * $\begin{equation}
  s=s^{f c}+s^{c o n v}\left(1-s^{f c}\right)=s^{c o n v}+s^{f c}\left(1-s^{c o n v}\right)
  \end{equation}$
