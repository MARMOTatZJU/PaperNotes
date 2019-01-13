# Deep Visual Tracking: Review
## Metholodgy

### Network structure
#### CNN-C
Classification paradigm
* Multi-scale feature map
* DeepSRDCF: feature map + SRDCF
* RPNT: Faster-RNN-like + online-SVM
* Saliency map + online-SVM
#### CNN-M
Matching paradigm
* SiamFC: fully-connected siamese, match templ & search region via ConvNet
* correlation filter
* fourrier domain
#### RNN
Sequence modeling
* Spatial correlation
  * RNN procude confidence map + correlation filter
  * RTT method: apply RNN features to CNN (saliency map)
* Temporal correlation
  * LSTM concat high level signal
#### Others
* Auto-encoder
* Complex-valued

### Network function
Network output
* FEN: feature extraction network + location
  * FEN-SL: single layer
    * e.g. DeepSRDCF
  * FEN-ML: multiple layer
    * e.g. hierarchical convolutional features, RTT(recurrent), C-COT
* EEN: end-to=end network for fe & cand. eval.
  * EEN-S: score, for every candidate
    * e.g. ADNet, SINT, DeepTrack
  * EEN-M: confidence map/heat map (via fully-convolutional)
    * e.g. SiamFC
  * EEN-B: bbox
    * e.g. GOTURN

### Network training
* Pretrain or not: no pre-trn. / image pre-trn. / video pre-trn.
* Online or not: online / offline

Examples:

* VP-NOL: SiamFC,
* VP-OL: MDNet

## Supplementary Part
### Variants of Siamese Net
#### TriHard loss in Siames
#### SiamRPN
#### Distractor-aware Siamese
### MDNet
Multi-domain
#### MDNet
#### RT-MDNet
Real time
### D2T
Detection to Tracking
