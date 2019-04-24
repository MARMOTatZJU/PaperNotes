# Spatial Transformer Networks
STN

## Approches
### Transform
$\left( \begin{array} { c } { x _ { i } ^ { s } } \\ { y _ { i } ^ { s } } \end{array} \right) = \mathcal { T } _ { \theta } \left( G _ { i } \right) = \mathrm { A } _ { \theta } \left( \begin{array} { c } { x _ { i } ^ { t } } \\ { y _ { i } ^ { t } } \\ { 1 } \end{array} \right) = \left[ \begin{array} { c c c } { \theta _ { 11 } } & { \theta _ { 12 } } & { \theta _ { 13 } } \\ { \theta _ { 21 } } & { \theta _ { 22 } } & { \theta _ { 23 } } \end{array} \right] \left( \begin{array} { c } { x _ { i } ^ { t } } \\ { y _ { i } ^ { t } } \\ { 1 } \end{array} \right)$

### Sampler
Sampling with kernel
$V _ { i } ^ { c } = \sum _ { n } ^ { H } \sum _ { m } ^ { W } U _ { n m } ^ { c } k \left( x _ { i } ^ { s } - m ; \Phi _ { x } \right) k \left( y _ { i } ^ { s } - n ; \Phi _ { y } \right) \forall i \in \left[ 1 \ldots H ^ { \prime } W ^ { \prime } \right] \forall c \in [ 1 \ldots C ]$
