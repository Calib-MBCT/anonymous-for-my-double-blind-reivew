# anonymous-for-my-double-blind-reivew
## A. Engineering Optimization for MBCT
We deploy the following engineering techniques to accelerate the running time and reduce the memory overhead: 

**Data aggregation:** MBCT algorithm takes discrete binning features as input. If a feature is continuous, we convert it into the discrete one. So the combinations of the features are finite and we can map the samples into finite groups. This means we can aggregate the samples with the same feature group and use the average statistics of one group as a new sample to replace the samples in this group. The data aggregation can greatly reduce the use of computing resources.

**Parallelization:** We follow parallelization method proposed by \newcite{chen2016xgboost} for accelerating the running time of MBCT. In the process of constructing the calibration tree, we parallelize $|feature|$ processes at any level of the tree for selecting the optimal binning feature. This parallelization method can guarantee load balancing to the greatest extent\cite{chen2016xgboost}. The times of run-time acceleration is the dimension of the feature. 

## B. Implementation details
We describe the implementation details in two folds as follows, to support the reproducibility of our framework. 
- **Feature Construction.** For Porto Seguro, the total number of origin features is 57, among that 31 featrues are discrete and 26 are continuous. All the 31 discrete features are directly applied for MBCT training. The original estimated probabilities on the data samples are estimated by a finetune GBDT model, and then fed into the MBCT calibrator discretely as a new feature. For Avazu, all of the 21 original features are discrete. We choose 12 features as inputs for training the MBCT. The original estimated probabilities are processed as we described in Porto Seguro. For CACTRDC, the dataset contains 7 features, which are discrete attribute IDs of online ad requests. Different from above two datasets, we employ an advanced DNN model to predict original estimated probabilities, and then feed it into MBCT as a new feature.
- **Online Deployment.** In order to meet the stability and latency requirements of online service, we could transform the trained MBCT model into a systematic "if...then..." rules, which benefits a lot from the interpretability of tree model. Then, we used a high-speed cache to store these "rules" for online deployment. We train and update our MBCT model frequently to ensure real-time calibration, promoting business value for the online advertising platforms.

## C. Multiple Boosting Calibration Trees
Algorithm~\ref{alg:mbct} shows the procedure of MBCT in detail, in which $loss$ represents the MVCE. In our experiment, $max\_depth$ is set to the same as the size of feature set, maximum tree number $N$ is set to 10. We set $alpha$ to 0.05 and $inv\_len$ to 0.1. 



