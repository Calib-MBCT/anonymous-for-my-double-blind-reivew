# anonymous-for-my-double-blind-reivew
## A. Engineering Optimization for MBCT
We deploy the following engineering techniques to accelerate the running time and reduce the memory overhead: 

- **Data aggregation:** MBCT algorithm takes discrete binning features as input. If a feature is continuous, we convert it into the discrete one. So the combinations of the features are finite and we can map the samples into finite groups. This means we can aggregate the samples with the same feature group and use the average statistics of one group as a new sample to replace the samples in this group. The data aggregation can greatly reduce the use of computing resources.

- **Parallelization:** We follow parallelization method proposed by XGBoost for accelerating the running time of MBCT. In the process of constructing the calibration tree, we parallelize ***d*** processes at any level of the tree for selecting the optimal binning feature. Where ***d*** is the number of features. This parallelization method can guarantee load balancing to the greatest extent. The times of run-time acceleration is the dimension of the feature. 

## B. Implementation details
We describe the implementation details in two folds as follows, to support the reproducibility of our framework. 
- **Feature Engineering.** 
  - CACTRDC: We extracted 8 features, of which 7 are discrete features from the train set, and 1 was 100-equal-width discretized CTR (click through rate).  
  - Proto Seguro: We select 7 discrete features from the train set, whose feature importance produced by the GBDT model is top 7 among all discrete features. We also take the 100-equal-width discretized predicted value as a calibration feature. 
  - Avazu: As same to Proto Seguro, we select 8 discrete features from the train set.
- **Online Deployment.** In order to meet the stability and latency requirements of online service, we could transform the trained MBCT model into a systematic "if...then..." rules, which benefits a lot from the interpretability of tree model. Then, we used a high-speed cache to store these "rules" for online deployment. We train and update our MBCT model frequently to ensure real-time calibration, promoting business value for the online advertising platforms.

## C. Additional experiments on CACTRDC dataset
### C1. The difference of MVCE scores in comparison with our MBCT
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/CACTRDC/baseline_bias.jpg" width="70%"/>
</div>

### C2. Ablation study of the training loss (in comparison with MVCE training)
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/CACTRDC/mbct_bias.jpg" width="70%"/>
</div>

### C3. PUD of finer-grained partitions in MBCT and His- togram Binning
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/CACTRDC/sample_bins.jpg" width="70%"/>
</div>

## D. Additional experiments on Proto Seguro dataset
### D1. The difference of MVCE scores in comparison with our MBCT
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/Porto_Seguro/baseline_bias.jpg" width="70%"/>
</div>

### D2. Ablation study of the training loss (in comparison with MVCE training)
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/Porto_Seguro/mbct_bias.jpg" width="70%"/>
</div>

### D3. PUD of finer-grained partitions in MBCT and His- togram Binning
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/Porto_Seguro/sample_bins.jpg" width="70%"/>
</div>

## E. Additional simulation results on calibration metrics
Here we show more simulation results for different settings:

### E1. h(x)~Beta(0.4, 0.7) and and E[Y|h(X)=c]=c^2
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/simulation/mvce_1.jpg" width="70%"/>
</div>
Main simulation results of ECE, ECEsweep and MVCE. The bin numbers of ECE and MVCE are set as 32. The bin number of ECEsweep is determined by itself.

 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/simulation/mvce_1_sn.jpg" width="70%"/>
</div>
TCE bias of MVCE and ECE under different numbers of bins and samples.

### E2. h(x)~Beta(0.6, 0.7) and and E[Y|h(X)=c]=c^2
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/simulation/mvce_2.jpg" width="70%"/>
</div>
Main simulation results of ECE, ECEsweep and MVCE. The bin numbers of ECE and MVCE are set as 32. The bin number of ECEsweep is determined by itself.

 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/simulation/mvce_2_sn.jpg" width="70%"/>
</div>
TCE bias of MVCE and ECE under different numbers of bins and samples.


### E3. h(x)~Beta(0.4, 0.7) and and E[Y|h(X)=c]=c^3
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/simulation/mvce_3.jpg" width="70%"/>
</div>
Main simulation results of ECE, ECEsweep and MVCE. The bin numbers of ECE and MVCE are set as 32. The bin number of ECEsweep is determined by itself.

 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/simulation/mvce_3_sn.jpg" width="70%"/>
</div>
TCE bias of MVCE and ECE under different numbers of bins and samples.

### E4. h(x)~Beta(0.6, 0.7) and and E[Y|h(X)=c]=c^3
 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/simulation/mvce_4.jpg" width="70%"/>
</div>
Main simulation results of ECE, ECEsweep and MVCE. The bin numbers of ECE and MVCE are set as 32. The bin number of ECEsweep is determined by itself.

 <div align=center><img src="https://github.com/Calib-MBCT/anonymous-for-my-double-blind-reivew/blob/main/images/simulation/mvce_4_sn.jpg" width="70%"/>
</div>
TCE bias of MVCE and ECE under different numbers of bins and samples.
