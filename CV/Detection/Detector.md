# Grid R-CNN
Localize grid point via heat maps
## Keypoints

## Approaches
* Heat maps for each grid point generated via deconvolution & dilation
* 1 grid point => 1 heat map / feature map
* Heat map trained in classification manner
* Highest response as grid point
* Boundaries calculated confidence weighted avg of grid points on the edge
