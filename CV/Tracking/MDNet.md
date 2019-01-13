# Learning Multi-Domain Convlutional Neural Networks for Visual Tracking
## Keypoints
* Shared representation + Domain-specific representation
* Offline training(shared layer) + Online update(domain-specific layer)
* NMS
* Seperation of domain-independent info. / domain-specific info.
* Scoring-based

Nota: Domain => a specific squence of video

## Problems
* A certain sequence of video regarded as a specific domain
* Multi-domain: a specific object possible to be targets in a part of videos and background in another part

## Approaches
### Model
VGG-M arch. (shared backbone) + fc (domain-specific)

### Online update
Tracking: randomly sampled around the previous target state
Two terms:
* Long term update: regular update
* Short term update: conducted when positive score less than 0.5

Negative sample: from short term
Candidate sampling: sampled from gaussian dist. (translation/scale)



### Bbox Regression
Using feature map from the last conv layer<br>
Single simple regressor<br>
Donnot trained online, only at the first frame<br>
