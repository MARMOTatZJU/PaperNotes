# Why do deep convolutional networks generalize so poorly to small image transformations?
Strict translation invariance failing

## Issues
Suble changes can attack the result of modern CNN (small translation/size or form changes)

## Reasons
* Subsampling
* Convolutional: strict translation invariant
  * e.g. convlution w/o subsampling
* Shiftability: reconstructable from subsampled response
$r ( x ) = \sum _ { i } B _ { s } \left( x - x _ { i } \right) r \left( x _ { i } \right)$
* Claim: shiftable => Global pooling on response being traslation ivariant
* Dataset bias / photographer bias
  * e.g. position's distribution are not uniform

# Comparison
* Ancient arch. hold for translation invariance
* Modern arch. not
