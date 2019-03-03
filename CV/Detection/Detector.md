# Fast R-CNN
ROI pooling (for fixed size ROI) + cls/reg branch to produce bbox (a.k.a. head)

# Faster R-CNN
RPN (Region Proposal Net) + Fast R-CNN
- Fixed size ROI pooling (conv 3x3)

# Feature Pyramid Net

# Grid R-CNN
Localize grid point via heat maps

## Approaches
* Heat maps for each grid point generated via deconvolution & dilation
* 1 grid point => 1 heat map / feature map
* Heat map trained in classification manner
* Highest response as grid point
* Boundaries calculated confidence weighted avg of grid points on the edge
* Extended region mapping to overcome grid point coverage problem
