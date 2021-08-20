# anonymous-for-my-double-blind-reivew
## A. Engineering Optimization for MBCT
We deploy the following engineering techniques to accelerate the running time and reduce the memory overhead: 

- **Data aggregation:** MBCT algorithm takes discrete binning features as input. If a feature is continuous, we convert it into the discrete one. So the combinations of the features are finite and we can map the samples into finite groups. This means we can aggregate the samples with the same feature group and use the average statistics of one group as a new sample to replace the samples in this group. The data aggregation can greatly reduce the use of computing resources.

- **Parallelization:** We follow parallelization method proposed by XGBoost for accelerating the running time of MBCT. In the process of constructing the calibration tree, we parallelize ***d*** processes at any level of the tree for selecting the optimal binning feature. This parallelization method can guarantee load balancing to the greatest extent. The times of run-time acceleration is the dimension of the feature. 

## B. Implementation details
We describe the implementation details in two folds as follows, to support the reproducibility of our framework. 
- **Feature Engineering.** 
  - CACTRDC: We extracted 8 features, of which 7 are discrete features from the train set, and 1 was 100-equal-width discretized CTR (click through rate).  
  - Proto Seguro: We select 7 discrete features from the train set, whose feature importance produced by the GBDT model is top 7 among all discrete features. We also take the 100-equal-width discretized predicted value as a calibration feature. 
  - Avazu: As same to Proto Seguro, we select 8 discrete features from the train set.
- **Online Deployment.** In order to meet the stability and latency requirements of online service, we could transform the trained MBCT model into a systematic "if...then..." rules, which benefits a lot from the interpretability of tree model. Then, we used a high-speed cache to store these "rules" for online deployment. We train and update our MBCT model frequently to ensure real-time calibration, promoting business value for the online advertising platforms.
