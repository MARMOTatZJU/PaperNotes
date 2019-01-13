# OTB
* Plot
  * Success plot: precision w.r.t. IoU
  * Precision plot: precision w.r.t. location error (euclidean dist. btw center location)
* Metrics based on plot
  * OP: mean overlap precision
  * DP: mean distance precision
* Overlap score/measure: IoU btw g.t. and predicted
* AOS: average overlap score
* AUC: area of success rate w.r.t. thres. of IoU

# VOT
* Accuracy: average overlap
* Robustness: number of afailures per sequence
  * Failure: overlap measure (IoU) = 0
  * Tracker reset after each failure
* EAO: Expected avaerage overlap measure/curve, average of AO w.r.t typical video seq. length
  * Typical video seq. length: sampled based on pdf, within the central interval of length 0.5
  * probability density computed by KDE
* EFO: equivalent filter operation, tracking speed

# GOT-10k
* AO: average overlap
* SR: success rate @ IoU=.5
* frame-pool(VOT-2017)
* Indicator for hard video selection:
  * Occlusion
  * Scale variation
  * Aspect ratio variation
  * Fast motion
  * Illumination variation
  * Small/Large objects
